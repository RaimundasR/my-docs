---

title: Praktinių darbų su swarm ir dockerhub diagramos 
nav\_order: 6
layout: default
siteNav: true
-------------

## Praktinių darbų ASCII tipo diagramos

---

### 1 praktinis darbas – Shepherd DockerHub Autentifikacija su Swarm Sąranka

```
+---------------------+
|      DockerHub      |
+----------+----------+
           |
           v
    +-------------+         +-----------------+
    | config.json |-------> | Docker Swarm    |
    +-------------+         | (manager+worker)|
           |                +-----------------+
           v
     +----------+  +----------+  +----------+
     |  ngix1   |  |  nginx   |  | shepherd |
     +----------+  +----------+  +----------+
```

---

### 2 praktinis darbas – Blue-Green Strategija su Shepherd ir NGINX

```
+---------------------+
|  Projektas: infra-  |
|    blue-green       |
+----------+----------+
           |
           v
    +------------------+
    |  nginx.conf      |
    |  (proxy logic)   |
    +---------+--------+
              |
              v
     +------------------+              +----------------+
     |  nginx-blue      | <---+        |  nginx-green   |
     | (active service) |     |        | (backup)       |
     +------------------+     |        +----------------+
                              |
                         +----v----+
                         | Shepherd|
                         +---------+
```

---

### 3 praktinis darbas – Docker Hub Cloud Build su GitHub Actions

```
+---------+       +-----------------+        +------------+
|  main   | ----> | GitHub Actions  | -----> | DockerHub  |
+---------+       +-----------------+        +------------+
```

---

### 4 praktinis darbas – Savarankiška CI/CD Užduotis su jūsų aplikacija

```
+-----------------+        +-------------+
| feature/branch  |        | main branch |
+--------+--------+        +------+------+
         |                        |
         v                        v
    +----------+           +-------------+
    | git lint |<--------- |    merge    |
    +----------+           +-------------+
         |
         v
  +----------------+        +-------------+
  | DockerHub Build| -----> |  Shepherd   |
  +----------------+        +------+------+    
                                    |
                     +--------------v------------+
                     | nginx-blue / nginx-green |
                     +--------------------------+
```
