# Deployment vs StatefulSet vs DaemonSet

## Introduction

In Kubernetes, workloads can be managed using different controllers.

The most commonly used controllers are:

1. Deployment
2. StatefulSet
3. DaemonSet

One of the most frequently asked Kubernetes interview questions is:

> What is the difference between Deployment, StatefulSet, and DaemonSet?

To answer this, first understand:

```text
Deployment  -> Stateless Applications

StatefulSet -> Stateful Applications

DaemonSet   -> One Pod Per Node
```

---

# Kubernetes Controllers Overview

```text
                Kubernetes
                      |
      +---------------+---------------+
      |               |               |
      v               v               v
 Deployment     StatefulSet      DaemonSet
```

Each controller manages Pods differently.

---

# What is a Deployment?

A Deployment manages stateless applications.

Examples:

```text
Frontend
Backend APIs
Web Servers
Microservices
```

---

## Deployment Architecture

```text
Deployment
      |
      v
ReplicaSet
      |
+-----+-----+-----+
|     |     |     |
P1    P2    P3
```

---

## Characteristics

### Stateless

Pods do not store permanent data.

---

### Identical Pods

All Pods are identical.

---

### Auto Healing

Failed Pods are recreated automatically.

---

### Auto Scaling

Can increase or decrease replicas.

---

## Example

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx

spec:
  replicas: 3
```

---

## Real World Example

```text
React Application

NodeJS API

Nginx Server
```

Usually managed by Deployments.

---

# Deployment Workflow

```text
Deployment
      |
ReplicaSet
      |
Pods
```

---

# What is a StatefulSet?

A StatefulSet manages stateful applications.

Applications that require:

- Stable Identity
- Stable Storage
- Ordered Deployment

---

## Examples

```text
MySQL
PostgreSQL
MongoDB
Redis Cluster
Kafka
```

---

# StatefulSet Architecture

```text
StatefulSet
      |
+-----+-----+-----+
|     |     |     |
DB-0 DB-1 DB-2
```

Notice:

```text
Stable Names
```

Each Pod has a unique identity.

---

# StatefulSet Pod Naming

Example:

```text
mysql-0

mysql-1

mysql-2
```

Unlike Deployments:

```text
nginx-abcd

nginx-xyz
```

which change frequently.

---

# Persistent Storage

Each Pod gets its own volume.

Example:

```text
mysql-0 -> Volume-0

mysql-1 -> Volume-1

mysql-2 -> Volume-2
```

Storage remains even if Pod restarts.

---

# StatefulSet Characteristics

### Stable Network Identity

```text
mysql-0
mysql-1
mysql-2
```

Always remain the same.

---

### Persistent Storage

Each Pod keeps its own data.

---

### Ordered Deployment

Pods start sequentially.

```text
mysql-0

then

mysql-1

then

mysql-2
```

---

### Ordered Termination

Pods stop in reverse order.

---

# StatefulSet Workflow

```text
StatefulSet
      |
Stable Pods
      |
Persistent Volumes
```

---

# What is a DaemonSet?

A DaemonSet ensures one Pod runs on every worker node.

---

# DaemonSet Architecture

```text
Cluster
   |
+--+--+--+
|     |  |
N1   N2 N3
|     |  |
P1   P2 P3
```

One Pod per node.

---

# Purpose of DaemonSet

Used for:

- Monitoring
- Logging
- Security Agents

---

# Examples

```text
Fluentd

Prometheus Node Exporter

Datadog Agent

Security Agents
```

---

# DaemonSet Behavior

Current Cluster:

```text
Node1
Node2
Node3
```

DaemonSet creates:

```text
Pod1
Pod2
Pod3
```

---

Add Node4:

```text
Node4 Added
```

Automatically:

```text
Pod4 Created
```

---

Remove Node4:

```text
Pod4 Removed
```

---

# DaemonSet Workflow

```text
New Node Added
      |
DaemonSet Detects
      |
New Pod Created
```

---

# Comparison Diagram

## Deployment

```text
Deployment
    |
P1 P2 P3
```

Pods distributed where needed.

---

## StatefulSet

```text
StatefulSet
     |
DB-0 DB-1 DB-2
```

Stable identities.

---

## DaemonSet

```text
Node1 -> Pod1

Node2 -> Pod2

Node3 -> Pod3
```

One Pod per node.

---

# Comparison Table

| Feature | Deployment | StatefulSet | DaemonSet |
|-----------|------------|-------------|------------|
| Application Type | Stateless | Stateful | Node-Level |
| Pod Identity | Dynamic | Stable | Stable |
| Persistent Storage | Optional | Required | Usually Not |
| Scaling | Manual/Auto | Manual/Auto | One Per Node |
| Ordered Deployment | No | Yes | No |
| Typical Use Cases | APIs, Web Apps | Databases | Monitoring, Logging |

---

# Real World Example

## Deployment

Application:

```text
React Frontend
```

Need:

```text
5 Replicas
```

Use:

```text
Deployment
```

---

## StatefulSet

Application:

```text
MySQL Cluster
```

Need:

```text
Persistent Data
```

Use:

```text
StatefulSet
```

---

## DaemonSet

Application:

```text
Log Collection Agent
```

Need:

```text
One Pod On Every Node
```

Use:

```text
DaemonSet
```

---

# Interview Question

## What is a Deployment?

### Answer

A Deployment is a Kubernetes controller used for managing stateless applications. It provides scaling, rolling updates, and self-healing capabilities through ReplicaSets.

---

# Interview Question

## What is a StatefulSet?

### Answer

A StatefulSet is a Kubernetes controller used for stateful applications. It provides stable pod identities, persistent storage, and ordered deployment and termination.

---

# Interview Question

## What is a DaemonSet?

### Answer

A DaemonSet ensures that a copy of a Pod runs on every worker node in the cluster. It is commonly used for monitoring, logging, and security agents.

---

# Interview Question

## Difference Between Deployment and StatefulSet

### Answer

Deployments are used for stateless applications where pod identity does not matter. StatefulSets are used for stateful applications that require stable identities and persistent storage.

---

# Interview Question

## When Would You Use a DaemonSet?

### Answer

I use a DaemonSet when I need a Pod to run on every node, such as log collectors, monitoring agents, or security tools.

---

# Best Practices

- Use Deployments for web applications.
- Use StatefulSets for databases.
- Use DaemonSets for node-level services.
- Configure resource limits.
- Use Persistent Volumes with StatefulSets.
- Monitor controller health.

---

# Summary

```text
Deployment
   |
Stateless Applications
```

Examples:

```text
Frontend
Backend APIs
Microservices
```

---

```text
StatefulSet
   |
Stateful Applications
```

Examples:

```text
MySQL
PostgreSQL
MongoDB
Kafka
```

---

```text
DaemonSet
   |
One Pod Per Node
```

Examples:

```text
Fluentd
Datadog
Node Exporter
```

Quick Memory Trick:

```text
Deployment  = Stateless

StatefulSet = Database

DaemonSet   = One Pod Per Node
```

This is one of the highest-frequency Kubernetes interview questions and is often asked in both beginner and experienced DevOps interviews.
