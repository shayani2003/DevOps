# Amazon ECR (Elastic Container Registry)

## Introduction

Amazon ECR (Elastic Container Registry) is AWS's fully managed container image registry.

It is used to store, manage, and deploy Docker container images securely.

ECR is commonly used with:

```text
Docker

ECS

EKS

Jenkins

GitHub Actions
```

---

# What is ECR?

ECR is a container image repository service.

Think of it as:

```text
GitHub
    |
Source Code

ECR
    |
Docker Images
```

---

# Why ECR?

Without ECR:

```text
Build Docker Image
       |
Store Locally
       |
Cannot Share Easily
```

---

With ECR:

```text
Build Docker Image
       |
Push To ECR
       |
Deploy Anywhere
```

---

# ECR Architecture

```text
Developer
     |
Docker Build
     |
Docker Image
     |
Amazon ECR
     |
ECS / EKS / EC2
```

---

# Real World Example

Application:

```text
Spring Boot App
```

Build:

```text
Docker Image
```

Store:

```text
Amazon ECR
```

Deploy:

```text
Amazon EKS
```

---

# Repository

## What is an ECR Repository?

A repository stores Docker images.

---

# Example

Repository:

```text
ecommerce-app
```

Images:

```text
v1

v2

v3
```

---

# Architecture

```text
Repository
    |
+---+---+---+
|   |   |   |
v1  v2  v3
```

---

# Private Repository

Default repository type.

Only authorized users can access.

---

# Architecture

```text
IAM User
     |
ECR
     |
Private Images
```

---

# Public Repository

Allows public access.

---

# Example

```text
Public Docker Images
```

available to everyone.

---

# Docker Workflow with ECR

```text
Create Application
       |
Docker Build
       |
Docker Image
       |
Push To ECR
       |
Deploy
```

---

# Step 1: Create Repository

AWS Console:

```text
ECR
  |
Create Repository
```

---

Example:

```text
my-app
```

---

# Step 2: Build Docker Image

```bash
docker build -t my-app .
```

---

Result:

```text
Docker Image Created
```

---

# Step 3: Authenticate to ECR

```bash
aws ecr get-login-password \
--region ap-south-1 \
| docker login \
--username AWS \
--password-stdin \
<account-id>.dkr.ecr.ap-south-1.amazonaws.com
```

---

# Architecture

```text
AWS Credentials
      |
Authentication
      |
ECR Access
```

---

# Step 4: Tag Image

```bash
docker tag my-app:latest \
123456789.dkr.ecr.ap-south-1.amazonaws.com/my-app:latest
```

---

# Step 5: Push Image

```bash
docker push \
123456789.dkr.ecr.ap-south-1.amazonaws.com/my-app:latest
```

---

# Architecture

```text
Docker Image
      |
Push
      |
ECR Repository
```

---

# Step 6: Pull Image

```bash
docker pull \
123456789.dkr.ecr.ap-south-1.amazonaws.com/my-app:latest
```

---

# Workflow

```text
ECR
 |
Pull Image
 |
Docker Host
```

---

# Image Tags

Tags identify image versions.

---

Examples:

```text
latest

v1.0

v2.0

production
```

---

# Architecture

```text
Repository
     |
+----+----+
|         |
v1      v2
```

---

# Why Use Tags?

Allows:

```text
Version Control

Rollback

Deployment Tracking
```

---

# Image Scanning

## What is Image Scanning?

ECR can scan images for vulnerabilities.

---

# Architecture

```text
Docker Image
      |
ECR Scan
      |
Security Report
```

---

# Benefits

```text
Identify Vulnerabilities

Improve Security
```

---

# Lifecycle Policies

Automatically remove old images.

---

# Example

```text
Keep Latest 10 Images
```

Delete older images.

---

# Architecture

```text
Repository
     |
Lifecycle Policy
     |
Cleanup
```

---

# Benefits

```text
Storage Optimization

Cost Reduction
```

---

# Encryption

ECR encrypts images.

---

Options:

```text
AWS Managed Key

KMS Key
```

---

# Architecture

```text
Docker Image
      |
Encryption
      |
ECR
```

---

# IAM Integration

Controls access to repositories.

---

# Example

Allow:

```json
{
  "Effect": "Allow",
  "Action": [
    "ecr:GetDownloadUrlForLayer",
    "ecr:BatchGetImage"
  ]
}
```

---

# Architecture

```text
IAM Policy
     |
ECR Access
```

---

# Cross-Region Replication

Automatically copies images between regions.

---

# Architecture

```text
ECR
Mumbai
   |
Replication
   |
ECR
Virginia
```

---

# Benefits

```text
Disaster Recovery

Global Deployment
```

---

# ECR with ECS

```text
ECR
 |
ECS
 |
Containers
```

---

# Workflow

```text
Push Image
      |
ECR
      |
ECS Pulls Image
      |
Run Container
```

---

# ECR with EKS

Most common Kubernetes architecture.

---

# Architecture

```text
Developer
      |
Docker Build
      |
ECR
      |
EKS
      |
Pods
```

---

# Kubernetes Example

```yaml
containers:
- name: app
  image: 123456789.dkr.ecr.ap-south-1.amazonaws.com/app:v1
```

---

# ECR with Jenkins

Most common DevOps interview architecture.

---

# Workflow

```text
GitHub
   |
Jenkins
   |
Docker Build
   |
Push To ECR
   |
Deploy To EKS
```

---

# Complete CI/CD Pipeline

```text
Developer
      |
GitHub
      |
Webhook
      |
Jenkins
      |
Build
      |
Docker Build
      |
ECR
      |
EKS
```

---

# Real Production Example

E-Commerce Application

Workflow:

```text
Developer Commit
       |
GitHub
       |
Jenkins
       |
Docker Build
       |
ECR
       |
EKS
```

Benefits:

```text
Automation

Versioning

Scalability
```

---

# ECR Security Best Practices

## Use IAM Roles

Avoid access keys.

---

## Enable Image Scanning

Detect vulnerabilities.

---

## Enable Encryption

Protect stored images.

---

## Use Least Privilege

Restrict repository access.

---

# Advantages

## Fully Managed

No registry servers to manage.

---

## Secure

IAM integration.

---

## Scalable

Handles large numbers of images.

---

## Integrated

Works with ECS, EKS, and CI/CD tools.

---

# Interview Question

## What is Amazon ECR?

### Answer

Amazon ECR (Elastic Container Registry) is a fully managed AWS service used to store, manage, and distribute Docker container images securely.

---

# Interview Question

## Why Use ECR?

### Answer

ECR provides a secure, scalable, and highly available container image repository that integrates with AWS services such as ECS, EKS, IAM, and CI/CD pipelines.

---

# Interview Question

## Difference Between Docker Hub and ECR

### Answer

Docker Hub is a public and private container registry provided by Docker, while ECR is AWS's managed container registry with native AWS security and service integration.

---

# Interview Question

## How Does EKS Pull Images from ECR?

### Answer

EKS worker nodes or Pods use IAM permissions to authenticate with ECR and pull container images securely without storing credentials.

---

# Interview Question

## What are Lifecycle Policies?

### Answer

Lifecycle Policies automatically remove unused or old container images based on defined rules, helping optimize storage and reduce costs.

---

# Interview Question

## What is Image Scanning?

### Answer

Image Scanning analyzes container images for known vulnerabilities and security risks, helping organizations improve container security.

---

# Memory Trick

```text
Build
  |
Docker Image
  |
ECR
  |
ECS / EKS
```

Key Features:

```text
Repositories

Tags

Image Scanning

Lifecycle Policies

Encryption
```

---

# Summary

Amazon ECR provides:

```text
Container Image Storage

Security

Versioning

Scanning

AWS Integration
```

Architecture:

```text
Developer
     |
Docker Build
     |
ECR
     |
EKS / ECS
```

Amazon ECR is a critical service in modern containerized AWS environments and is frequently discussed in DevOps, Kubernetes, CI/CD, and Cloud interviews because it serves as the central registry for container images.
