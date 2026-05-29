# Dockerfile

## What is a Dockerfile?

A Dockerfile is a text file containing instructions that Docker uses to build an image automatically.

Think of it as a blueprint for creating Docker images.

---

## Why Do We Need a Dockerfile?

Without a Dockerfile:

- Install dependencies manually
- Configure environment manually
- Start application manually

With a Dockerfile:

- Everything is automated
- Consistent deployments
- Easier CI/CD integration

---

## Dockerfile Workflow

Dockerfile
     |
     v
docker build
     |
     v
Docker Image
     |
     v
docker run
     |
     v
Container

---

## Basic Dockerfile Structure

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

## Explanation of Each Instruction

### FROM

Defines the base image.

```dockerfile
FROM python:3.12-slim
```

Docker downloads the Python image.

---

### WORKDIR

Sets the working directory.

```dockerfile
WORKDIR /app
```

Equivalent to:

```bash
cd /app
```

---

### COPY

Copies files from local machine to image.

```dockerfile
COPY requirements.txt .
```

Copies:

```text
requirements.txt -> /app
```

---

### RUN

Executes commands during image build.

```dockerfile
RUN pip install -r requirements.txt
```

Installs dependencies.

---

### CMD

Default command executed when container starts.

```dockerfile
CMD ["python", "app.py"]
```

Runs the application.

---

## Complete Python Example

Project Structure:

```text
project/
│
├── app.py
├── requirements.txt
└── Dockerfile
```

app.py

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Docker is Working!"

app.run(host="0.0.0.0", port=5000)
```

requirements.txt

```text
flask
```

Dockerfile

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

## Build Docker Image

```bash
docker build -t flask-app .
```

Explanation:

- build → Create image
- -t → Tag image
- flask-app → Image name
- . → Current directory

---

## Check Images

```bash
docker images
```

---

## Run Container

```bash
docker run -p 5000:5000 flask-app
```

Explanation:

```text
Host Port     Container Port
5000    --->      5000
```

---

## View Running Containers

```bash
docker ps
```

---

## Stop Container

```bash
docker stop container_id
```

---

## Dockerfile Layers

Each instruction creates a layer.

Example:

```dockerfile
FROM python:3.12-slim
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
```

Layers:

Layer 1 → Base Image

Layer 2 → Copy requirements

Layer 3 → Install dependencies

Layer 4 → Copy source code

Diagram:

Layer 4  Source Code
--------------------
Layer 3  Dependencies
--------------------
Layer 2  Requirements
--------------------
Layer 1  Base Image

---

## Why Layers Are Important

Benefits:

- Faster builds
- Reuse cached layers
- Smaller updates

Example:

If only source code changes:

Docker reuses previous layers.

Only last layer rebuilds.

---

## Common Dockerfile Instructions

### ENV

Sets environment variables.

```dockerfile
ENV APP_ENV=production
```

---

### EXPOSE

Documents application port.

```dockerfile
EXPOSE 5000
```

---

### ENTRYPOINT

Defines main executable.

```dockerfile
ENTRYPOINT ["python"]
```

---

### CMD vs ENTRYPOINT

| CMD | ENTRYPOINT |
|-------|-----------|
| Default command | Fixed executable |
| Easily overridden | Difficult to override |

---

## Best Practices

### Use Small Base Images

Good:

```dockerfile
FROM python:3.12-slim
```

Bad:

```dockerfile
FROM ubuntu
```

---

### Copy Requirements First

Good:

```dockerfile
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
```

Improves caching.

---

### Use .dockerignore

Example:

```text
.git
node_modules
__pycache__
.env
```

Prevents unnecessary files from entering image.

---

### Avoid Running as Root

Create non-root user.

```dockerfile
RUN useradd appuser

USER appuser
```

Improves security.

---

## Common Interview Questions

### What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build Docker images automatically.

---

### Difference Between RUN and CMD

RUN:
- Executes during image build

CMD:
- Executes when container starts

Example:

```dockerfile
RUN pip install flask

CMD ["python", "app.py"]
```

---

### Difference Between COPY and ADD

COPY:
- Copies files

ADD:
- Can extract archives and download URLs

Preferred:
- COPY

---

### Why Use python:slim Instead of python?

- Smaller image size
- Faster download
- Better security
- Reduced attack surface

---

## Real Interview Answer

"A Dockerfile is a blueprint used to build Docker images. It contains instructions such as FROM, COPY, RUN, and CMD. Docker executes these instructions layer by layer to create an image. Using Dockerfiles ensures consistent and repeatable deployments across environments."

---

## Summary

- Dockerfile creates Docker images.
- FROM defines base image.
- COPY transfers files.
- RUN executes build-time commands.
- CMD starts the application.
- Layer caching improves build performance.
