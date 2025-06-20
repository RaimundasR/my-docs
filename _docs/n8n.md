---
title: n8n diegimas k8s
nav_order: 1
has_children: true
layout: default
siteNav: true
---
{% include toc.md %}

# 🚀 Deploying n8n in Kubernetes with Ingress, TLS, and Basic Auth

This guide explains how to deploy **n8n** using the `community-charts/n8n` Helm chart in a Kubernetes cluster with the following:

* Custom domain: `n8n.devplay.art`
* TLS via Let's Encrypt using `cert-manager`
* Basic Authentication via NGINX ingress annotations
* Rate limiting
* PostgreSQL as the backend database

---

## 📦 Prerequisites

* A running Kubernetes cluster
* `kubectl` configured
* `helm` installed
* A working Ingress controller (e.g., NGINX)
* A public domain pointed to your Ingress controller
* Optional: Cloudflare or DNS provider setup for `n8n.devplay.art`

---

## 1. 🔐 Install Cert-Manager

> This sets up automatic certificate provisioning using Let's Encrypt.

Install cert-manager via Helm:

```bash
kubectl create namespace cert-manager

helm repo add jetstack https://charts.jetstack.io
helm repo update

helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.14.5 \
  --set installCRDs=true
```

Wait until all cert-manager pods are ready:

```bash
kubectl get pods -n cert-manager
```

Create a ClusterIssuer for Let's Encrypt:

```yaml
# letsencrypt-prod.yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: you@example.com
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
      - http01:
          ingress:
            class: nginx
```

Apply it:

```bash
kubectl apply -f letsencrypt-prod.yaml
```

---

## 2. ✏️ Create `values.yaml`

```yaml
# values.yaml

ingress:
  enabled: true
  className: "nginx"
  annotations:
    nginx.ingress.kubernetes.io/limit-rps: "10"
    nginx.ingress.kubernetes.io/limit-burst-multiplier: "5"
    nginx.ingress.kubernetes.io/limit-connections: "20"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    nginx.ingress.kubernetes.io/auth-type: "basic"
    nginx.ingress.kubernetes.io/auth-secret: "n8n-basic-auth"
    nginx.ingress.kubernetes.io/auth-realm: "Authentication Required - n8n"
  hosts:
    - host: n8n.devplay.art
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
    - secretName: n8n-tls
      hosts:
        - n8n.devplay.art

postgresql:
  enabled: true
  auth:
    username: n8nuser
    password: n8npassword
    database: n8n

service:
  type: ClusterIP
  port: 5678
  name: http

webhook:
  mode: regular
  url: "https://n8n.devplay.art"
```

> Note: Replace `you@example.com` with your actual email in the ClusterIssuer.

---

## 3. 🔐 Create Basic Auth Secret

```bash
htpasswd -c auth n8nadmin
# Enter password when prompted
kubectl create secret generic n8n-basic-auth --from-file=auth -n n8n
```

---

## 4. 🚀 Install n8n with Helm

```bash
helm repo add community-charts https://community-charts.github.io/helm-charts
helm repo update

helm upgrade --install my-n8n community-charts/n8n \
  --version 1.8.5 \
  -n n8n \
  --create-namespace \
  -f values.yaml
```

---

## ✅ Verify

1. Wait for the pod to be ready:

   ```bash
   kubectl get pods -n n8n
   ```
2. Open [https://n8n.devplay.art](https://n8n.devplay.art) in your browser.
3. Enter your basic auth credentials.

---

## 🛠️ Optional Improvements

* Use GitHub OAuth instead of Basic Auth
* Restrict access to internal IP ranges only
* Add persistent volume storage for workflows/data

Let me know if you'd like to include these in your setup!
