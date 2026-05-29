# Kubernetes Service Types

## Introduction

Pods in Kubernetes are temporary.

A Pod can:

```text
Start
Stop
Restart
Move To Another Node
```

Because Pod IP addresses can change, applications cannot directly communicate with Pods reliably.

Kubernetes solves this problem using:

```text
Services
```

---

# What is a Kubernetes Service?

A Service is an abstraction that provides a stable network endpoint for accessing Pods.

Benefits:

- Stable IP Address
- Load Balancing
- Service Discovery
- Decouples Clients from Pods

---

# Why Services Are Needed

Without Service:

```text
User
 |
Pod-A (10.1.1.5)
```

If Pod-A dies:

```text
Pod-B (10.1.2.8)
```

IP changes.

Application breaks.

---

With Service:

```text
User
 |
Service
 |
+----+----+----+
|    |    |    |
P1   P2   P3
```

Pods can change.

Service IP remains stable.

---

# Service Architecture

```text
User
 |
Service
 |
+----+----+----+
|    |    |    |
P1   P2   P3
```

Service acts as a load balancer.

---

# Types of Kubernetes Services

Kubernetes provides:

1. ClusterIP
2. NodePort
3. LoadBalancer
4. ExternalName

---

# Service Overview

```text
Service
   |
+--+--+--+--+
|     |     |
v     v     v
ClusterIP
NodePort
LoadBalancer
ExternalName
```

---

# 1. ClusterIP

## What is ClusterIP?

ClusterIP is the default Service type.

Provides access:

```text
Inside Cluster Only
```

---

# Architecture

```text
Pod A
   |
ClusterIP Service
   |
Pod B
```

Internal communication only.

---

# Example

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:
  type: ClusterIP

  selector:
    app: nginx

  ports:
  - port: 80
```

---

# Traffic Flow

```text
Application
     |
ClusterIP
     |
Pods
```

---

# Use Cases

Examples:

```text
Backend APIs

Databases

Internal Microservices
```

---

# Advantages

- Secure
- Internal Access
- Default Service Type

---

# Limitation

Cannot access from internet.

---

# Diagram

```text
Cluster
 |
ClusterIP
 |
Pods

Outside Access
     X
```

---

# 2. NodePort

## What is NodePort?

NodePort exposes the application on a port of every worker node.

Access:

```text
<Node-IP>:<NodePort>
```

---

# Architecture

```text
Internet
    |
Node IP
    |
NodePort
    |
Pods
```

---

# Example

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx

spec:
  type: NodePort

  ports:
  - port: 80
    nodePort: 30080
```

---

# Traffic Flow

```text
User
 |
192.168.1.10:30080
 |
NodePort
 |
Pods
```

---

# Example URL

```text
http://192.168.1.10:30080
```

---

# Advantages

- Easy to configure
- External access possible

---

# Disadvantages

- Limited scalability
- Manual port management

---

# NodePort Range

Default:

```text
30000 - 32767
```

---

# Diagram

```text
Internet
     |
NodePort
     |
+----+----+
|         |
Pod1     Pod2
```

---

# 3. LoadBalancer

## What is LoadBalancer?

LoadBalancer exposes services externally using a cloud provider load balancer.

Most common production approach.

---

# Architecture

```text
Internet
    |
Load Balancer
    |
Service
    |
Pods
```

---

# Example

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx

spec:
  type: LoadBalancer
```

---

# AWS Example

Kubernetes creates:

```text
AWS ELB

AWS ALB
```

automatically.

---

# Traffic Flow

```text
Users
   |
AWS Load Balancer
   |
Kubernetes Service
   |
Pods
```

---

# Advantages

- External Access
- Cloud Managed
- Production Ready
- Automatic Load Balancing

---

# Disadvantages

- Additional Cloud Cost

---

# Real World Example

```text
E-Commerce Website

Banking Application

Customer Portal
```

Usually uses:

```text
LoadBalancer
```

---

# Diagram

```text
Internet
    |
Load Balancer
    |
+----+----+----+
|    |    |    |
P1   P2   P3
```

---

# 4. ExternalName

## What is ExternalName?

Maps a Kubernetes Service to an external DNS name.

No Pods involved.

---

# Architecture

```text
Application
      |
ExternalName Service
      |
google.com
```

---

# Example

```yaml
apiVersion: v1
kind: Service

metadata:
  name: external-db

spec:
  type: ExternalName

  externalName: database.company.com
```

---

# Traffic Flow

```text
Application
      |
ExternalName
      |
External DNS
```

---

# Use Cases

Examples:

```text
External Database

Third-Party API

Legacy Services
```

---

# Service Comparison

| Feature | ClusterIP | NodePort | LoadBalancer | ExternalName |
|----------|------------|------------|--------------|--------------|
| Default | Yes | No | No | No |
| Internal Access | Yes | Yes | Yes | Yes |
| External Access | No | Yes | Yes | External DNS |
| Cloud Integration | No | No | Yes | No |
| Production Usage | Internal Apps | Testing | Common | Special Cases |

---

# Service Selection Guide

Internal Communication:

```text
ClusterIP
```

---

Simple External Access:

```text
NodePort
```

---

Production External Access:

```text
LoadBalancer
```

---

External Service Mapping:

```text
ExternalName
```

---

# Service Discovery

Kubernetes automatically creates DNS.

Example:

```text
nginx-service.default.svc.cluster.local
```

Applications can communicate using:

```text
Service Name
```

instead of IP addresses.

---

# Real Production Example

Microservices:

```text
Frontend
Backend
Database
```

Architecture:

```text
Frontend
    |
ClusterIP
    |
Backend
    |
ClusterIP
    |
Database
```

External users:

```text
Internet
     |
LoadBalancer
     |
Frontend
```

---

# Interview Question

## What is a Kubernetes Service?

### Answer

A Kubernetes Service provides a stable network endpoint for accessing Pods. It enables service discovery, load balancing, and reliable communication between applications.

---

# Interview Question

## What is ClusterIP?

### Answer

ClusterIP is the default Kubernetes Service type. It exposes applications only within the cluster and is commonly used for internal communication between services.

---

# Interview Question

## What is NodePort?

### Answer

NodePort exposes an application on a specific port of every worker node, allowing external access using the node's IP address and port number.

---

# Interview Question

## What is LoadBalancer?

### Answer

LoadBalancer exposes applications externally through a cloud provider's load balancer such as AWS ELB or Azure Load Balancer. It is the most common production Service type.

---

# Interview Question

## Difference Between NodePort and LoadBalancer

### Answer

NodePort exposes an application through a port on worker nodes, while LoadBalancer creates a cloud-managed load balancer that distributes traffic to Pods automatically.

---

# Interview Question

## What is ExternalName?

### Answer

ExternalName maps a Kubernetes Service to an external DNS name, allowing applications inside the cluster to access external services using Kubernetes DNS.

---

# Best Practices

- Use ClusterIP for internal communication.
- Use LoadBalancer for production internet-facing applications.
- Avoid NodePort in large production environments.
- Use DNS-based service discovery.
- Implement health checks.
- Secure externally exposed services.

---

# Memory Trick

```text
ClusterIP
     |
Inside Cluster

NodePort
     |
Node IP + Port

LoadBalancer
     |
Internet Access

ExternalName
     |
External DNS
```

---

# Summary

Kubernetes Service Types:

```text
ClusterIP
NodePort
LoadBalancer
ExternalName
```

Quick Revision:

```text
ClusterIP
   |
Internal Only

NodePort
   |
Node IP Access

LoadBalancer
   |
Production Internet Access

ExternalName
   |
External DNS Mapping
```

Services are one of the most important Kubernetes networking concepts and are frequently discussed in DevOps, SRE, and Platform Engineering interviews.
