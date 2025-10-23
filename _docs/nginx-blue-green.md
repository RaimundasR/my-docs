---

title: Blue-Green Strategija su Shepherd ir NGINX Docker Swarm'e
nav_order: 4
layout: default
siteNav: true
-------------

{% include toc.md %}

# Blue-Green Strategija su Shepherd ir NGINX Docker Swarm'e

Šiame skyriuje aprašomas būdas, kaip pakeisti tradicinę "round-robin" atnaujinimo strategiją į **aktyvaus–pasyvaus (blue-green)** modelį naudojant `containrrr/shepherd`, `Docker Swarm` ir `NGINX` kaip reverse proxy.

---
## Failų struktųra
```
root@master-swarm:~/infra-green-blue# tree
.
├── deploy-green-blue.yml
└── nginx
    └── nginx.conf
```

## 🎯 Tikslas

Užtikrinti, kad:

* Vienas konteineris **visada aptarnauja srautą**.
* Kitas konteineris yra **atsarginis**, atnaujinamas Shepherd pagalba.
* Srautas perjungiamas tik tada, kai atnaujintas konteineris jau pilnai pasiruošęs.

---

## 🔧 1. Blue-Green Paslaugų Struktūra

Po šio diegimo galite stebėti Shepherd logus komanda:

```bash
docker stack deploy -c deploy-green-blue.yml nginx

docker service logs nginx_shepherd -f
```

Pakeiskite `docker-compose.yml`, vietoje `ngix1` naudokite dvi atskiras paslaugas – `nginx-blue` ir `nginx-green`:

```yaml
version: "3.8"

services:
  nginx-blue:
    image: TAVO_USER_NAME/nginx1:v1
    deploy:
      replicas: 1
      placement:
        constraints:
          - node.role == worker
      labels:
        - shepherd.enable=true
    networks:
      - app_net

  nginx-green:
    image: TAVO_USER_NAME/nginx1:v1
    deploy:
      replicas: 1
      placement:
        constraints:
          - node.role == worker
      labels:
        - shepherd.enable=true
    networks:
      - app_net

  nginx:
    image: nginx:latest
    ports:
      - "8085:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    deploy:
      placement:
        constraints:
          - node.role == manager
    networks:
      - app_net

  shepherd:
    image: containrrr/shepherd
    environment:
      SLEEP_TIME: 1m
      WITH_REGISTRY_AUTH: "true"
      DEBUG: "true"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    configs:
      - source: shepherd-config-json
        target: /root/.docker/config.json
    deploy:
      placement:
        constraints:
          - node.role == manager
    networks:
      - app_net

configs:
  shepherd-config-json:
    external: true

networks:
  app_net:
    driver: overlay
```

---

## ⚙️ 2. `nginx.conf` su Pagrindiniu ir Atsarginiu Serveriu

Pakeiskite `nginx.conf` failą, kad NGINX naudotų vieną pagrindinį serverį ir vieną atsarginį. Naudokite `resolver` su Docker DNS:

```nginx
events {}

http {
    resolver 127.0.0.11 valid=10s;

    upstream backend {
        server nginx-blue:80 max_fails=1 fail_timeout=5s;
        server nginx-green:80 backup;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://backend;
            proxy_http_version 1.1;
            proxy_set_header Host $host;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            proxy_next_upstream error timeout http_502 http_504 http_500;
            proxy_next_upstream_tries 2;
        }
    }
}
```

### Paleisti service

```yml
docker stack deploy -c deploy-stack.yml  nginx
```
**Pastaba:** Vietoje `tasks.*` naudotas paslaugos vardas `nginx-blue`, `nginx-green` kartu su `resolver 127.0.0.11`, kuris užtikrina, kad NGINX visada naudoja tik aktyvius konteinerius, išvengiant `no route to host` po atnaujinimų.

---

## 🔁 3. Paslaugų Perjungimas Po Atnaujinimo

Kai `nginx-green` atnaujintas su nauja image versija ir veikia stabiliai, galite jį padaryti pagrindiniu:

1. Pakeiskite `nginx.conf`, kad `nginx-green` būtų pirmas:

```nginx
upstream backend {
    server nginx-green:80 max_fails=1 fail_timeout=5s;
    server nginx-blue:80 backup;
}
```

2. Perkraukite `nginx` paslaugą:

```bash
docker service update --force TAVO_STACK_pavadinimas_nginx
```

3. Tada atnaujinkite `nginx-blue`, kad jis taptų nauju atsarginiu konteineriu.

---

## ✅ Privalumai

* **Zero-downtime** atnaujinimai
* Vienas konteineris visada veikia kaip aktyvus
* Shepherd saugiai atnaujina pasyvų egzempliorių
* Galima ištestuoti naują versiją prieš perjungiant srautą

---

## ❓ Kodėl tai veikia su `ngix1`, bet ne su `tasks.nginx-blue`?

Kai naudoji `ngix1` kaip `server ngix1:80`, Swarm pateikia virtualų adresą, kuris rodo tik į **gyvus konteinerius** per savo built-in load balancer. Bet kai naudoji `tasks.nginx-blue`, NGINX bando jungtis prie **visų konkrečių container IP**, ir jei nors vienas dar nėra pilnai prisijungęs prie tinklo – gausi `no route to host`.

Kai naudoji `resolver 127.0.0.11` ir `server nginx-blue:80`, NGINX DNS užklausos sprendžiamos per Docker built-in DNS, kuris automatiškai atnaujina pasiekiamus konteinerius. Taip išvengiama `no route to host`, nes konteineriai DNS'e rodomi tik tada, kai yra tinkamai prisijungę prie Swarm tinklo.
