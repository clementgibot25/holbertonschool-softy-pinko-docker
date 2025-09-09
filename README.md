# Docker and Proxy Concepts

## Table of Contents
- [Introduction to Docker](#introduction-to-docker)
- [Docker Concepts](#docker-concepts)
  - [Containers](#containers)
  - [Images](#images)
  - [Dockerfile](#dockerfile)
  - [Docker Compose](#docker-compose)
- [Getting Started with Docker](#getting-started-with-docker)
- [Proxy vs Reverse Proxy](#proxy-vs-reverse-proxy)
  - [Forward Proxy](#forward-proxy)
  - [Reverse Proxy](#reverse-proxy)
  - [Key Differences](#key-differences)
- [Practical Examples](#practical-examples)

## Introduction to Docker

Docker is a platform for developing, shipping, and running applications in containers. Containers allow you to package up an application with all of its dependencies into a standardized unit for software development.

## Docker Concepts

### Containers
A container is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries, and settings.

**Key characteristics:**
- Isolation: Each container runs in its own isolated environment
- Portability: Can run on any system with Docker installed
- Efficiency: Shares the host system's kernel
- Stateless: By default, containers are ephemeral

**Example:**
```bash
# Run an Nginx container
docker run -d -p 8080:80 --name my-nginx nginx
```

### Images
A Docker image is a read-only template with instructions for creating a Docker container. Images are built from a Dockerfile.

**Key concepts:**
- Immutable: Once created, images don't change
- Layered: Each instruction in a Dockerfile creates a new layer
- Cached: Unchanged layers are cached for faster builds

**Example:**
```bash
# Build an image from a Dockerfile
docker build -t my-app .

# List all images
docker images
```

### Dockerfile
A text document containing all the commands to assemble an image.

**Example Dockerfile:**
```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set the working directory
WORKDIR /app

# Copy the current directory contents into the container
COPY . .

# Install any needed packages
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

### Docker Compose
A tool for defining and running multi-container Docker applications.

**Example docker-compose.yml:**
```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
  redis:
    image: "redis:alpine"
```

## Getting Started with Docker

1. **Install Docker**
   - [Windows](https://docs.docker.com/desktop/windows/install/)
   - [macOS](https://docs.docker.com/desktop/mac/install/)
   - [Linux](https://docs.docker.com/engine/install/)

2. **Basic Commands**
   ```bash
   # Run a container
   docker run hello-world
   
   # List running containers
   docker ps
   
   # List all containers (including stopped)
   docker ps -a
   
   # Stop a container
   docker stop <container_id>
   
   # Remove a container
   docker rm <container_id>
   
   # Remove an image
   docker rmi <image_id>
   ```

## Proxy vs Reverse Proxy

### Forward Proxy
A forward proxy acts as an intermediary for requests from clients seeking resources from servers.

**Use cases:**
- Bypass content restrictions
- Anonymize web requests
- Cache frequently accessed content
- Monitor and filter traffic

**Example:**
```
Client <--> Forward Proxy <--> Internet
```

### Reverse Proxy
A reverse proxy is a server that sits in front of web servers and forwards client requests to those web servers.

**Use cases:**
- Load balancing
- SSL termination
- Caching
- Security and anonymity for backend servers

**Example:**
```
Client <--> Reverse Proxy <--> [Server1, Server2, Server3]
```

### Key Differences

| Feature          | Forward Proxy | Reverse Proxy |
|------------------|---------------|---------------|
| Position        | Client-side   | Server-side   |
| Hides           | Client IP     | Server IP     |
| Main Purpose    | Client privacy, bypass restrictions | Load balancing, security, caching |
| Client Awareness| Client must be configured to use proxy | Client is unaware of the proxy |

## Practical Examples

### Nginx as a Reverse Proxy
```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Docker Compose with Nginx Reverse Proxy
```yaml
version: '3.8'
services:
  web:
    build: .
    expose:
      - 3000
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - web
```

For more information, refer to the [Docker documentation](https://docs.docker.com/) and [Nginx documentation](https://nginx.org/en/docs/).
