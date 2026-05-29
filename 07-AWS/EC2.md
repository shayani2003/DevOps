# Amazon EC2 (Elastic Compute Cloud)

## Introduction

Amazon EC2 (Elastic Compute Cloud) is a web service that provides resizable virtual servers in the cloud.

It allows users to run applications without purchasing and maintaining physical hardware.

---

# What is EC2?

EC2 is a virtual machine (VM) service provided by AWS.

Think of EC2 as:

```text
Your Computer
      |
CPU
RAM
Storage
Operating System
```

running inside AWS.

---

# Why EC2?

Without EC2:

```text
Buy Physical Server
      |
Install OS
      |
Configure Network
      |
Maintain Hardware
```

Expensive and time-consuming.

---

With EC2:

```text
Login AWS
      |
Launch Instance
      |
Ready In Minutes
```

---

# EC2 Architecture

```text
User
  |
AWS Console
  |
EC2 Instance
  |
Application
```

---

# Real World Example

Suppose you want to host:

```text
Portfolio Website

E-Commerce Application

REST API

Database Server
```

You can run them on EC2.

---

# EC2 Features

## Elastic

Scale resources up or down.

---

## On-Demand

Pay only for usage.

---

## Secure

Integrated with IAM and Security Groups.

---

## Highly Available

Can be deployed across multiple Availability Zones.

---

# EC2 Components

An EC2 instance consists of:

```text
Operating System

CPU

Memory

Storage

Network
```

---

# EC2 Instance Lifecycle

```text
Launch
   |
Running
   |
Stopped
   |
Terminated
```

---

# Instance States

## Pending

Instance is launching.

---

## Running

Instance is active.

---

## Stopping

Instance is shutting down.

---

## Stopped

Instance is powered off.

---

## Terminated

Instance permanently deleted.

---

# Lifecycle Diagram

```text
Pending
   |
Running
   |
Stopped
   |
Running
   |
Terminated
```

---

# Amazon Machine Image (AMI)

## What is AMI?

AMI stands for:

```text
Amazon Machine Image
```

It is a template used to create EC2 instances.

Contains:

```text
Operating System

Software

Configuration
```

---

# Example

AMI:

```text
Ubuntu 22.04
```

Launch:

```text
EC2 Instance
```

from that image.

---

# AMI Architecture

```text
AMI
 |
EC2 Instance
```

---

# Instance Types

AWS provides different instance types.

Examples:

```text
t2.micro

t3.micro

m5.large

c5.large

r5.large
```

---

# Instance Families

## General Purpose

```text
t2
t3
m5
```

Balanced CPU and RAM.

---

## Compute Optimized

```text
c5
c6
```

More CPU.

---

## Memory Optimized

```text
r5
r6
```

More RAM.

---

## Storage Optimized

```text
i3
i4
```

High disk performance.

---

# Example Selection

Web Server:

```text
t3.micro
```

---

Machine Learning:

```text
c5.large
```

---

Database:

```text
r5.large
```

---

# Security Groups

## What is a Security Group?

Acts as a virtual firewall.

Controls:

```text
Inbound Traffic

Outbound Traffic
```

---

# Security Group Diagram

```text
Internet
    |
Security Group
    |
EC2
```

---

# Example Rule

Allow SSH:

```text
Port 22
```

---

Allow HTTP:

```text
Port 80
```

---

Allow HTTPS:

```text
Port 443
```

---

# Key Pair

## What is Key Pair?

Used for secure login to EC2.

Components:

```text
Public Key

Private Key
```

---

# Architecture

```text
Private Key
      |
SSH
      |
EC2
```

---

# Connect to EC2

Linux:

```bash
ssh -i key.pem ubuntu@public-ip
```

---

# EBS (Elastic Block Store)

## What is EBS?

Persistent storage attached to EC2.

---

# Architecture

```text
EC2
 |
EBS Volume
```

---

# Features

- Persistent
- Snapshot Support
- High Availability

---

# Example

```text
EC2 Server
      |
100 GB EBS
```

---

# Elastic IP

## What is Elastic IP?

A static public IP address.

Normally:

```text
Stop EC2
      |
IP Changes
```

---

Elastic IP:

```text
Stop EC2
      |
IP Remains Same
```

---

# User Data

Used to execute scripts automatically during instance startup.

---

# Example

```bash
#!/bin/bash

yum update -y

yum install httpd -y

systemctl start httpd
```

---

# Architecture

```text
Launch EC2
      |
User Data Executes
      |
Application Installed
```

---

# EC2 Pricing Models

## On-Demand

Pay per use.

---

## Reserved Instance

Commit for:

```text
1 Year

3 Years
```

Lower cost.

---

## Spot Instance

Uses unused AWS capacity.

Very cheap.

---

## Dedicated Host

Physical server dedicated to one customer.

---

# Pricing Comparison

| Type | Cost | Use Case |
|--------|--------|-----------|
| On-Demand | High | Short-Term |
| Reserved | Medium | Long-Term |
| Spot | Lowest | Flexible Workloads |
| Dedicated | Highest | Compliance |

---

# High Availability

Deploy across multiple AZs.

---

# Architecture

```text
Region
 |
+----+----+
|         |
AZ1      AZ2
 |         |
EC2      EC2
```

---

# Auto Scaling Integration

```text
Load Balancer
      |
+-----+-----+
|           |
EC2       EC2
```

Automatically adds or removes instances.

---

# Monitoring with CloudWatch

Metrics:

```text
CPU

Memory

Network

Disk
```

---

# Complete EC2 Architecture

```text
Internet
    |
Security Group
    |
EC2 Instance
    |
EBS Volume
```

---

# Real Production Example

E-Commerce Website

Architecture:

```text
Users
  |
Load Balancer
  |
+-----+-----+
|           |
EC2       EC2
```

Benefits:

- High Availability
- Scalability
- Fault Tolerance

---

# Interview Question

## What is EC2?

### Answer

Amazon EC2 (Elastic Compute Cloud) is a web service that provides resizable virtual servers in the AWS cloud. It allows users to deploy and run applications without managing physical hardware.

---

# Interview Question

## What is an AMI?

### Answer

An AMI (Amazon Machine Image) is a template used to launch EC2 instances. It contains the operating system, software packages, and configuration settings required for the instance.

---

# Interview Question

## What is the Difference Between Security Group and NACL?

### Answer

A Security Group acts as a stateful firewall at the instance level, while a Network ACL acts as a stateless firewall at the subnet level.

---

# Interview Question

## What is EBS?

### Answer

Elastic Block Store (EBS) is persistent block-level storage that can be attached to EC2 instances for storing operating systems, applications, and data.

---

# Interview Question

## What is an Elastic IP?

### Answer

An Elastic IP is a static public IPv4 address that remains associated with your AWS account until released.

---

# Interview Question

## What are Spot Instances?

### Answer

Spot Instances use unused AWS capacity and offer significant cost savings, but AWS can terminate them when capacity is required elsewhere.

---

# Best Practices

- Use Security Groups.
- Enable Monitoring.
- Use IAM Roles instead of access keys.
- Take EBS Snapshots regularly.
- Use Auto Scaling.
- Distribute instances across multiple Availability Zones.
- Use Elastic Load Balancers.

---

# Memory Trick

```text
EC2
 |
Virtual Machine
```

Key Components:

```text
AMI

Instance Type

Security Group

Key Pair

EBS

Elastic IP
```

---

# Summary

Amazon EC2 provides:

```text
Virtual Servers

Scalable Compute

Cloud Hosting
```

Architecture:

```text
Internet
    |
Security Group
    |
EC2
    |
EBS
```

EC2 is the foundation of many AWS architectures and is one of the most frequently asked AWS interview topics because it is the core compute service of AWS.
