# Docker Basics

## What is Docker?

Docker is a containerization platform that allows developers to package an application along with its dependencies into a container.

A container runs consistently across different environments.

---

## Why Docker is Used?

### 1. Consistent Environment

Works the same on:

- Developer machine
- Testing server
- Production server

### 2. Faster Deployment

Containers start within seconds.

### 3. Lightweight

Containers share the host OS kernel.

### 4. Easy Scaling

Multiple containers can be created quickly.

### 5. Portability

Run anywhere Docker is installed.

---

## Traditional Deployment vs Docker

### Traditional Deployment

Application
     |
Libraries
     |
Operating System

Problem:
Dependency conflicts may occur.

---

### Docker Deployment

+-------------------+
| Container         |
| Application       |
| Dependencies      |
+-------------------+

+-------------------+
| Container         |
| Application       |
| Dependencies      |
+-------------------+

        Host OS

Each application has its own isolated environment.

---

## Docker Architecture

                 Docker Client
                        |
                        |
                 Docker Daemon
                        |
       +----------------+----------------+
       |                                 |
 Docker Image                     Docker Container

### Components

#### Docker Client

Used to execute commands.

Example:

```bash
docker build
docker run
```

#### Docker Daemon

Background service responsible for:

- Building images
- Running containers
- Managing networks

#### Docker Image

Blueprint of a container.

#### Docker Container

Running instance of an image.

---

## Docker Lifecycle

Dockerfile
     |
     v
Docker Image
     |
     v
Docker Container
     |
     v
Running Application

---

## Image vs Container

| Docker Image | Docker Container |
|-------------|------------------|
| Template | Running instance |
| Read-only | Read-write |
| Static | Dynamic |

Example:

Image:

```text
nginx:latest
```

Container:

```bash
docker run nginx
```

---

## Real World Example

Suppose we have:

Python Application

Dependencies:

- Flask
- Requests
- Gunicorn

Without Docker:

Every server must install dependencies manually.

With Docker:

Everything is packaged inside the image.

Run:

```bash
docker run my-python-app
```

Application works immediately.

---

## Important Docker Terms

### Dockerfile

Instructions to build an image.

### Docker Image

Packaged application.

### Docker Container

Running application.

### Docker Hub

Public image repository.

### Registry

Storage location for images.

Examples:

- Docker Hub
- AWS ECR
- Azure ACR

---

## Common Docker Commands

### Check Version

```bash
docker --version
```

### Pull Image

```bash
docker pull nginx
```

### List Images

```bash
docker images
```

### Run Container

```bash
docker run nginx
```

### List Running Containers

```bash
docker ps
```

### List All Containers

```bash
docker ps -a
```

### Stop Container

```bash
docker stop container_id
```

### Remove Container

```bash
docker rm container_id
```

### Remove Image

```bash
docker rmi image_id
```

---

## Interview Questions

### What is Docker?

Docker is a containerization platform that packages applications and dependencies together, ensuring consistent execution across environments.

---

### Why Docker instead of Virtual Machines?

Docker containers:

- Lightweight
- Faster startup
- Less resource consumption
- Share host OS kernel

Virtual Machines:

- Require full operating system
- Consume more resources

---

## Docker vs Virtual Machine

+----------------------+
| Application          |
+----------------------+
| Guest OS             |
+----------------------+
| Hypervisor           |
+----------------------+
| Host OS              |
+----------------------+

Virtual Machine

-------------------------

+----------------------+
| Application          |
+----------------------+
| Docker Container     |
+----------------------+
| Docker Engine        |
+----------------------+
| Host OS              |
+----------------------+

Docker Container

Docker is more lightweight.

---

## Best Practices

- Use official images
- Keep images small
- Avoid running as root
- Use .dockerignore
- Scan images for vulnerabilities
- Use multi-stage builds

---

## Summary

Docker allows applications and dependencies to be packaged into lightweight containers, enabling portability, consistency, scalability, and faster deployments.
