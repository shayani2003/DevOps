# AWS ELB (Elastic Load Balancing)

## Introduction

Elastic Load Balancing (ELB) is an AWS service that automatically distributes incoming application traffic across multiple targets.

Targets can be:

```text
EC2 Instances

Containers

IP Addresses

Lambda Functions
```

ELB improves:

- Availability
- Scalability
- Fault Tolerance
- Performance

---

# What is a Load Balancer?

A Load Balancer acts as a traffic distributor.

Without Load Balancer:

```text
Users
   |
Single Server
```

Problem:

```text
Server Failure
      |
Application Down
```

---

With Load Balancer:

```text
Users
   |
Load Balancer
   |
+----+----+----+
|    |    |    |
S1   S2   S3
```

Traffic distributed.

---

# Why ELB?

Benefits:

- High Availability
- Automatic Traffic Distribution
- Health Monitoring
- Fault Tolerance
- SSL Termination

---

# ELB Architecture

```text
Users
  |
ELB
  |
+----+----+----+
|    |    |    |
EC2 EC2 EC2
```

---

# Types of Load Balancers

AWS provides:

1. Application Load Balancer (ALB)
2. Network Load Balancer (NLB)
3. Gateway Load Balancer (GWLB)

---

# ELB Overview

```text
Elastic Load Balancer
          |
+---------+---------+
|         |         |
ALB      NLB      GWLB
```

---

# Application Load Balancer (ALB)

## What is ALB?

ALB operates at:

```text
Layer 7
(Application Layer)
```

of the OSI Model.

---

# Supports

```text
HTTP

HTTPS

WebSockets
```

---

# Architecture

```text
Users
   |
ALB
   |
+----+----+
|         |
Web1    Web2
```

---

# Intelligent Routing

ALB can route based on:

```text
URL Path

Hostname

HTTP Headers
```

---

# Example

Request:

```text
example.com/api
```

Route:

```text
Backend API
```

---

Request:

```text
example.com/images
```

Route:

```text
Image Service
```

---

# Path-Based Routing

```text
/api
  |
Backend

/images
  |
Image Service
```

---

# Host-Based Routing

```text
api.company.com
      |
API Service

shop.company.com
      |
E-Commerce Service
```

---

# ALB Architecture

```text
Users
   |
ALB
   |
+-----------+-----------+
|                       |
API Service     Image Service
```

---

# ALB Use Cases

```text
Web Applications

Microservices

REST APIs

Kubernetes Ingress
```

---

# Network Load Balancer (NLB)

## What is NLB?

NLB operates at:

```text
Layer 4
(Transport Layer)
```

---

# Supports

```text
TCP

UDP

TLS
```

---

# Features

- Ultra High Performance
- Low Latency
- Millions of Requests

---

# Architecture

```text
Users
   |
NLB
   |
+----+----+
|         |
Server1 Server2
```

---

# Use Cases

```text
Gaming

Financial Applications

High Performance APIs
```

---

# ALB vs NLB

| Feature | ALB | NLB |
|----------|------|------|
| OSI Layer | Layer 7 | Layer 4 |
| Protocols | HTTP/HTTPS | TCP/UDP |
| Path Routing | Yes | No |
| Host Routing | Yes | No |
| Performance | High | Very High |

---

# Gateway Load Balancer (GWLB)

## What is GWLB?

GWLB is used for:

```text
Security Appliances
```

Examples:

```text
Firewalls

Intrusion Detection Systems

Traffic Inspection
```

---

# Architecture

```text
Traffic
   |
GWLB
   |
Firewall Appliance
```

---

# Use Cases

```text
Network Security

Traffic Inspection

Threat Detection
```

---

# Target Groups

## What is a Target Group?

A Target Group contains resources that receive traffic.

---

# Example

```text
Target Group
     |
+----+----+----+
|    |    |    |
EC2 EC2 EC2
```

---

# Supported Targets

```text
EC2

IP Address

Lambda
```

---

# Traffic Flow

```text
User
 |
Load Balancer
 |
Target Group
 |
EC2
```

---

# Health Checks

## What are Health Checks?

ELB continuously checks target health.

---

# Workflow

```text
Healthy
   |
Receive Traffic
```

---

```text
Unhealthy
   |
No Traffic
```

---

# Architecture

```text
ELB
 |
Health Check
 |
Target
```

---

# Example

Check:

```text
/health
```

Response:

```text
200 OK
```

Healthy.

---

Response:

```text
500 Error
```

Unhealthy.

---

# Cross-Zone Load Balancing

Distributes traffic evenly across Availability Zones.

---

# Architecture

```text
ELB
 |
+------+------+
|             |
AZ1         AZ2
 |             |
EC2          EC2
```

---

# Benefits

- Better Availability
- Better Resource Utilization

---

# SSL/TLS Termination

ELB can handle HTTPS encryption.

---

# Architecture

```text
Users
   |
HTTPS
   |
ELB
   |
HTTP
   |
EC2
```

---

# Benefits

- Reduced EC2 Load
- Simplified Certificate Management

---

# Auto Scaling Integration

Most common architecture.

---

# Architecture

```text
Users
   |
ELB
   |
Auto Scaling Group
   |
+----+----+----+
|    |    |    |
EC2 EC2 EC2
```

---

# Traffic During Scaling

New EC2:

```text
Created
   |
Added To Target Group
   |
Receives Traffic
```

---

# Multi-AZ Architecture

```text
Users
  |
ELB
  |
+------+------+
|             |
AZ1         AZ2
 |             |
EC2          EC2
```

---

# Complete Production Architecture

```text
Internet
    |
Route 53
    |
ALB
    |
Auto Scaling Group
    |
+------+------+
|             |
EC2          EC2
```

---

# Real World Example

E-Commerce Website

Architecture:

```text
Users
  |
Route 53
  |
ALB
  |
+------+------+
|             |
Web1        Web2
```

Benefits:

- High Availability
- Load Distribution
- Fault Tolerance

---

# Interview Question

## What is ELB?

### Answer

Elastic Load Balancing (ELB) is an AWS service that automatically distributes incoming traffic across multiple targets such as EC2 instances, containers, and Lambda functions to improve availability and scalability.

---

# Interview Question

## What are the Types of Load Balancers in AWS?

### Answer

AWS provides:

```text
Application Load Balancer (ALB)

Network Load Balancer (NLB)

Gateway Load Balancer (GWLB)
```

Each serves different use cases.

---

# Interview Question

## Difference Between ALB and NLB

### Answer

ALB operates at Layer 7 and supports HTTP/HTTPS with path-based and host-based routing. NLB operates at Layer 4 and supports TCP/UDP traffic with extremely high performance and low latency.

---

# Interview Question

## What is a Target Group?

### Answer

A Target Group is a collection of resources such as EC2 instances, IP addresses, or Lambda functions that receive traffic from a Load Balancer.

---

# Interview Question

## What Happens if an EC2 Instance Becomes Unhealthy?

### Answer

ELB health checks detect the unhealthy instance and stop sending traffic to it until it becomes healthy again.

---

# Interview Question

## Why Use ELB with Auto Scaling?

### Answer

ELB distributes traffic across all healthy instances while Auto Scaling automatically adjusts the number of instances based on demand, providing scalability and high availability.

---

# Best Practices

- Use ALB for web applications.
- Use NLB for high-performance TCP/UDP workloads.
- Enable Cross-Zone Load Balancing.
- Configure Health Checks properly.
- Integrate with Auto Scaling.
- Use HTTPS with SSL/TLS certificates.

---

# Memory Trick

```text
ALB
 |
HTTP/HTTPS
 |
Layer 7
```

```text
NLB
 |
TCP/UDP
 |
Layer 4
```

```text
GWLB
 |
Security Appliances
```

---

# Summary

AWS Load Balancers:

```text
ALB

NLB

GWLB
```

Architecture:

```text
Users
  |
Load Balancer
  |
Target Group
  |
EC2
```

Benefits:

```text
High Availability

Scalability

Fault Tolerance

Traffic Distribution
```

Elastic Load Balancing is a core AWS service and one of the most frequently discussed topics in AWS, Cloud, and DevOps interviews because it plays a critical role in building highly available and scalable applications.
