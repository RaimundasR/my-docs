---
title: Konteinerizacija ir jos valdymas užduotis
nav_order: 2
has_children: true
layout: default
siteNav: true
---
{% include toc.md %}

# 🧪 Praktinė užduotis: Docker Swarm klasterio kūrimas ir testavimas

## 🎯 Tikslas

Sukurti Docker Swarm klasterį, paskirstyti konteinerius tarp VM, sukonfigūruoti `overlay` tinklą ir patikrinti workde nodes.

---

## 1️⃣ VM paruošimas

Naudok DigitalOcean ar kitą platformą ir sukurk 3 VM su:

* OS: Ubuntu 22.04 LTS
* RAM: 4 GB
* Rolės:

  * **1x master node**
  * **2x worker node**

---

## 2️⃣ Docker diegimas

Diegimas visose VM:

```bash
sudo apt update && sudo apt install docker.io -y
```

### Master node:

```bash
docker swarm init --advertise-addr <TINKAMAS_IP>
```

> Jei gauni klaidą dėl kelių IP:
>
> ```
> Error response from daemon: could not choose an IP address to advertise...
> ```
>
> Nurodyk IP rankiniu būdu, pvz.:
>
> * Viešas IP: `docker swarm init --advertise-addr 134.209.242.xxx`
> * Vidinis IP (dažnai geriau): `docker swarm init --advertise-addr 10.19.0.x`
>
> Norėdamas rasti tinkamą IP:
>
> ```bash
> ip a | grep inet
> ```

### Worker nodes (abiejose):

```bash
docker swarm join --token <token> <master_ip>:2377
```

---

## 3️⃣ Overlay tinklo sukūrimas

Master node:

```bash
docker network create --driver overlay my_net
```

---

## 4️⃣ NGINX konteinerių paruošimas

### Sukurk struktūrą su failais:

```
project/
├── worker1/
│   └── nginx/
│       ├── index.html   # tekstas: nginx 1 from worker node 1
│       └── Dockerfile
└── worker2/
    └── nginx/
        ├── index.html   # tekstas: nginx 2 from worker node 2
        └── Dockerfile
```

### Dockerfile turinys (abiejuose aplankuose):

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

### Build'inimas master node:

```bash
docker build -t nginx1 ./worker1/nginx
docker build -t nginx2 ./worker2/nginx
```

---

## 5️⃣ Servisų paleidimas

> ⚠️ **Svarbu:** Kad Docker Swarm galėtų paleisti paslaugą iš `nginx1` ar `nginx2` image,
> šie image'ai turi būti pasiekiami **visiems node'ams**. Kadangi šiuo atveju jie sukurti
> tik master node, rekomenduojama:
>
> * Įkelti image į Docker Hub ar kitą registry:
>
> ```bash
> docker tag nginx1 tavo_vartotojas/nginx1
> docker push tavo_vartotojas/nginx1
> ```
>
> * Tada naudoti `tavo_vartotojas/nginx1` vietoj `nginx1` kuriant servisą.
>
> * Prieš tai prisijunk prie Docker Hub:
>
> ```bash
> docker login
> ```
>
> Sistema paprašys tavo naudotojo vardo ir slaptažodžio (arba personal access token, jei įjungtas 2FA).

### Worker 1

> ❗ Jei gauni klaidą:
>
> ```
> no suitable node (scheduling constraints not satisfied on 3 nodes)
> ```
>
> Patikrink:
>
> * Ar `node.hostname==worker-01` tiksliai atitinka node pavadinimą, kurį randi per `docker node ls`
> * Ar tinklas `my_net` egzistuoja ir pasiekiamas
> * Ar image prieinamas iš registry (pvz. Docker Hub)

> Jei jau buvai paleidęs paslaugą su tokiu pavadinimu ir ji nepavyko, ištrink ją:

```bash
docker service rm nginx1
```

> Tada paleisk servisą iš Docker Hub image:

```bash
docker service create \
  --name nginx1 \
  --replicas 2 \
  --network my_net \
  --constraint 'node.hostname==worker-01' \
  tavo_vartotojas(arba_projektas)/nginx1
```

### Worker 2:

```bash
docker service create \
  --name nginx2 \
  --replicas 2 \
  --network my_net \
  --constraint 'node.hostname==worker-02' \
  tavo_vartotojas(arba_projektas)/nginx2
```

---

## 6️⃣ Load balancer (NGINX)

Master node:

1. Įdiek NGINX:

```bash
sudo apt install nginx -y
```

2. Redaguok `/etc/nginx/sites-available/default` (ne `nginx.conf`):

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak
sudo rm /etc/nginx/sites-available/default
sudo nano /etc/nginx/sites-available/default
```

> Pakeisk **tik `server` bloką**, o ne visą failą. `http {}` blokas **neturi būti naudojamas** čia.

Rekomenduojamas turinys (jei Swarm serviso DNS neveikia, naudok IP vietoj pavadinimo):

```
upstream node1 {
  server worker-01:8080;
}

upstream node2 {
  server worker-02:8080;
}

server {
  listen 80;

  location /node1 {
    proxy_pass http://node1/;
  }

  location /node2 {
    proxy_pass http://node2/;
  }
}
```

3. Sukurk simbolinę nuorodą, jei reikia:

```bash
sudo ln -sf /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default
```

4. Patikrink konfigūraciją prieš perkrovimą:

```bash
sudo nginx -t
```

> ⚠️ Jei gauni klaidą `host not found in upstream`, tai reiškia, kad `nginx1` ir `nginx2` dar **nepasiekiami per DNS**.
> Šie pavadinimai turi būti **Swarm servisų pavadinimai**, o ne realūs hostname'ai.
> NGINX matys juos tik tada, kai bus paleisti servisai `nginx1` ir `nginx2` tame pačiame **Swarm overlay tinkle**.
>
> Laikinai gali:
>
> * Ištrinti `upstream` blokus ir naudoti tiesioginį `proxy_pass` su IP
> * Arba pirmiau paleisti abu servisus ir tik tada perkrauti NGINX

5. Jei sintaksė teisinga, perkrauk NGINX:

```bash
sudo systemctl restart nginx
```

---

## 7️⃣ Testavimas

> 💡 Jei `nginx1`/`nginx2` neatsako iš master VM (pvz. `curl nginx1:80` neveikia):
>
> * Tai **normalu**, nes `nginx1` yra Swarm serviso vardas, kuris rezolvuojamas **tik konteinerio viduje tame pačiame overlay tinkle**.
> * Bandymas pasiekti `nginx1` iš `host` OS neveiks, nes tai nėra DNS vardas / IP tavo VM sistemai.
>
> ✅ NGINX load balancer veiks tik tada, kai:
>
> * `nginx1` ir `nginx2` servisai jau veikia Swarm'e
> * Master NGINX konteineris prijungtas prie to paties `my_net` overlay tinklo

> 🔁 Alternatyva testavimui:
>
> * Naudok `docker exec -it <nginx-container> sh` ir testuok `curl nginx1` **viduje NGINX konteinerio**
> * Arba naudok `proxy_pass` tiesiai į IP adresus vietoj serviso vardų

Naršyklėje atidaryk:

* `http://<master_ip>/node1` → matysi nginx1 turinį
* `http://<master_ip>/node2` → matysi nginx2 turinį

Perkrovus puslapį, turinys gali kisti – veikia **round-robin load balancing** tarp replikų.

---

## ✅ Išvados

* Swarm leidžia lengvai kurti paslaugas su replikomis.
* Palaiko automatinį paskirstymą, atkūrimą ir tinklo izoliaciją.
* Puikiai tinka testavimui, mokymuisi ar mažiems klasteriams.

---


# 🧪 Užduotis: Sukurti ir paleisti dvi Vue.js Docker aplikacijas (`app1`, `app2`) bei pasiekti jas per NGINX reverse proxy

Šios užduoties tikslas — sukurti dvi skirtingas Vue.js aplikacijas, kiekvieną paleisti atskirame Docker konteineryje kaip Swarm `service`, ir per NGINX sukonfigūruoti jų pasiekiamumą per `/app1` ir `/app2` kelius naršyklėje.

---

## 📁 1. Projektų struktūra

```
projektas/
├── app1/
│   ├── Dockerfile
│   └── package.json
├── app2/
│   ├── Dockerfile
│   └── package.json
└── nginx/
    └── nginx.conf
```

---

# 🧪 Užduotis: Sukurti ir paleisti dvi Vue.js Docker aplikacijas (`app1`, `app2`) bei pasiekti jas per NGINX reverse proxy

Šios užduoties tikslas — sukurti dvi skirtingas Vue.js aplikacijas, kiekvieną paleisti atskirame Docker konteineryje kaip Swarm `service`, ir per NGINX sukonfigūruoti jų pasiekiamumą per `/app1` ir `/app2` kelius naršyklėje.

---

## 📁 1. Projektų struktūra

### 🛠 Privalomi failai kiekviename app kataloge

#### 1️⃣ Įsitikink, kad `app1/` (ir `app2/`) struktūra atrodo taip:

```
app1/
├── Dockerfile
├── package.json
├── public/
│   └── index.html
└── src/
    ├── App.vue
    └── main.js
```

#### 2️⃣ Sukurk minimalią `src/main.js` versiją:

```js
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

#### 3️⃣ `src/App.vue` turinys:

```vue
<template>
  <h1>Labas iš App 1 🎉</h1>
</template>

<script>
export default {
  name: 'App'
}
</script>
```

#### 4️⃣ Nepamiršk `public/index.html`:

```html
<!DOCTYPE html>
<html lang="lt">
<head>
  <meta charset="utf-8">
  <title>App1</title>
</head>
<body>
  <div id="app"></div>
</body>
</html>
```

> ✅ Įsitikink, kad kiekvienas `app1/` ir `app2/` katalogas turi būtinus failus, kad `npm run build` sėkmingai veiktų:
>
> * `src/main.js` – pagrindinis JS įėjimo taškas
> * `src/App.vue` – komponento failas
> * `public/index.html` – HTML šablonas

Pavyzdys:

```
app1/
├── Dockerfile
├── package.json
├── public/
│   └── index.html
└── src/
    ├── App.vue
    └── main.js
```

```
projektas/
├── app1/
│   ├── Dockerfile
│   └── package.json
├── app2/
│   ├── Dockerfile
│   └── package.json
```

---

## 📦 2. `package.json` turinys (`app1` ir `app2` kataloguose)

> 🧩 Šis failas būtinas, nes `docker build` vykdo `npm install` ir `npm run build`. Jis turi apibrėžti visas reikalingas priklausomybes ir `build` skriptą.(`app1` ir `app2` kataloguose)

> Nepamiršk į `App.vue` faile įrašyti skirtingą tekstą, kad aplikacijos skirtųsi.

```json
{
  "name": "vue-docker-app",
  "version": "1.0.0",
  "description": "Vue.js aplikacija Docker multi-stage aplinkai",
  "scripts": {
    "build": "vue-cli-service build"
  },
  "dependencies": {
    "vue": "^3.0.0"
  },
  "devDependencies": {
    "@vue/cli-service": "^5.0.0"
  }
}
```

---

## 🐳 3. `Dockerfile` turinys (`app1` ir `app2` kataloguose)

```dockerfile
# build etapas
FROM node:lts-alpine as build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# production etapas
FROM nginx:stable-alpine as production-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## 🔨 4. Docker image kūrimas iš `app1/` ir `app2/` katalogų

> 📝 **Pastaba**: `docker build` metu gali pasirodyti `npm WARN deprecated` žinutės dėl pasenusių priklausomybių. Tai **ne trukdžiai**, o tik įspėjimai. Jei reikia, priklausomybes galima atnaujinti vėliau.

### 🪟 Langas 1 – Sukurk image iš `app1/` katalogo:

```bash
docker build -t vue-app1 ./app1
```

### 🪟 Langas 2 – Sukurk image iš `app2/` katalogo:

```bash
docker build -t vue-app2 ./app2
```

---

## 🧪 5. Sukurti paslaugas Docker Swarm aplinkoje

### 🪟 Langas 1 – App 1:

```bash
docker service create \
  --name vue-app1 \
  --replicas 3 \
  --network my_net \
  --publish published=8083,target=80 \
  --constraint 'node.hostname==worker-01' \
  vue-app1
```

### 🪟 Langas 2 – App 2:

```bash
docker service create \
  --name vue-app2 \
  --replicas 3 \
  --network my_net \
  --publish published=8084,target=80 \
  --constraint 'node.hostname==worker-02' \
  vue-app2
```

---

## 🌐 5. NGINX konfigūracija su papildomais `upstream`

`nginx/nginx.conf` faile pridėk šiuos `upstream`:

```nginx
upstream node1 {
  server worker-01:8081;
}

upstream node2 {
  server worker-02:8080;
}

upstream app1 {
  server worker-01:8083;
}

upstream app2 {
  server worker-02:8084;
}

server {
  listen 80;

  location /node1 {
    proxy_pass http://node1/;
  }

  location /node2 {
    proxy_pass http://node2/;
  }

  location /app1 {
    proxy_pass http://app1/;
  }

  location /app2 {
    proxy_pass http://app2/;
  }
}
```

---

## ✅ 7. Patikrink naršyklėje

* [http://localhost/app1](http://localhost/app1) → turėtų rodyti App 1
* [http://localhost/app2](http://localhost/app2) → turėtų rodyti App 2

---

🎉 Viskas paruošta! Dvi Vue.js aplikacijos veikia atskiruose Docker konteineriuose kaip Swarm paslaugos, o jų pasiekiamumą reguliuoja NGINX proxy.
