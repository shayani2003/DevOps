# Amazon EKS (Elastic Kubernetes Service)

## Introduction

Amazon EKS (Elastic Kubernetes Service) is AWS's managed Kubernetes service.

It allows organizations to run Kubernetes clusters without managing the Kubernetes control plane themselves.

EKS combines:

```text
Kubernetes
     +
AWS Infrastructure
```

---

# What is EKS?

Amazon EKS is a managed service that runs Kubernetes on AWS.

AWS manages:

```text
Control Plane

API Server

etcd

High Availability
```

while you manage:

```text
Applications

Pods

Deployments

Services
```

---

# Why EKS?

Without EKS:

```text
Install Kubernetes
       |
Configure Masters
       |
Manage etcd
       |
Handle Upgrades
       |
Manage Availability
```

Complex.

---

With EKS:

```text
Create Cluster
       |
Deploy Applications
```

AWS manages Kubernetes infrastructure.

---

# What is Kubernetes?

Kubernetes is a container orchestration platform.

Used to:

```text
Deploy Containers

Scale Containers

Manage Containers
```

---

# EKS Architecture

```text
Developer
      |
kubectl
      |
EKS Control Plane
      |
Worker Nodes
      |
Pods
```

---

# EKS Components

Main Components:

```text
Control Plane

Worker Nodes

Node Groups

Pods

Services

Namespaces
```

---

# Control Plane

## What is the Control Plane?

The brain of Kubernetes.

Managed by AWS.

---

# Components

```text
API Server

Scheduler

Controller Manager

etcd
```

---

# Architecture

```text
Control Plane
      |
+-----+-----+
|     |     |
API Scheduler etcd
```

---

# Responsibilities

```text
Cluster Management

Scheduling

State Management
```

---

# Worker Nodes

## What are Worker Nodes?

Machines where containers run.

---

# Architecture

```text
Control Plane
      |
Worker Node
      |
Pods
```

---

# Responsibilities

```text
Run Pods

Execute Containers

Provide Resources
```

---

# Worker Node Components

```text
Kubelet

Container Runtime

Kube Proxy
```

---

# Node Group

## What is a Node Group?

A collection of worker nodes.

---

# Architecture

```text
Node Group
      |
+-----+-----+
|           |
Node1     Node2
```

---

# Benefits

```text
Easy Scaling

Centralized Management
```

---

# Managed Node Groups

AWS manages:

```text
Provisioning

Updates

Scaling
```

---

# Self-Managed Nodes

User manages nodes manually.

---

# Comparison

| Feature | Managed | Self Managed |
|----------|----------|-------------|
| AWS Management | Yes | No |
| Updates | Automatic | Manual |
| Complexity | Lower | Higher |

---

# Pods

Smallest deployable unit in Kubernetes.

---

# Architecture

```text
Worker Node
      |
Pod
      |
Container
```

---

# Example

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

---

# Services

Expose Pods to users.

---

# Architecture

```text
User
 |
Service
 |
Pods
```

---

# Example

```text
ClusterIP

NodePort

LoadBalancer
```

---

# Namespaces

Used to logically separate resources.

---

# Example

```text
production

development

testing
```

---

# Architecture

```text
Cluster
   |
Namespaces
   |
Resources
```

---

# EKS Networking

Uses Amazon VPC.

---

# Architecture

```text
VPC
 |
Subnets
 |
Worker Nodes
 |
Pods
```

---

# Public and Private Subnets

Typical production architecture.

---

# Architecture

```text
Public Subnet
      |
Load Balancer

Private Subnet
      |
Worker Nodes
```

---

# Security

EKS integrates with:

```text
IAM

Security Groups

VPC
```

---

# IAM Authentication

Users authenticate using IAM.

---

# Architecture

```text
IAM User
      |
EKS Cluster
```

---

# IAM Roles for Service Accounts (IRSA)

One of the most important EKS interview topics.

---

# What is IRSA?

Allows Pods to access AWS services securely.

---

Without IRSA:

```text
Node Role
      |
All Pods Share Permissions
```

---

With IRSA:

```text
Pod
 |
IAM Role
 |
AWS Service
```

---

# Architecture

```text
Pod
 |
IAM Role
 |
S3
```

---

# Benefits

```text
Least Privilege

Improved Security
```

---

# EKS with ECR

Most common deployment pattern.

---

# Architecture

```text
Docker Build
      |
ECR
      |
EKS
      |
Pods
```

---

# Example

```yaml
containers:
- image: 123456789.dkr.ecr.ap-south-1.amazonaws.com/app:v1
```

---

# EKS with Load Balancer

---

# Architecture

```text
Users
  |
ALB
  |
Service
  |
Pods
```

---

# Workflow

```text
User Request
      |
Load Balancer
      |
Service
      |
Pod
```

---

# EKS Auto Scaling

Supports:

```text
Cluster Autoscaler

Horizontal Pod Autoscaler (HPA)
```

---

# Horizontal Pod Autoscaler

Scales Pods.

---

# Example

```text
CPU > 80%
```

Action:

```text
Add Pods
```

---

# Architecture

```text
High CPU
     |
HPA
     |
More Pods
```

---

# Cluster Autoscaler

Scales Nodes.

---

# Example

```text
Pods Need Resources
       |
Add Worker Nodes
```

---

# Architecture

```text
Pending Pods
      |
Cluster Autoscaler
      |
New Node
```

---

# Monitoring

Common tools:

```text
CloudWatch

Prometheus

Grafana
```

---

# Architecture

```text
EKS
 |
Metrics
 |
Monitoring Tools
```

---

# High Availability Architecture

```text
EKS Control Plane
        |
+-------+-------+
|               |
AZ1           AZ2
 |               |
Nodes         Nodes
```

---

# Multi-AZ Deployment

Recommended production setup.

---

Benefits:

```text
High Availability

Fault Tolerance
```

---

# CI/CD Architecture

Most common DevOps workflow.

---

# Architecture

```text
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

---

# Complete Production Architecture

```text
Users
  |
Route 53
  |
ALB
  |
EKS Service
  |
Pods
  |
ECR
```

---

# EKS vs ECS

One of the most common interview questions.

| Feature | ECS | EKS |
|----------|------|------|
| Platform | AWS Native | Kubernetes |
| Complexity | Lower | Higher |
| Flexibility | Lower | Higher |
| Learning Curve | Easier | Harder |
| Portability | Lower | Higher |

---

# ECS vs EKS Architecture

```text
ECS
 |
AWS Orchestrator
```

---

```text
EKS
 |
Kubernetes
```

---

# Real World Example

E-Commerce Application

Workflow:

```text
Developer Push
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
Scalability

High Availability

Automation
```

---

# Advantages

## Managed Kubernetes

AWS manages control plane.

---

## Highly Available

Multi-AZ control plane.

---

## Secure

IAM and VPC integration.

---

## Scalable

Pod and node autoscaling.

---

# Interview Question

## What is Amazon EKS?

### Answer

Amazon EKS (Elastic Kubernetes Service) is a managed Kubernetes service that allows users to run Kubernetes clusters on AWS without managing the Kubernetes control plane.

---

# Interview Question

## What Components Does EKS Manage?

### Answer

AWS manages the Kubernetes control plane, including the API Server, Scheduler, Controller Manager, and etcd.

---

# Interview Question

## What are Worker Nodes?

### Answer

Worker Nodes are machines that run Kubernetes Pods and containers. They provide the compute resources required by applications.

---

# Interview Question

## What is a Node Group?

### Answer

A Node Group is a collection of worker nodes managed together. AWS provides Managed Node Groups that simplify provisioning, updates, and scaling.

---

# Interview Question

## What is IRSA?

### Answer

IAM Roles for Service Accounts (IRSA) allows Kubernetes Pods to assume specific IAM Roles and securely access AWS services without sharing node-level permissions.

---

# Interview Question

## Difference Between ECS and EKS

### Answer

ECS is AWS's native container orchestration service and is simpler to manage, while EKS is a managed Kubernetes service that provides greater flexibility, portability, and Kubernetes ecosystem support.

---

# Interview Question

## How Does EKS Pull Images?

### Answer

EKS worker nodes or Pods authenticate with Amazon ECR using IAM permissions and pull container images required for deployment.

---

# Best Practices

- Use Managed Node Groups.
- Deploy across multiple Availability Zones.
- Use IRSA instead of node-wide permissions.
- Store images in ECR.
- Enable Cluster Autoscaler.
- Monitor using CloudWatch and Prometheus.
- Keep Kubernetes versions updated.

---

# Memory Trick

```text
Control Plane
      |
Manages Cluster
```

```text
Worker Nodes
      |
Run Pods
```

```text
IRSA
     |
Pod -> IAM Role -> AWS Service
```

---

# Summary

Amazon EKS provides:

```text
Managed Kubernetes

High Availability

Auto Scaling

AWS Integration
```

Core Components:

```text
Control Plane

Worker Nodes

Node Groups

Pods

Services
```

Most Common Architecture:

```text
GitHub
   |
Jenkins
   |
Docker
   |
ECR
   |
EKS
```

Amazon EKS is one of the most important modern DevOps and Cloud topics because it combines Kubernetes orchestration with AWS-managed infrastructure, making it a very frequent interview subject for DevOps, SRE, Platform Engineer, and Cloud Engineer roles.
