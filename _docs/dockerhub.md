---

title: Docker Hub Cloud Build su GitHub Actions
nav\_order: 5
layout: default
siteNav: true
-------------

{% include toc.md %}

# Docker Hub Cloud Build su GitHub Actions

Automatinis Docker image'ų kūrimas ir publikavimas į Docker Hub naudojant Docker Cloud Build ir GitHub Actions.

## ✨ Funkcionalumas

* Kai padaromas `push` į `main` šaką:

  * Sukuriamas Docker image per Docker Cloud Build (per `buildx` driver `cloud`)
  * Image įrašomas į Docker Hub su tag'u `nginx1:v1`

---

## 🔧 Reikalavimai

### GitHub Secrets & Variables

Eik į **GitHub repo → Settings → Secrets and variables → Actions** ir pridėk:

| Tipas    | Pavadinimas   | Reikšmė                          |
| -------- | ------------- | -------------------------------- |
| Variable | `DOCKER_USER` | Tavo Docker Hub vartotojo vardas |
| Secret   | `DOCKER_PAT`  | Docker Hub Personal Access Token |

### Docker Hub Cloud Build Endpoint

1. Prisijunk prie [Docker Hub](https://hub.docker.com)
2. Eik į **Builds → Cloud Builds**
3. Spausk **Create Build Endpoint**
4. Sukurk endpoint pavadinimu `test`

---

## 📁 Projekto struktūra

```
.
├── .github
│   └── workflows
│       └── ci.yml
└── nginx
    ├── Dockerfile
    └── index.html
```

### nginx/Dockerfile

```Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

### nginx/index.html

```html
<h1>...New test blue and green from docker hub cloud build</h1>
```

---

## ⚙️ GitHub Actions: `.github/workflows/ci.yml`

```yaml
name: ci

on:
  push:
    branches:
      - "main"

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKER_USER }}
          password: ${{ secrets.DOCKER_PAT }}

      - name: Set up Docker Buildx (Docker Hub Cloud Build)
        uses: docker/setup-buildx-action@v3
        with:
          driver: cloud
          endpoint: "raimundas0106/test"
          install: true

      - name: Build and push image
        uses: docker/build-push-action@v6
        with:
          context: ./nginx
          file: ./nginx/Dockerfile
          tags: ${{ vars.DOCKER_USER }}/nginx1:v1
          outputs: type=registry
          platforms: linux/amd64,linux/arm64
```

---

## ✅ Rezultatas

* Image automatiškai kuriamas per Docker Cloud Build
* Publikuojamas į tavo Docker Hub repo
* Veikia kiekvieną kartą, kai padaromas `push` į `main`

---

