---

title: Blue/Green Deploy naudojant GitLab Container Registry ir Shepherd
nav_order: 5
layout: default
siteNav: true
---

# 🐳 Blue/Green Deploy naudojant GitLab Container Registry ir Shepherd

## 🧩 Funkcionalumas

Automatinis Docker Swarm paslaugų atnaujinimas (blue/green pattern) naudojant:

* GitLab Container Registry kaip image šaltinį
* Shepherd kaip auto-puller įrankį, kuris tikrina ar image digest pasikeitė ir automatiškai restartina paslaugą

---

## 📦 Image saugojimas GitLab Registry

1. Įsitikink, kad tavo projektas turi įjungtą **Container Registry**:

   * **Settings → General → Visibility, project features, permissions → Container registry**

2. Image pavyzdys:

   ```
   registry.gitlab.com/tavo-vartotojas/tavo-projektas/nginx1:v1
   ```

---

## 🔐 Docker config.json su GitLab autentifikacija

1. Sukurk `config.json` failą su GitLab registry prisijungimu:

   ```bash
   mkdir -p docker-config
   echo '{
     "auths": {
       "registry.gitlab.com": {
         "username": "<CI_DEPLOY_USER>",
         "password": "<CI_DEPLOY_TOKEN>"
       }
     }
   }' > docker-config/config.json
   ```

2. Sukurk Docker Swarm config objektą:

   ```bash
   docker config create shepherd-config-json docker-config/config.json
   ```

---

## 🐳 Docker Compose (Swarm) su Shepherd

```yaml
version: "3.8"

services:
  nginx-blue:
    image: registry.gitlab.com/tavo-vartotojas/tavo-projektas/nginx1:v1
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
    image: registry.gitlab.com/tavo-vartotojas/tavo-projektas/nginx1:v1
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

## 🚀 Paleidimas

1. Deploy Docker stack:

   ```bash
   docker stack deploy -c docker-compose.yml tavo-stack
   ```

2. Tikrink Shepherd logus:

   ```bash
   docker service logs tavo-stack_shepherd
   ```

Jei image digest pasikeis (net jei tag tas pats), Shepherd automatiškai perkurs atitinkamą paslaugą.

---

## 🎯 Patarimas: Skirtingi tag'ai blue/green

Jei nori naudoti realų blue/green perjungimą, naudok:

```yaml
nginx-blue:
  image: registry.gitlab.com/tavo/nginx1:v1-blue

nginx-green:
  image: registry.gitlab.com/tavo/nginx1:v1-green
```

Tada `nginx.conf` galės srautą nukreipti į aktyvią versiją per `upstream`, o pakeitus nginx konfigūraciją — aktyvuoti naują versiją.

---

Paruošta tavo `blue-green` deployment'ui su GitLab Container Registry ir Shepherd 🐑🚀
