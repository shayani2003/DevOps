# Amazon Route 53

## Introduction

Amazon Route 53 is AWS's highly available and scalable Domain Name System (DNS) service.

It is used to:

- Register Domain Names
- Route Internet Traffic
- Perform Health Checks
- Improve Application Availability

---

# What is DNS?

DNS stands for:

```text
Domain Name System
```

DNS translates:

```text
www.google.com
```

into:

```text
142.250.183.206
```

(IP Address)

---

# Why DNS is Needed?

Humans remember:

```text
amazon.com
```

better than:

```text
54.239.28.85
```

DNS performs the translation.

---

# DNS Architecture

```text
User
 |
www.example.com
 |
DNS
 |
IP Address
 |
Web Server
```

---

# What is Route 53?

Route 53 is AWS's managed DNS service.

The name:

```text
Route
```

means:

```text
Traffic Routing
```

and

```text
53
```

refers to:

```text
DNS Port 53
```

---

# Route 53 Services

Route 53 provides:

```text
Domain Registration

DNS Management

Traffic Routing

Health Checks
```

---

# Route 53 Architecture

```text
User
 |
Route 53
 |
AWS Resource
```

---

# Domain Registration

Route 53 can register domains.

Example:

```text
mycompany.com

example.org

myapp.in
```

---

# Workflow

```text
Buy Domain
     |
Route 53
     |
DNS Management
```

---

# Hosted Zone

## What is a Hosted Zone?

A Hosted Zone stores DNS records for a domain.

---

# Example

Domain:

```text
example.com
```

Hosted Zone contains:

```text
A Record

MX Record

CNAME Record

TXT Record
```

---

# Architecture

```text
Hosted Zone
      |
DNS Records
```

---

# Types of Hosted Zones

## Public Hosted Zone

Accessible from the internet.

---

Example:

```text
google.com
```

---

## Private Hosted Zone

Accessible only within a VPC.

---

Example:

```text
internal.company.local
```

---

# DNS Records

DNS records define how traffic is routed.

---

# Record Types

Most Important:

```text
A

AAAA

CNAME

MX

TXT

NS
```

---

# A Record

Maps:

```text
Domain Name
      |
IPv4 Address
```

---

Example:

```text
example.com
      |
192.168.1.10
```

---

# Architecture

```text
example.com
      |
A Record
      |
IPv4
```

---

# AAAA Record

Maps:

```text
Domain Name
      |
IPv6 Address
```

---

Example:

```text
example.com
      |
2001:db8::1
```

---

# CNAME Record

Maps one domain to another.

---

Example:

```text
www.example.com
        |
example.com
```

---

# Architecture

```text
Alias Name
     |
CNAME
     |
Original Domain
```

---

# MX Record

Mail Exchange Record.

Used for:

```text
Email Routing
```

---

Example:

```text
example.com
      |
mail.example.com
```

---

# TXT Record

Stores text information.

---

Common Uses:

```text
Domain Verification

SPF

DKIM

Security Validation
```

---

# NS Record

Name Server Record.

Identifies DNS servers for a domain.

---

Example:

```text
ns-123.awsdns.com
```

---

# Route 53 Routing Policies

One of the most important interview topics.

---

# Routing Policy Overview

```text
Simple

Weighted

Latency

Failover

Geolocation

Multivalue
```

---

# Simple Routing Policy

Default routing.

---

Architecture:

```text
User
 |
Single Resource
```

---

Example:

```text
example.com
      |
EC2
```

---

# Weighted Routing Policy

Distributes traffic based on percentages.

---

Example:

```text
80% -> Server A

20% -> Server B
```

---

# Architecture

```text
Users
  |
+----+----+
|         |
80%      20%
A         B
```

---

# Use Case

Blue-Green Deployment.

---

# Latency Routing Policy

Routes users to the lowest latency region.

---

Example:

```text
India User
     |
Mumbai Region

US User
     |
Virginia Region
```

---

# Architecture

```text
User
 |
Lowest Latency Region
```

---

# Benefits

Improved performance.

---

# Failover Routing Policy

Supports Disaster Recovery.

---

Architecture:

```text
Primary Server
      |
Health Check
      |
Secondary Server
```

---

# Workflow

```text
Primary Healthy
      |
Serve Traffic
```

---

If failure:

```text
Primary Down
      |
Secondary Active
```

---

# Geolocation Routing

Routes traffic based on user location.

---

Example:

```text
India Users
     |
India Website

US Users
     |
US Website
```

---

# Architecture

```text
Country
   |
Specific Endpoint
```

---

# Multivalue Routing

Returns multiple healthy endpoints.

---

Example:

```text
Server1

Server2

Server3
```

---

# Benefits

- Availability
- Load Distribution

---

# Route 53 Health Checks

## What are Health Checks?

Monitor application health.

---

# Example

Check:

```text
HTTP

HTTPS

TCP
```

---

# Architecture

```text
Route 53
      |
Health Check
      |
Application
```

---

# Health Check Workflow

```text
Healthy
   |
Serve Traffic
```

---

```text
Unhealthy
   |
Failover
```

---

# Alias Record

AWS-specific feature.

---

Used for:

```text
Load Balancer

CloudFront

S3 Website
```

---

# Example

```text
example.com
      |
ALB
```

---

# Alias vs CNAME

| Feature | Alias | CNAME |
|----------|--------|---------|
| AWS Resources | Yes | No |
| Root Domain | Yes | No |
| Extra DNS Query | No | Yes |

---

# Real Production Architecture

```text
Users
  |
Route 53
  |
Load Balancer
  |
EC2 Instances
```

---

# Multi-Region Architecture

```text
Users
  |
Route 53
  |
+-------+-------+
|               |
US Region   India Region
```

---

# Disaster Recovery Architecture

```text
Primary Region
      |
Health Check
      |
Secondary Region
```

---

# Complete Route 53 Architecture

```text
User
  |
Route 53
  |
Routing Policy
  |
Load Balancer
  |
EC2
```

---

# Interview Question

## What is Route 53?

### Answer

Amazon Route 53 is a highly available and scalable DNS service that provides domain registration, DNS management, traffic routing, and health checking capabilities.

---

# Interview Question

## Why is it Called Route 53?

### Answer

The number 53 refers to the DNS service port (Port 53), which is used for DNS queries.

---

# Interview Question

## What is a Hosted Zone?

### Answer

A Hosted Zone is a container for DNS records that define how traffic is routed for a domain.

---

# Interview Question

## Difference Between A Record and CNAME

### Answer

An A Record maps a domain name directly to an IPv4 address, while a CNAME maps one domain name to another domain name.

---

# Interview Question

## What is Weighted Routing?

### Answer

Weighted Routing distributes traffic across multiple resources based on configured percentages, making it useful for gradual deployments and traffic splitting.

---

# Interview Question

## What is Failover Routing?

### Answer

Failover Routing directs traffic to a primary resource and automatically switches to a secondary resource if health checks detect a failure.

---

# Interview Question

## What is an Alias Record?

### Answer

An Alias Record is an AWS-specific DNS record that points directly to AWS resources such as Application Load Balancers, CloudFront distributions, and S3 websites.

---

# Best Practices

- Use Health Checks.
- Use Alias Records for AWS resources.
- Enable Failover Routing for critical applications.
- Use Latency Routing for global applications.
- Protect domains using IAM and MFA.
- Monitor DNS configurations regularly.

---

# Memory Trick

```text
A Record
    |
IPv4
```

```text
AAAA Record
     |
IPv6
```

```text
CNAME
     |
Domain -> Domain
```

Routing Policies:

```text
Simple

Weighted

Latency

Failover

Geolocation

Multivalue
```

---

# Summary

Route 53 provides:

```text
DNS

Domain Registration

Traffic Routing

Health Checks
```

Architecture:

```text
User
 |
Route 53
 |
Routing Policy
 |
Application
```

Amazon Route 53 is a core AWS networking service and is frequently discussed in cloud, DevOps, and solution architect interviews because it controls how users reach applications hosted on AWS.
