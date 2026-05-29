# Kubernetes Architecture

## What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform used to deploy, manage, scale, and monitor containerized applications.

Originally developed by Google and now maintained by the Cloud Native Computing Foundation (CNCF).

---

# Why Kubernetes?

Managing a few Docker containers manually is easy.

Example:

```text
Container 1
Container 2
Container 3
```

But in production:

```text
100+
500+
1000+
Containers
```

Managing them manually becomes difficult.

Kubernetes provides:

- Automatic Scaling
- Self-Healing
- Load Balancing
- Service Discovery
- Rolling Updates
- High Availability

---

# Kubernetes Architecture Overview

```text
                    Kubernetes Cluster
                           |
         +-----------------+-----------------+
         |                                   |
         v                                   v
   Control Plane                      Worker Nodes
         |                                   |
         |                            +------+------+ 
         |                            |             |
         v                            v             v
   Cluster Management             Pod A         Pod B
```

---

# Main Components

Kubernetes Architecture consists of:

## Control Plane Components

1. API Server
2. ETCD
3. Scheduler
4. Controller Manager
5. Cloud Controller Manager

---

## Worker Node Components

1. Kubelet
2. Kube Proxy
3. Container Runtime
4. Pods

---

# Complete Architecture Diagram

```text
                    Control Plane
                          |
   +----------+-----------+----------+
   |          |           |          |
   v          v           v          v
API Server  ETCD     Scheduler  Controller
                                      |
                                      |
               +----------------------+
               |
               v
        +------+------+
        | Worker Node |
        +------+------+
               |
       +-------+-------+
       |               |
       v               v
     Pod A          Pod B
```

---

# What is a Cluster?

A Kubernetes Cluster consists of:

```text
Control Plane
      +
Worker Nodes
```

Example:

```text
1 Control Plane

3 Worker Nodes
```

---

# Control Plane

The Control Plane is the brain of Kubernetes.

Responsibilities:

- Makes decisions
- Schedules workloads
- Monitors cluster state
- Maintains desired state

---

# 1. API Server

## What is API Server?

The API Server is the entry point of Kubernetes.

Every request goes through it.

---

## Diagram

```text
kubectl
   |
   v
API Server
```

---

## Responsibilities

- Accept requests
- Validate requests
- Update cluster state
- Communicate with ETCD

---

## Example

```bash
kubectl create deployment nginx
```

Request flow:

```text
kubectl
   |
API Server
   |
ETCD
```

---

# 2. ETCD

## What is ETCD?

ETCD is a distributed key-value database.

Stores:

```text
Cluster State
Configuration
Secrets
Deployments
Nodes
```

---

## Diagram

```text
API Server
     |
     v
   ETCD
```

---

## Example

Stores:

```text
Pod Information

Node Information

Service Information
```

---

# 3. Scheduler

## What is Scheduler?

The Scheduler decides where Pods should run.

---

## Example

New Pod Created.

Available Nodes:

```text
Node 1
Node 2
Node 3
```

Scheduler chooses best node.

---

## Diagram

```text
New Pod
   |
Scheduler
   |
Best Node Selected
```

---

# 4. Controller Manager

## What is Controller Manager?

Ensures actual state matches desired state.

---

Example:

Desired:

```text
3 Pods
```

Current:

```text
2 Pods
```

Controller creates:

```text
1 Additional Pod
```

---

## Diagram

```text
Desired State
      |
Controller Manager
      |
Actual State
```

---

# 5. Cloud Controller Manager

Used in cloud providers.

Examples:

```text
AWS
Azure
GCP
```

Handles:

- Load Balancers
- Storage
- Cloud Networking

---

# Worker Node

Worker Nodes run applications.

---

## Worker Node Components

```text
Kubelet
Kube Proxy
Container Runtime
Pods
```

---

# 1. Kubelet

## What is Kubelet?

Agent running on every worker node.

Responsibilities:

- Talks to API Server
- Starts containers
- Monitors Pods

---

## Diagram

```text
API Server
     |
 Kubelet
     |
   Pods
```

---

# 2. Kube Proxy

## What is Kube Proxy?

Handles networking.

Responsibilities:

- Traffic routing
- Load balancing
- Service communication

---

## Diagram

```text
User Request
      |
Kube Proxy
      |
Target Pod
```

---

# 3. Container Runtime

Software responsible for running containers.

Examples:

```text
containerd
CRI-O
Docker (older)
```

---

## Diagram

```text
Pod
  |
Container Runtime
  |
Container
```

---

# 4. Pod

## What is a Pod?

Smallest deployable unit in Kubernetes.

A Pod contains:

```text
One or More Containers
```

---

## Diagram

```text
Pod
 |
+---------+
|Container|
+---------+
```

---

# Request Flow in Kubernetes

User accesses application.

```text
User
 |
Service
 |
Pod
 |
Application
```

---

Detailed Flow:

```text
User
 |
Ingress
 |
Service
 |
Pod
 |
Container
```

---

# Self-Healing

Suppose Pod crashes.

Before:

```text
Pod 1
Pod 2
Pod 3
```

---

Pod 2 crashes:

```text
Pod 1
Pod 3
```

---

Controller detects issue.

Creates:

```text
Pod 4
```

Cluster restored.

---

# Scaling

Current:

```text
2 Pods
```

Scale:

```bash
kubectl scale deployment nginx --replicas=5
```

Result:

```text
5 Pods
```

---

# High Availability

Multiple Pods ensure availability.

```text
User
 |
Service
 |
+---+---+---+
|   |   |   |
P1 P2 P3 P4
```

If one Pod fails:

```text
Traffic continues
```

---

# Kubernetes Architecture Workflow

```text
Developer
      |
kubectl apply
      |
API Server
      |
ETCD
      |
Scheduler
      |
Worker Node
      |
Pod Created
```

---

# Real-World Example

E-Commerce Application

Requirements:

```text
Frontend
Backend
Database
```

Deployment:

```text
Frontend Pods

Backend Pods

Database Pods
```

Managed by Kubernetes.

Benefits:

- Auto Scaling
- Self-Healing
- High Availability

---

# Interview Question

## What is Kubernetes?

### Answer

Kubernetes is an open-source container orchestration platform used to automate the deployment, scaling, management, and monitoring of containerized applications.

---

# Interview Question

## Explain Kubernetes Architecture

### Answer

Kubernetes architecture consists of a Control Plane and Worker Nodes. The Control Plane includes components such as API Server, ETCD, Scheduler, and Controller Manager, which manage the cluster. Worker Nodes run application workloads using Kubelet, Kube Proxy, Container Runtime, and Pods.

---

# Interview Question

## What is ETCD?

### Answer

ETCD is a distributed key-value store used by Kubernetes to store cluster configuration, state information, secrets, and metadata.

---

# Interview Question

## What is Kubelet?

### Answer

Kubelet is an agent running on each worker node. It communicates with the API Server and ensures containers are running as expected.

---

# Interview Question

## What is the Role of Scheduler?

### Answer

The Scheduler assigns Pods to worker nodes based on available resources, constraints, and scheduling policies.

---

# Best Practices

- Use multiple worker nodes.
- Enable monitoring.
- Secure ETCD.
- Use RBAC.
- Configure resource limits.
- Implement high availability.

---

# Summary

Kubernetes Architecture:

```text
Control Plane
      |
Worker Nodes
      |
Pods
```

Control Plane Components:

```text
API Server
ETCD
Scheduler
Controller Manager
Cloud Controller Manager
```

Worker Node Components:

```text
Kubelet
Kube Proxy
Container Runtime
Pods
```

Kubernetes provides:

- Auto Scaling
- Self-Healing
- High Availability
- Service Discovery
- Load Balancing

making it the most widely used container orchestration platform in modern cloud-native environments.
