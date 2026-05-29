# Amazon ECS (Elastic Container Service)

## Introduction

Amazon ECS (Elastic Container Service) is AWS's fully managed container orchestration service.

It helps run, manage, and scale Docker containers without managing complex container infrastructure.

ECS is commonly used with:

```text
Docker

ECR

Fargate

EC2
```

---

# What is ECS?

Amazon ECS is a container orchestration platform.

It helps:

```text
Deploy Containers

Manage Containers

Scale Containers

Monitor Containers
```

---

# Why ECS?

Without ECS:

```text
Docker Container
       |
Manual Deployment
       |
Manual Scaling
       |
Manual Monitoring
```

---

With ECS:

```text
Docker Container
       |
ECS
       |
Automated Management
```

---

# ECS Architecture

```text
Developer
      |
Docker Image
      |
ECR
      |
ECS
      |
Running Containers
```

---

# Real World Example

Application:

```text
Spring Boot API
```

Dockerized:

```text
Docker Image
```

Stored:

```text
ECR
```

Deployed:

```text
ECS
```

---

# ECS Components

Main Components:

```text
Cluster

Task Definition

Task

Service

Container
```

---

# ECS Cluster

## What is a Cluster?

A logical grouping of resources where containers run.

---

# Architecture

```text
ECS Cluster
      |
+-----+-----+
|           |
Task      Task
```

---

# Example

Cluster Name:

```text
production-cluster
```

---

# Benefits

```text
Resource Organization

Container Management
```

---

# Task Definition

## What is a Task Definition?

A blueprint for running containers.

Similar to:

```text
Pod Definition in Kubernetes
```

---

# Contains

```text
Container Image

CPU

Memory

Ports

Environment Variables
```

---

# Architecture

```text
Task Definition
       |
Container Configuration
```

---

# Example

```json
{
  "family": "my-app",
  "containerDefinitions": [
    {
      "name": "app",
      "image": "my-app:v1"
    }
  ]
}
```

---

# Task

## What is a Task?

A running instance of a Task Definition.

---

# Architecture

```text
Task Definition
       |
Run
       |
Task
```

---

# Example

```text
Task Definition
       |
3 Running Tasks
```

---

# Service

## What is an ECS Service?

A Service ensures desired tasks are always running.

---

# Example

Desired:

```text
3 Tasks
```

---

If one crashes:

```text
ECS Creates New Task
```

Automatically.

---

# Architecture

```text
Service
   |
Desired Tasks = 3
   |
+---+---+---+
|   |   |   |
T1  T2  T3
```

---

# Container

Actual running Docker container.

---

# Workflow

```text
Docker Image
      |
Task
      |
Container
```

---

# ECS Launch Types

ECS supports:

```text
EC2 Launch Type

Fargate Launch Type
```

---

# EC2 Launch Type

Containers run on EC2 instances.

---

# Architecture

```text
ECS
 |
EC2
 |
Containers
```

---

# Workflow

```text
EC2 Created
      |
ECS Schedules Containers
```

---

# Benefits

```text
More Control

Custom Configuration
```

---

# Drawbacks

```text
Manage EC2 Yourself
```

---

# AWS Fargate

One of the most important interview topics.

---

# What is Fargate?

Serverless compute engine for containers.

No EC2 management required.

---

# Architecture

```text
Developer
      |
ECS
      |
Fargate
      |
Container
```

---

# Benefits

```text
No Server Management

Automatic Scaling

Pay Per Use
```

---

# Example

Without Fargate:

```text
Launch EC2
      |
Deploy Containers
```

---

With Fargate:

```text
Deploy Containers
```

AWS manages infrastructure.

---

# EC2 vs Fargate

| Feature | EC2 | Fargate |
|----------|------|---------|
| Server Management | Required | Not Required |
| Flexibility | High | Medium |
| Cost Control | Better | Simpler |
| Operations | More Work | Less Work |

---

# ECS Networking

Each task can receive networking configuration.

---

# Architecture

```text
VPC
 |
Subnet
 |
ECS Tasks
```

---

# Security Groups

Protect ECS Tasks.

---

# Architecture

```text
Internet
    |
Security Group
    |
ECS Task
```

---

# Load Balancer Integration

Common production architecture.

---

# Architecture

```text
Users
  |
ALB
  |
ECS Service
  |
Tasks
```

---

# Benefits

```text
Traffic Distribution

High Availability
```

---

# Auto Scaling

ECS supports automatic scaling.

---

# Example

CPU:

```text
80%
```

Action:

```text
Add Tasks
```

---

# Architecture

```text
CloudWatch
      |
Alarm
      |
Scale ECS Tasks
```

---

# ECS with ECR

Most common deployment flow.

---

# Architecture

```text
Docker Build
      |
ECR
      |
ECS
```

---

# Workflow

```text
Build Image
      |
Push To ECR
      |
Deploy To ECS
```

---

# ECS Deployment Types

## Rolling Update

Default deployment method.

---

# Workflow

```text
Old Tasks
      |
New Tasks Start
      |
Old Tasks Removed
```

---

# Benefits

```text
Minimal Downtime
```

---

# Blue-Green Deployment

Supported through AWS CodeDeploy.

---

# Architecture

```text
Blue Environment
      |
Green Environment
      |
Switch Traffic
```

---

# Monitoring

Monitor ECS using:

```text
CloudWatch

Container Insights
```

---

# Metrics

```text
CPU Usage

Memory Usage

Task Count
```

---

# ECS Security

Uses:

```text
IAM Roles

Security Groups

Task Roles
```

---

# Task Role

Allows containers to access AWS services securely.

---

# Example

```text
Container
      |
IAM Task Role
      |
S3 Access
```

---

# ECS vs Kubernetes

| Feature | ECS | Kubernetes |
|----------|-----|------------|
| Complexity | Lower | Higher |
| AWS Integration | Native | Good |
| Learning Curve | Easier | Harder |
| Flexibility | Lower | Higher |

---

# ECS vs EKS

Most common interview topic.

---

| Feature | ECS | EKS |
|----------|------|------|
| Orchestrator | AWS ECS | Kubernetes |
| Complexity | Easier | More Complex |
| Management | Simpler | More Flexible |
| Learning Curve | Low | High |

---

# Complete ECS Architecture

```text
Developer
      |
GitHub
      |
Jenkins
      |
Docker Build
      |
ECR
      |
ECS Service
      |
Tasks
```

---

# Real Production Example

E-Commerce API

Workflow:

```text
Developer Push
       |
Jenkins
       |
Docker Build
       |
ECR
       |
ECS
```

Benefits:

```text
Automation

Scalability

Availability
```

---

# Advantages

## Fully Managed

AWS manages orchestration.

---

## Scalable

Automatic scaling support.

---

## Integrated

Works seamlessly with AWS services.

---

## Secure

IAM and Security Group integration.

---

# Interview Question

## What is Amazon ECS?

### Answer

Amazon ECS (Elastic Container Service) is a fully managed container orchestration service that helps deploy, manage, and scale Docker containers on AWS.

---

# Interview Question

## What is a Task Definition?

### Answer

A Task Definition is a blueprint that defines how containers should run in ECS, including image, CPU, memory, ports, and environment variables.

---

# Interview Question

## What is the Difference Between a Task and a Service?

### Answer

A Task is a running instance of a Task Definition, while a Service ensures a specified number of Tasks remain running and healthy.

---

# Interview Question

## What is AWS Fargate?

### Answer

AWS Fargate is a serverless compute engine for containers that allows users to run containers without managing EC2 instances.

---

# Interview Question

## Difference Between ECS and EKS

### Answer

ECS is AWS's native container orchestration service and is simpler to use, while EKS is a managed Kubernetes service that provides greater flexibility and portability.

---

# Interview Question

## How Does ECS Pull Images?

### Answer

ECS typically pulls container images from Amazon ECR. When a Task starts, ECS retrieves the required image from the repository and launches the container.

---

# Memory Trick

```text
Task Definition
       |
Task
       |
Service
```

Launch Types:

```text
EC2

Fargate
```

Deployment Flow:

```text
Docker
  |
ECR
  |
ECS
```

---

# Summary

Amazon ECS provides:

```text
Container Orchestration

Scaling

Monitoring

Deployment Automation
```

Core Components:

```text
Cluster

Task Definition

Task

Service
```

Most Common Architecture:

```text
Jenkins
   |
Docker Build
   |
ECR
   |
ECS
```

Amazon ECS is a key AWS container service and is frequently asked in DevOps, Cloud, and Containerization interviews because it simplifies container deployment and management on AWS.
