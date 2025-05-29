---
title: Shepherd DockerHub Autentifikacija su Swarm Sąranka
nav_order: 3
has_children: true
layout: default
siteNav: true
---
{% include toc.md %}

# Shepherd DockerHub Autentifikacija su Swarm Sąranka

Šis gidas paaiškina, kaip sukonfigūruoti [containrrr/shepherd](https://github.com/containrrr/shepherd), kad Docker Swarm paslaugos būtų automatiškai atnaujinamos naudojant autentifikuotą prisijungimą prie DockerHub.

---
## Failų struktųra

Docker tag nustatysime v1. pvz:

```bash
docker build -t nginx1:v1  ./worker1/nginx
docker tag nginx1:v1  <your_repo>>/nginx1:v1
docker push  <your_repo>>/nginx1:v1
```


```
root@master-swarm:~/infra-stack#
.
├── docker-stack.yml
├── docker-stack.yml_bac
└── nginx
    └── nginx.conf
```
---

## 📦 Docker Compose Stack

```yaml
version: "3.8"

services:
  ngix1:
    image: TAVO_USER_NAME(ARBA_REPO)/nginx1:v1
    deploy:
      replicas: 2
      placement:
        constraints:
          - node.role == worker
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
      labels:
        - shepherd.enable=true
    networks:
      - app_net

  nginx:
    image: nginx:latest
    ports:
      - "8085:80"
    volumes:
      - /root/infra-stack/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    deploy:
      placement:
        constraints:
          - node.role == manager
      labels:
        - shepherd.enable=true
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

## 🔑 DockerHub `config.json` Autentifikacija

Sukurkite `~/.docker/config.json` naudodami:

```bash
docker login
cat ~/.docker/config.json
```

Tada sukurkite šį konfigą kaip Docker Swarm config:

```bash
docker config create shepherd-config-json ~/.docker/config.json
```

---

## 🌐 NGINX Konfigūracija

Išsaugokite šį failą kaip `/root/infra-stack/nginx/nginx.conf`:

```nginx
events {}

http {
    upstream backend {
        server ngix1:80;
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
        }
    }
}
```
### Paleisti service

```
docker stack deploy -c deploy-green-blue.yml  nginx
```
---

## 🔁 Pasirinktinai: Swarm CronJob Integracija

Norėdami Shepherd paleisti pagal grafiką (pvz., kas 5 minutes):

1. Įdiekite [swarm-cronjob](https://github.com/crazy-max/swarm-cronjob).
2. Pakeiskite Shepherd paslaugą:

```yaml
  shepherd:
    image: containrrr/shepherd
    environment:
      RUN_ONCE_AND_EXIT: "true"
    deploy:
      replicas: 0
      restart_policy:
        condition: none
    labels:
      - swarm.cronjob.enable=true
      - swarm.cronjob.schedule=*/5 * * * *
```


## 🐞 Trikčių šalinimas

* Jei matote:

  ```
  toomanyrequests: You have reached your unauthenticated pull rate limit.
  ```

  → Greičiausiai `config.json` nebuvo tinkamai prijungtas arba nebuvo sukurtas per `docker login`.

* Patikrinkite `config.json` konteineryje:

  ```bash
  docker exec -it <shepherd_container_id> cat /root/.docker/config.json
  ```

* Pridėkite `DEBUG: "true"` Shepherd paslaugos aplinkoje, kad gautumėte daugiau logų.

---

## ✅ Santrauka

Ši sąranka užtikrina:

* Autentifikuotus DockerHub image pull'us, kad būtų išvengta rate limit.
* Automatinį paslaugų atnaujinimą naudojant Shepherd.
* NGINX reverse proxy nukreipimą į `ngix1` replikas.
* Galimybę naudoti `swarm-cronjob` suplanuotiems atnaujinimams.

---

## 📊 Architektūros Diagrama (ASCII)

```
+---------------------+
|   Docker Swarm      |
|                     |
|  +-------------+    |
|  |  Manager    |    |
|  |             |    |
|  | shepherd    |<---+ <== DockerHub (authed pull)
|  | nginx       |    |
|  +-------------+    |
|                     |
|  +-------------+    |
|  |  Worker     |    |
|  |             |    |
|  | ngix1 x 2   |<--- NGINX proxy_pass
|  +-------------+    |
+---------------------+
```

---
