---
title: Konteinerizacija ir jos valdymas užduotis
nav_order: 2
has_children: true
layout: default
siteNav: true
---
{% include toc.md %}

# 🧪 Praktinė užduotis: Docker Swarm klasterio kūrimas ir testavimas

## 🌟 Tikslas

Sukurti Docker Swarm klasterį, paskirstyti konteinerius tarp VM, sukonfigūruoti `overlay` tinklą 

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
docker swarm init
```

(Išves „join“ komandą su token'u – išsaugok ją.)

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

### Sukurk du `Dockerfile`:

**worker1/nginx/index.html** su tekstu `nginx 1 from worker node 1`
**worker2/nginx/index.html** su tekstu `nginx 2 from worker node 2`

Tada build:

```bash
docker build -t nginx1 ./worker1
docker build -t nginx2 ./worker2
```

---

## 5️⃣ Servisų paleidimas

### Worker 1:

```bash
docker service create \
  --name nginx1 \
  --replicas 2 \
  --network my_net \
  --constraint 'node.hostname==worker1' \
  nginx1
```

### Worker 2:

```bash
docker service create \
  --name nginx2 \
  --replicas 2 \
  --network my_net \
  --constraint 'node.hostname==worker2' \
  nginx2
```

---

<!-- ## 6️⃣ Load balancer (NGINX)

Master node:

1. Įidiek NGINX:

```bash
sudo apt install nginx -y
```

2. Konfigūruok `/etc/nginx/nginx.conf` (arba `default.conf`):

```nginx
http {
  upstream node1 {
    server nginx1:80;
  }
  upstream node2 {
    server nginx2:80;
  }

  server {
    listen 80;

    location /node1 {
      proxy_pass http://node1;
    }

    location /node2 {
      proxy_pass http://node2;
    }
  }
}
```

3. Paleisk arba perkrauk:

```bash
sudo systemctl restart nginx
```

---

## 7️⃣ Testavimas

Naršyklėje atidaryk:

* `http://<master_ip>/node1` → matysi nginx1 turinį
* `http://<master_ip>/node2` → matysi nginx2 turinį

Perkrovus puslapį, turinys gali kisti – veikia **round-robin load balancing** tarp replikų.

---

## ✅ Išvados

* Swarm leidžia lengvai kurti paslaugas su replikomis.
* Palaiko automatinį paskirstymą, atkūrimą ir tinklo izoliaciją.
* Puikiai tinka testavimui, mokymuisi ar mažiems klasteriams.

--- -->
