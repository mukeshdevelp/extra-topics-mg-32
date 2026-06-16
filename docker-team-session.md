# Docker – Complete Learning Guide

## Table of Contents

1. [Containerization vs Virtualization](#1-containerization-vs-virtualization)
2. [Docker Architecture & Basic Commands](#2-docker-architecture--basic-commands)
3. [Docker Volumes](#3-docker-volumes)
4. [Docker Networking](#4-docker-networking)
5. [Dockerfile](#5-dockerfile)
6. [DockerHub](#6-dockerhub)
7. [Docker Commands](#7-docker-commands)

---

# 1. Containerization vs Virtualization

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

### Virtualization

Virtualization creates multiple **Virtual Machines (VMs)** on a single physical server using a **Hypervisor**.

Each VM contains:

* Guest Operating System
* Libraries
* Application
* Dependencies

Architecture:

```text
Physical Server
↓
Hypervisor
↓
VM1 VM2 VM3
↓
Guest OS
↓
Application
```

Examples:

* VMware
* VirtualBox
* Hyper-V

---

### Containerization

Containerization packages an application and all dependencies into isolated **Containers**.

Containers share the host OS kernel.

Architecture:

```text
Host OS
↓
Docker Engine
↓
Container1 Container2
↓
Application
```

Examples:

* Docker
* Podman
* Containerd

---

## Layman Explanation

Imagine one apartment building.

### Virtualization

Each tenant gets:

* Separate house
* Separate electricity
* Separate kitchen

Heavy and expensive.

### Containerization

Each tenant gets:

* Separate room
* Shared electricity and water

Lightweight and fast.

---

## Comparison

| Feature        | Virtualization | Containerization |
| -------------- | -------------- | ---------------- |
| OS             | Separate       | Shared           |
| Boot Time      | Minutes        | Seconds          |
| Performance    | Lower          | Higher           |
| Resource Usage | High           | Low              |
| Isolation      | Strong         | Moderate         |

---

# 2. Docker Architecture & Basic Commands

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Docker uses a client-server architecture.

Components:

### Docker Client

Accepts commands.

```bash
docker build
docker run
```

### Docker Daemon

Background service managing:

* Containers
* Images
* Networks
* Volumes

### Docker Registry

Stores images.

Example:

* DockerHub

Architecture:

```text
Docker Client
      ↓
Docker Daemon
      ↓
Docker Registry
```

---

## Layman Explanation

Think of food delivery:

Customer → Docker Client
Kitchen → Docker Daemon
Food Store → DockerHub

---

## Basic Commands

Check version:

```bash
docker version
```

Check daemon:

```bash
systemctl status docker
```

Run container:

```bash
docker run nginx
```

List containers:

```bash
docker ps
```

Stop:

```bash
docker stop CONTAINER_ID
```

Remove:

```bash
docker rm CONTAINER_ID
```

---

# 3. Docker Volumes

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Volumes persist data outside container lifecycle.

Container deleted → Data remains.

Create volume:

```bash
docker volume create my-volume
```

Attach:

```bash
docker run -v my-volume:/data nginx
```

View:

```bash
docker volume ls
```

---

## Types

### Named Volume

```bash
-v data:/app
```

### Bind Mount

```bash
-v /host:/container
```

### Anonymous Volume

```bash
-v /app
```

---

## Layman Explanation

Container = Rental house

Volume = Storage locker

Even if house is removed → locker data remains.

---

# 4. Docker Networking

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Docker networking enables communication.

List networks:

```bash
docker network ls
```

Inspect:

```bash
docker network inspect bridge
```

---

## Network Types

### Bridge

Default local communication.

```bash
docker network create my-net
```

---

### Host

Uses host directly.

```bash
docker run --network host nginx
```

---

### None

No networking.

---

### Overlay

Multi-host communication.

---

## Layman Explanation

Containers are houses.

Networking = Roads connecting houses.



---

# 5. Dockerfile (Detailed)

[⬆ Back to Table of Contents](#table-of-contents)

## What is Dockerfile?

### Technical Explanation

A **Dockerfile** is a text file containing instructions to automatically build a Docker Image.

Instead of manually creating containers and installing software every time, Dockerfile automates the entire build process.

Build Flow:

```text
Dockerfile
   ↓
docker build
   ↓
Docker Image
   ↓
docker run
   ↓
Container
```

---

## Layman Explanation

Think of a **Dockerfile as a cooking recipe**.

Recipe → Dockerfile
Ingredients → Source Code
Cooked Food → Docker Image
Eating Food → Running Container

---

## Example Dockerfile

```Dockerfile
FROM ubuntu:22.04

RUN apt update && apt install python3 -y

WORKDIR /app

COPY . .

ENV PORT=8000

EXPOSE 8000

CMD ["python3","app.py"]
```

Build:

```bash
docker build -t python-app:v1 .
```

Run:

```bash
docker run -p 8000:8000 python-app:v1
```

---

# Dockerfile Instructions Explained

---

## 1. FROM

### Purpose

Defines the base image.

Syntax:

```Dockerfile
FROM ubuntu
```

Example:

```Dockerfile
FROM python:3.12
```

Use Cases:

* Start application from OS image
* Use language runtime

Examples:

* Python app → python
* Java app → openjdk
* Node app → node

Difference:

| FROM        | Meaning          |
| ----------- | ---------------- |
| FROM ubuntu | Base OS          |
| FROM python | Runtime included |

Rule:

* Dockerfile must begin with FROM.

---

## 2. RUN

### Purpose

Executes commands during image build.

Syntax:

```Dockerfile
RUN command
```

Example:

```Dockerfile
RUN apt update
```

Install packages:

```Dockerfile
RUN apt install nginx -y
```

Use Cases:

* Install packages
* Create directories
* Configure software

Difference:

| RUN                   | CMD                             |
| --------------------- | ------------------------------- |
| Executes during build | Executes during container start |

Example:

```Dockerfile
RUN mkdir /app
```

Folder becomes part of image.

---

## 3. COPY

### Purpose

Copies local files into image.

Syntax:

```Dockerfile
COPY source destination
```

Example:

```Dockerfile
COPY app.py /app
```

Use Cases:

* Copy application code
* Copy config files
* Copy scripts

Difference:

| COPY             | ADD                      |
| ---------------- | ------------------------ |
| Local files only | Local + URL + extraction |

Example:

```Dockerfile
COPY . .
```

Copies everything.

---

## 4. ADD

### Purpose

Advanced version of COPY.

Syntax:

```Dockerfile
ADD source destination
```

Example:

```Dockerfile
ADD app.tar.gz /app
```

Features:

* Downloads URLs
* Extract archives

Use Cases:

* Extract tar files
* Download remote resources

Difference:

| COPY      | ADD            |
| --------- | -------------- |
| Preferred | Extra features |

Recommendation:

```text
Use COPY unless ADD is necessary.
```

---

## 5. WORKDIR

### Purpose

Sets default working directory.

Syntax:

```Dockerfile
WORKDIR /app
```

Example:

```Dockerfile
WORKDIR /application
```

Use Cases:

* Avoid repeated cd commands
* Organize application files

Difference:

Without:

```Dockerfile
RUN cd app
```

With:

```Dockerfile
WORKDIR /app
```

All future commands run inside `/app`.

---

## 6. ENV

### Purpose

Set environment variables.

Syntax:

```Dockerfile
ENV KEY=value
```

Example:

```Dockerfile
ENV PORT=8080
```

Use Cases:

* Application config
* Secrets placeholders
* Runtime customization

Check:

```bash
echo $PORT
```

Difference:

| ENV     | ARG        |
| ------- | ---------- |
| Runtime | Build-time |

Example:

```Dockerfile
ENV APP_ENV=production
```

---

## 7. EXPOSE

### Purpose

Documents which port container uses.

Syntax:

```Dockerfile
EXPOSE 80
```

Example:

```Dockerfile
EXPOSE 3000
```

Use Cases:

* Web servers
* APIs

Difference:

| EXPOSE        | -p                |
| ------------- | ----------------- |
| Documentation | Actual publishing |

Example:

```bash
docker run -p 3000:3000 app
```

---

## 8. CMD

### Purpose

Default command executed when container starts.

Syntax:

```Dockerfile
CMD ["command"]
```

Example:

```Dockerfile
CMD ["nginx","-g","daemon off;"]
```

Use Cases:

* Start application
* Run services

Difference:

| RUN        | CMD     |
| ---------- | ------- |
| Build time | Runtime |

Only one CMD executes.

---

## 9. ENTRYPOINT

### Purpose

Defines fixed executable.

Example:

```Dockerfile
ENTRYPOINT ["python3"]
```

Run:

```bash
docker run app script.py
```

Executes:

```bash
python3 script.py
```

Use Cases:

* Standard startup
* Enforced execution

Difference:

| ENTRYPOINT | CMD         |
| ---------- | ----------- |
| Fixed      | Overridable |

---

## CMD vs ENTRYPOINT

Dockerfile:

```Dockerfile
ENTRYPOINT ["python3"]

CMD ["app.py"]
```

Run:

```bash
docker run image
```

Executes:

```bash
python3 app.py
```

Override:

```bash
docker run image test.py
```

Executes:

```bash
python3 test.py
```

---

## Multi-stage Docker Build

Example:

```Dockerfile
FROM node:20 AS builder

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build


FROM nginx

COPY --from=builder /app/dist /usr/share/nginx/html
```

Benefits:

* Smaller images
* Faster deployment
* Better security

---

## Dockerfile Best Practices

### Use lightweight images

```Dockerfile
FROM alpine
```

### Reduce layers

Bad:

```Dockerfile
RUN apt update
RUN apt install git
```

Good:

```Dockerfile
RUN apt update && apt install git -y
```

### Ignore unnecessary files

Create:

```text
.dockerignore
```

Example:

```text
node_modules
.git
```



---

# 6. DockerHub

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

DockerHub is a public/private registry.

Functions:

* Store images
* Share images
* Pull images

Login:

```bash
docker login
```

Pull:

```bash
docker pull nginx
```

Push:

```bash
docker push username/app:v1
```

---

## Layman Explanation

DockerHub = Cloud storage for container images.

Like:

* Google Drive
* GitHub

but for Docker Images.

---

# 7. Docker Commands

[⬆ Back to Table of Contents](#table-of-contents)

## Frequently Used Commands

Check images:

```bash
docker images
```

Pull image:

```bash
docker pull ubuntu
```

Run:

```bash
docker run ubuntu
```

Run detached:

```bash
docker run -d nginx
```

Interactive:

```bash
docker run -it ubuntu bash
```

Logs:

```bash
docker logs container
```

Exec:

```bash
docker exec -it container bash
```

Remove:

```bash
docker rm container
```

Delete image:

```bash
docker rmi image
```

Cleanup:

```bash
docker system prune
```

---

## Layman Explanation

| Command     | Meaning      |
| ----------- | ------------ |
| docker pull | Download     |
| docker run  | Start        |
| docker ps   | Show running |
| docker stop | Stop         |
| docker rm   | Delete       |
| docker logs | View logs    |

---


