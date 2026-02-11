# Two-Tier Flask App using Docker Bridge Network

## Project Overview

This project demonstrates a simple **Two-Tier Architecture** using:

- Flask (Application Tier)
- MySQL (Database Tier)
- Docker
- Docker Bridge Networking

The goal of this project was to understand:

- How Docker containers communicate
- How bridge networking works
- How environment variables are used
- How to connect an application container to a database container

---

## Architecture

Application Container (Flask)  
⬇  
Docker Bridge Network (two-tier)  
⬇  
MySQL Container  

Both containers are connected using a custom bridge network called `two-tier`.

---

## Docker Networking

A custom bridge network was created:

```bash
docker network create two-tier -d bridge
```

This allows:

- Container-to-container communication
- Internal DNS resolution using container names
- Network isolation from other containers

The Flask container connects to MySQL using:

```
MYSQL_HOST=mysql
```

Where `mysql` is the container name of the MySQL service.

---

## Setup Steps

### 1️. Create Bridge Network

```bash
docker network create two-tier -d bridge
```

---

### 2️. Run MySQL Container

```bash
docker run -d \
--name mysql \
--network two-tier \
-e MYSQL_ROOT_PASSWORD=nandini1234 \
-e MYSQL_DATABASE=devOps \
mysql:latest
```

After starting MySQL, a database named `devOps` was created inside the container.

---

### 3️. Build Flask Application Image

Dockerfile was created for the Flask app and image was built:

```bash
docker build -t two-tier-flask-app .
```

---

### 4️. Run Flask Container

```bash
docker run -d -p 5000:5000 \
--network two-tier \
-e MYSQL_HOST=mysql \
-e MYSQL_USER=root \
-e MYSQL_PASSWORD=nandini1234 \
-e MYSQL_DB=devOps \
two-tier-flask-app
```

---

##  How Communication Works

- Both containers are attached to the same bridge network.
- Docker provides internal DNS.
- The Flask container connects to MySQL using the hostname `mysql`.
- No IP address is required.
- Port 5000 is exposed only for the Flask application.

---

##  Concepts Learned

- Docker images vs containers
- Bridge networking
- Custom Docker networks
- Environment variables in containers
- MySQL container initialization
- Two-tier application architecture
- Container-to-container communication

---

##  Future Improvements

- Add Docker Compose
- Add volume for MySQL persistence
- Use non-root MySQL user
- Add health checks
- Deploy on cloud (AWS / EC2)

---

##  What is Two-Tier Architecture?

Two-tier architecture consists of:

1. Application layer
2. Database layer

The application directly communicates with the database.

---
