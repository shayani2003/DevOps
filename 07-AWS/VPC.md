# Amazon VPC (Virtual Private Cloud)

## Introduction

Amazon VPC (Virtual Private Cloud) is one of the most important AWS networking services.

It allows you to create your own isolated network inside AWS where you can launch AWS resources securely.

Think of a VPC as:

```text
Your Private Data Center
         |
Inside AWS Cloud
```

---

# What is a VPC?

A VPC is a logically isolated virtual network in AWS.

Inside a VPC, you can create:

```text
EC2 Instances

RDS Databases

Load Balancers

EKS Clusters
```

while controlling:

```text
IP Addresses

Routing

Security

Internet Access
```

---

# Why Do We Need a VPC?

Without VPC:

```text
All AWS Resources
       |
Same Network
```

Security issues.

---

With VPC:

```text
Company A VPC

Company B VPC

Company C VPC
```

Each isolated from others.

---

# VPC Architecture

```text
AWS Region
     |
     V
+-------------------+
|       VPC         |
|                   |
| Public Subnet     |
| Private Subnet    |
|                   |
+-------------------+
```

---

# CIDR Block

Every VPC gets an IP range.

Example:

```text
10.0.0.0/16
```

Meaning:

```text
65,536 IP Addresses
```

available.

---

# Common CIDR Examples

```text
10.0.0.0/16

172.16.0.0/16

192.168.0.0/16
```

---

# Components of a VPC

Main Components:

```text
Subnets

Route Tables

Internet Gateway

NAT Gateway

Security Groups

Network ACLs
```

---

# What is a Subnet?

A subnet is a smaller network inside a VPC.

---

# Example

VPC:

```text
10.0.0.0/16
```

Subnets:

```text
10.0.1.0/24

10.0.2.0/24

10.0.3.0/24
```

---

# Subnet Architecture

```text
VPC
 |
+-------+-------+
|               |
Public      Private
Subnet      Subnet
```

---

# Types of Subnets

1. Public Subnet
2. Private Subnet

---

# Public Subnet

A subnet with internet access.

---

# Architecture

```text
Internet
    |
Internet Gateway
    |
Public Subnet
    |
EC2
```

---

# Use Cases

```text
Web Servers

Load Balancers

Bastion Hosts
```

---

# Private Subnet

No direct internet access.

---

# Architecture

```text
Internet
    X
    |
Private Subnet
    |
Database
```

---

# Use Cases

```text
RDS

MongoDB

Application Servers

Internal Services
```

---

# Public vs Private Subnet

| Feature | Public | Private |
|----------|---------|---------|
| Internet Access | Yes | No |
| Internet Gateway Route | Yes | No |
| Common Resources | Web Server | Database |
| Security | Lower | Higher |

---

# Internet Gateway (IGW)

## What is Internet Gateway?

Provides internet access to a VPC.

---

# Architecture

```text
Internet
     |
Internet Gateway
     |
VPC
```

---

# Example Route

```text
0.0.0.0/0
      |
Internet Gateway
```

---

# NAT Gateway

## What is NAT Gateway?

Allows private subnet resources to access the internet without exposing them publicly.

---

# Architecture

```text
Private EC2
      |
NAT Gateway
      |
Internet
```

---

# Example

Private Server needs:

```text
Software Updates
```

Use:

```text
NAT Gateway
```

---

# Route Tables

## What is a Route Table?

Controls traffic routing.

---

# Example

```text
Destination     Target

10.0.0.0/16     Local

0.0.0.0/0       IGW
```

---

# Architecture

```text
Subnet
   |
Route Table
   |
Destination
```

---

# Security Group

## What is a Security Group?

Instance-level firewall.

Controls:

```text
Inbound Traffic

Outbound Traffic
```

---

# Example

Allow:

```text
SSH 22

HTTP 80

HTTPS 443
```

---

# Architecture

```text
Internet
     |
Security Group
     |
EC2
```

---

# Network ACL (NACL)

## What is NACL?

Subnet-level firewall.

---

# Architecture

```text
Internet
    |
NACL
    |
Subnet
    |
EC2
```

---

# Security Group vs NACL

| Feature | Security Group | NACL |
|-----------|---------------|------|
| Level | Instance | Subnet |
| Stateful | Yes | No |
| Allow Rules | Yes | Yes |
| Deny Rules | No | Yes |

---

# Stateful vs Stateless

## Security Group (Stateful)

Request:

```text
Allow HTTP
```

Response automatically allowed.

---

# NACL (Stateless)

Request allowed.

Response rule must also exist.

---

# Availability Zones

A VPC can span multiple Availability Zones.

---

# Architecture

```text
VPC
 |
+-----+-----+
|           |
AZ1        AZ2
```

---

# High Availability Design

```text
Internet
    |
Load Balancer
    |
+-----+-----+
|           |
EC2       EC2
AZ1       AZ2
```

---

# Real Production Architecture

```text
Internet
    |
Load Balancer
    |
Public Subnet
    |
Application Servers
    |
Private Subnet
    |
Database
```

---

# Complete VPC Architecture

```text
                    Internet
                        |
                Internet Gateway
                        |
              +----------------+
              |      VPC       |
              |                |
              | Public Subnet  |
              |      |         |
              |     EC2        |
              |      |         |
              | NAT Gateway    |
              |      |         |
              | Private Subnet |
              |      |         |
              |     RDS        |
              +----------------+
```

---

# VPC Peering

Connect two VPCs.

---

# Architecture

```text
VPC A
   |
Peering
   |
VPC B
```

---

# Benefits

- Private Communication
- No Internet Required

---

# Interview Question

## What is a VPC?

### Answer

A VPC (Virtual Private Cloud) is a logically isolated virtual network in AWS where users can launch and manage AWS resources with complete control over networking, security, and routing.

---

# Interview Question

## Difference Between Public and Private Subnet

### Answer

A Public Subnet has a route to an Internet Gateway and can access the internet directly. A Private Subnet does not have direct internet access and is typically used for databases and internal services.

---

# Interview Question

## What is an Internet Gateway?

### Answer

An Internet Gateway is a VPC component that enables communication between resources in a VPC and the public internet.

---

# Interview Question

## What is a NAT Gateway?

### Answer

A NAT Gateway allows resources in private subnets to access the internet while preventing inbound internet connections.

---

# Interview Question

## Difference Between Security Group and NACL

### Answer

Security Groups operate at the instance level and are stateful, while NACLs operate at the subnet level and are stateless. NACLs support both allow and deny rules, whereas Security Groups support only allow rules.

---

# Best Practices

- Place databases in private subnets.
- Use Security Groups for instance security.
- Use NACLs for subnet security.
- Deploy resources across multiple Availability Zones.
- Use NAT Gateway for private subnet internet access.
- Follow least privilege access.

---

# Memory Trick

```text
VPC
 |
Your Private AWS Network
```

Components:

```text
Subnet

Route Table

Internet Gateway

NAT Gateway

Security Group

NACL
```

---

# Summary

VPC Architecture:

```text
Internet
     |
Internet Gateway
     |
Public Subnet
     |
NAT Gateway
     |
Private Subnet
```

Key Concepts:

```text
Public Subnet = Internet Access

Private Subnet = No Direct Internet

Security Group = Stateful

NACL = Stateless
```

Amazon VPC is one of the most important AWS networking services and is almost guaranteed to be discussed in AWS and DevOps interviews.
