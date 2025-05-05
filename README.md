# Docker_learning
A basic docker learning command

# Complete Docker Guide — Beginner to Advanced

This `README.md` is a full-fledged Docker guide for your GitHub repository. It covers everything from installation to deploying real-world applications using Docker, including Dockerfile, Compose, volumes, networks, and Docker Hub usage.

---

## Table of Contents

1. [What is Docker?](#what-is-docker)
2. [Docker Architecture](#docker-architecture)
3. [Installing Docker](#installing-docker)
4. [Basic Docker Commands](#basic-docker-commands)
5. [Dockerfile — Custom Images](#dockerfile)
6. [Volumes — Data Persistence](#volumes)
7. [Networks — Communication Between Containers](#networks)
8. [Docker Compose — Multi-container Applications](#docker-compose)
9. [Docker Hub — Pushing Images](#docker-hub)
10. [Sample Project: Dockerizing Flask App](#dockerizing-a-flask-app)
11. [Advanced Topics](#advanced-topics)
12. [Clean-Up Commands](#clean-up)
13. [Troubleshooting Docker](#troubleshooting-docker)
14. [Resources](#resources)

---

## What is Docker?

Docker is an open-source platform that lets you build, package, ship, and run applications in isolated environments called containers. It ensures consistency across development, testing, and production.

---

## Docker Architecture

- **Docker Client**: CLI tool (`docker`) to interact with the Docker daemon  
- **Docker Daemon**: Manages containers/images  
- **Dockerfile**: Instructions to build custom images  
- **Docker Images**: Read-only templates to create containers  
- **Containers**: Running instances of Docker images  
- **Docker Hub**: Registry of Docker images  

---

## Installing Docker

### Ubuntu
```bash
sudo apt update
sudo apt install ca-certificates curl gnupg lsb-release -y
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

sudo systemctl start docker
sudo systemctl enable docker
docker --version
docker run hello-world
```

### Windows/macOS
- Download Docker Desktop: https://www.docker.com/products/docker-desktop  
- Install and launch Docker  
- Run `docker --version` to confirm  

---

## Basic Docker Commands

| Action                             | Command                                         |
|-----------------------------------|-------------------------------------------------|
| Run image                         | `docker run nginx`                              |
| Port mapping                      | `docker run -p 8080:80 nginx`                   |
| List running containers           | `docker ps`                                     |
| List all containers               | `docker ps -a`                                  |
| Stop a container                  | `docker stop <id>`                              |
| Remove a container                | `docker rm <id>`                                |
| Build image from Dockerfile       | `docker build -t myapp .`                       |
| List images                       | `docker images`                                 |
| Remove image                      | `docker rmi <id>`                               |
| View logs                         | `docker logs <id>`                              |
| Open shell                        | `docker exec -it <id> bash`                     |

---

## Dockerfile

```Dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

```bash
docker build -t my-node-app .
docker run -p 3000:3000 my-node-app
```

---

## Volumes

### Types of Volumes

| Type        | Description                                      |
|-------------|--------------------------------------------------|
| Volume      | Managed by Docker                                |
| Bind Mount  | Linked to host directory                         |
| Tmpfs       | Stored in memory only                            |

### Usage
```bash
docker volume create mydata
docker run -d -v mydata:/app/data ubuntu
```

### Inspect, Share & Prune
```bash
docker volume inspect mydata
docker volume rm mydata
docker volume prune
```

---

## Networks

### Types

| Network     | Description                                   |
|-------------|-----------------------------------------------|
| bridge      | Default, for container-to-container comm.     |
| host        | Uses host's network stack                     |
| overlay     | Multi-host in Swarm                           |
| macvlan     | Container gets MAC address                    |

### Create and Connect
```bash
docker network create mynet
docker run -it --name app --network mynet alpine
```

---

## Docker Compose

**docker-compose.yml**
```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: redis
```

```bash
docker-compose up -d
docker-compose down
```

---

## Docker Hub

```bash
docker login
docker tag myapp username/myapp
docker push username/myapp
docker pull username/myapp
```

---

## Dockerizing a Flask App

**app.py**
```python
from flask import Flask
app = Flask(__name__)
@app.route('/')
def home():
    return "Docker + Flask working!"
app.run(host="0.0.0.0", port=5000)
```

**Dockerfile**
```Dockerfile
FROM python:3.9
WORKDIR /app
COPY . .
RUN pip install flask
CMD ["python", "app.py"]
```

```bash
docker build -t flask-app .
docker run -p 5000:5000 flask-app
```

---

## Advanced Topics

### Save/Load Image
```bash
docker save -o app.tar myapp
docker load -i app.tar
```

### Multi-stage Builds, Swarm, Kubernetes
> For large-scale production, consider multi-stage builds and orchestration tools like Kubernetes or Docker Swarm.

---

## Clean-Up

```bash
docker system prune -a
```

---

## Troubleshooting Docker

| Problem                      | Solution                                     |
|-----------------------------|----------------------------------------------|
| Permission denied           | `sudo usermod -aG docker $USER && newgrp docker` |
| Docker not running          | `sudo systemctl start docker`               |
| Port conflict               | Change the port in `-p` flag                 |
| No space left               | `docker system prune`                        |

---

## Resources

- [Docker Docs](https://docs.docker.com/)
- [Docker Cheat Sheet](https://github.com/wsargent/docker-cheat-sheet)
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
