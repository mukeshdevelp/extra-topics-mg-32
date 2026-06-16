# Kubernetes & Docker – Complete Learning Guide

## Table of Contents

1. [Introduction to Kubernetes Deployment](#1-introduction-to-kubernetes-deployment)
2. [Introduction to Recreate Deployment Strategy](#2-introduction-to-recreate-deployment-strategy)
3. [Introduction to Rolling Updates Strategy](#3-introduction-to-rolling-updates-strategy)
4. [Introduction to Kubernetes Services](#4-introduction-to-kubernetes-services)
5. [Kubernetes Service Types](#5-kubernetes-service-types)
6. [Use Case of Labels & Selectors in Kubernetes](#6-use-case-of-labels--selectors-in-kubernetes)
7. [Kubernetes ClusterIP Service Overview](#7-kubernetes-clusterip-service-overview)
8. [Kubernetes NodePort Service Overview](#8-kubernetes-nodeport-service-overview)
9. [Kubernetes LoadBalancer Service Overview](#9-kubernetes-loadbalancer-service-overview)
10. [Kubernetes Pod Overview](#10-kubernetes-pod-overview)
11. [Introduction to Kubernetes ReplicaSets](#11-introduction-to-kubernetes-replicasets)
12. [Scaling with ReplicaSets](#12-scaling-with-replicasets)
13. [Docker Compose Setup & Basic Commands](#13-docker-compose-setup--basic-commands)
14. [Creating a Docker Compose YAML File](#14-creating-a-docker-compose-yaml-file)
15. [Introduction to Kubernetes Ingress Controller (Practical Demo)](#15-introduction-to-kubernetes-ingress-controller-practical-demo)
16. [Docker Compose Commands with Use Cases](#16-docker-compose-commands-with-use-cases)
17. [Docker Swarm vs Kubernetes](#17-docker-swarm-vs-kubernetes)
18. [Kubernetes Namespace](#18-kubernetes-namespace)


---

# 1. Introduction to Kubernetes Deployment

A **Deployment** in Kubernetes manages application deployment and ensures the desired number of replicas are running.

### Features

* Automated rollout and rollback
* Scaling applications
* Self-healing
* Version updates

### Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:latest
```

Apply:

```bash
kubectl apply -f deployment.yaml
```

Check:

```bash
kubectl get deployments
```

---

# 2. Introduction to Recreate Deployment Strategy

Recreate strategy terminates old pods before creating new pods.

### Flow

```text
Old Pods → Delete → New Pods Created
```

### Configuration

```yaml
strategy:
  type: Recreate
```

### Use Cases

* Applications requiring full shutdown
* Database schema changes

### Disadvantage

* Causes downtime

---

# 3. Introduction to Rolling Updates Strategy

Rolling updates replace pods gradually.

### Flow

```text
Old Pod → New Pod
Old Pod → New Pod
```

### Configuration

```yaml
strategy:
  type: RollingUpdate

  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
```

### Benefits

* Zero downtime
* Controlled deployment

Commands:

```bash
kubectl rollout status deployment nginx
kubectl rollout undo deployment nginx
```

---

# 4. Introduction to Kubernetes Services

A Service exposes pods internally or externally.

### Purpose

* Stable networking
* Load balancing
* Service discovery

Example:

```yaml
kind: Service
```

Check:

```bash
kubectl get svc
```

---

# 5. Kubernetes Service Types

### ClusterIP

Internal communication.

### NodePort

Expose service using node port.

### LoadBalancer

Expose externally via cloud LB.

### ExternalName

Maps to DNS.

---

# 6. Use Case of Labels & Selectors in Kubernetes

Labels organize Kubernetes objects.

Example:

```yaml
labels:
  app: frontend
```

Selector:

```yaml
selector:
  app: frontend
```

### Use Cases

* Connect service to pods
* Environment separation
* Monitoring

Check:

```bash
kubectl get pods --show-labels
```

---

# 7. Kubernetes ClusterIP Service Overview

Default service type.

### Example

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:
  type: ClusterIP

  selector:
    app: nginx

  ports:
  - port: 80
```

Access:

```bash
curl service-name
```

---

# 8. Kubernetes NodePort Service Overview

Expose application outside cluster.

Example:

```yaml
spec:
  type: NodePort

  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
```

Access:

```text
http://NodeIP:30080
```

---

# 9. Kubernetes LoadBalancer Service Overview

Creates cloud load balancer.

Example:

```yaml
spec:
  type: LoadBalancer
```

Check:

```bash
kubectl get svc
```

Access:

```text
EXTERNAL-IP
```

---

# 10. Kubernetes Pod Overview

Smallest deployable unit.

Pod can contain:

* One container
* Multiple containers

Example:

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx

spec:
  containers:
  - name: nginx
    image: nginx
```

Commands:

```bash
kubectl get pods
kubectl describe pod nginx
kubectl logs nginx
```

---

# 11. Introduction to Kubernetes ReplicaSets

ReplicaSet maintains desired pod count.

Example:

```yaml
apiVersion: apps/v1
kind: ReplicaSet

metadata:
  name: app-rs

spec:
  replicas: 3
```

Check:

```bash
kubectl get rs
```

---

# 12. Scaling with ReplicaSets

Increase replicas:

```bash
kubectl scale rs app-rs --replicas=5
```

Verify:

```bash
kubectl get pods
```

Benefits:

* High availability
* Load distribution

---

# 13. Docker Compose Setup & Basic Commands

Install:

```bash
sudo apt install docker-compose
```

Check:

```bash
docker compose version
```

Commands:

```bash
docker compose up
docker compose down
docker compose ps
docker compose logs
```

---

# 14. Creating a Docker Compose YAML File

Example:

```yaml
version: "3"

services:

  web:
    image: nginx

  redis:
    image: redis
```

Run:

```bash
docker compose up -d
```

Stop:

```bash
docker compose down
```

---

# 15. Introduction to Kubernetes Ingress Controller (Practical Demo)

Ingress manages HTTP/HTTPS routing.

Architecture:

```text
User
 ↓
Ingress
 ↓
Service
 ↓
Pods
```

Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: app

spec:
  rules:
  - host: demo.local
```

Apply:

```bash
kubectl apply -f ingress.yaml
```

Verify:

```bash
kubectl get ingress
```

---

# 16. Docker Compose Commands with Use Cases

Start containers:

```bash
docker compose up -d
```

Stop:

```bash
docker compose stop
```

Restart:

```bash
docker compose restart
```

View logs:

```bash
docker compose logs
```

Remove:

```bash
docker compose down
```

Scale:

```bash
docker compose up --scale web=3
```

---

# 17. Docker Swarm vs Kubernetes

| Feature          | Docker Swarm | Kubernetes        |
| ---------------- | ------------ | ----------------- |
| Setup            | Easy         | Complex           |
| Scaling          | Basic        | Advanced          |
| Networking       | Simple       | Advanced          |
| Auto-healing     | Yes          | Strong            |
| Ecosystem        | Smaller      | Large             |
| Production Usage | Moderate     | Industry Standard |

### Choose Docker Swarm

* Small deployments

### Choose Kubernetes

* Enterprise production

---

# 18. Kubernetes Namespace

Namespace isolates cluster resources.

Create:

```bash
kubectl create namespace dev
```

Deploy:

```bash
kubectl apply -f app.yaml -n dev
```

View:

```bash
kubectl get ns
```

Delete:

```bash
kubectl delete namespace dev
```

### Default Namespaces

* default
* kube-system
* kube-public
* kube-node-lease

---

