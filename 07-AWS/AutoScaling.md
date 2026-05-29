# AWS Auto Scaling

## Introduction

AWS Auto Scaling automatically adjusts resources based on application demand.

It helps ensure:

- High Availability
- Performance
- Cost Optimization

Without Auto Scaling:

```text
Traffic Increase
      |
Server Overload
      |
Application Slow
```

---

With Auto Scaling:

```text
Traffic Increase
      |
Add More Servers
      |
Application Stable
```

---

# What is Auto Scaling?

Auto Scaling automatically:

```text
Adds EC2 Instances

Removes EC2 Instances
```

based on demand.

---

# Why Auto Scaling?

Applications rarely receive constant traffic.

Example:

```text
Morning   -> Low Traffic

Afternoon -> Medium Traffic

Festival  -> Very High Traffic
```

---

Without Auto Scaling:

```text
Need Maximum Servers Always
```

Higher cost.

---

With Auto Scaling:

```text
Scale Up

Scale Down
```

Automatically.

---

# Auto Scaling Architecture

```text
Users
  |
Load Balancer
  |
Auto Scaling Group
  |
+----+----+----+
|    |    |    |
EC2 EC2 EC2
```

---

# Core Components

AWS Auto Scaling consists of:

```text
Launch Template

Auto Scaling Group (ASG)

Scaling Policy

CloudWatch
```

---

# Launch Template

## What is a Launch Template?

A Launch Template defines how EC2 instances should be launched.

---

# Contains

```text
AMI

Instance Type

Security Group

Key Pair

User Data
```

---

# Architecture

```text
Launch Template
       |
Create EC2
```

---

# Example

```text
AMI:
Ubuntu 22.04

Instance Type:
t3.micro

Security Group:
Web-SG
```

---

# Why Launch Templates?

Ensures every new EC2 instance is created consistently.

---

# Auto Scaling Group (ASG)

## What is an ASG?

An Auto Scaling Group manages EC2 instances automatically.

---

# Responsibilities

```text
Maintain Desired Capacity

Replace Failed Instances

Scale Up

Scale Down
```

---

# Architecture

```text
Auto Scaling Group
        |
+-------+-------+
|               |
EC2           EC2
```

---

# Desired Capacity

Desired number of instances.

Example:

```text
Desired = 2
```

ASG ensures:

```text
2 Instances Running
```

always.

---

# Minimum Capacity

Minimum instances allowed.

Example:

```text
Min = 1
```

---

# Maximum Capacity

Maximum instances allowed.

Example:

```text
Max = 10
```

---

# Capacity Example

```text
Minimum = 1

Desired = 2

Maximum = 10
```

---

# ASG Architecture

```text
ASG
 |
Min = 1

Desired = 2

Max = 10
```

---

# Health Checks

ASG continuously monitors instances.

---

# Workflow

```text
Instance Healthy
      |
Keep Running
```

---

```text
Instance Failed
      |
Terminate
      |
Launch New Instance
```

---

# Self-Healing Architecture

```text
EC2 Crash
    |
ASG Detects
    |
New EC2 Created
```

---

# Scaling Policies

Scaling Policies define when scaling occurs.

---

# Types

```text
Target Tracking

Step Scaling

Simple Scaling

Predictive Scaling
```

---

# Target Tracking Scaling

Most commonly used.

---

# Example

Target CPU:

```text
50%
```

---

If CPU:

```text
70%
```

Scale Out.

---

If CPU:

```text
20%
```

Scale In.

---

# Architecture

```text
CPU > 50%
      |
Add Instance

CPU < 50%
      |
Remove Instance
```

---

# Step Scaling

Scales based on thresholds.

---

# Example

```text
CPU > 60%
   |
+1 Instance

CPU > 80%
   |
+3 Instances
```

---

# Architecture

```text
60%
 |
+1

80%
 |
+3
```

---

# Simple Scaling

Basic scaling policy.

---

Example:

```text
CPU > 70%
```

Action:

```text
Add 1 Instance
```

---

Wait:

```text
Cooldown Period
```

before next action.

---

# Predictive Scaling

Uses machine learning.

Predicts future demand.

---

# Example

Every day:

```text
9 AM
```

Traffic increases.

---

Predictive Scaling:

```text
Launch Instances Before 9 AM
```

---

# Architecture

```text
Historical Data
       |
Prediction
       |
Scale Ahead
```

---

# CloudWatch Integration

CloudWatch provides metrics.

---

# Metrics

```text
CPU Utilization

Memory

Network

Disk
```

---

# Architecture

```text
CloudWatch
     |
Metric Alarm
     |
ASG
```

---

# Example

CPU:

```text
80%
```

Alarm Triggered.

---

Action:

```text
Add Instance
```

---

# Scale Out vs Scale In

## Scale Out

Add resources.

---

Example:

```text
2 Instances
      |
4 Instances
```

---

# Scale In

Remove resources.

---

Example:

```text
4 Instances
      |
2 Instances
```

---

# Architecture

```text
High Traffic
      |
Scale Out
```

---

```text
Low Traffic
      |
Scale In
```

---

# Auto Scaling with ELB

Most common production architecture.

---

# Architecture

```text
Users
  |
ALB
  |
ASG
  |
+----+----+----+
|    |    |    |
EC2 EC2 EC2
```

---

# Traffic Increase

```text
Users Increase
      |
CPU Increase
      |
ASG Adds EC2
```

---

# Traffic Decrease

```text
Users Decrease
      |
CPU Decrease
      |
ASG Removes EC2
```

---

# Multi-AZ Architecture

Recommended production setup.

---

# Architecture

```text
ALB
 |
+------+------+
|             |
AZ1         AZ2
 |             |
EC2          EC2
```

---

Benefits:

```text
High Availability
```

---

# Real World Example

E-Commerce Website

Normal Day:

```text
2 Instances
```

---

Festival Sale:

```text
20 Instances
```

---

After Sale:

```text
Back To 2
```

---

Automatic.

---

# Complete Auto Scaling Architecture

```text
Users
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

# Advantages

## High Availability

Failed instances automatically replaced.

---

## Cost Optimization

Pay only for required resources.

---

## Scalability

Handles traffic spikes.

---

## Fault Tolerance

Maintains application availability.

---

# Interview Question

## What is Auto Scaling?

### Answer

AWS Auto Scaling automatically adjusts the number of resources based on demand, helping maintain performance, availability, and cost efficiency.

---

# Interview Question

## What is an Auto Scaling Group?

### Answer

An Auto Scaling Group (ASG) is a collection of EC2 instances managed together. It maintains desired capacity, performs health checks, and automatically scales resources.

---

# Interview Question

## What is a Launch Template?

### Answer

A Launch Template defines how EC2 instances should be launched, including AMI, instance type, security groups, key pairs, and user data.

---

# Interview Question

## Difference Between Scale Out and Scale In

### Answer

Scale Out adds more instances to handle increased demand, while Scale In removes instances when demand decreases.

---

# Interview Question

## How Does Auto Scaling Work with CloudWatch?

### Answer

CloudWatch monitors metrics such as CPU utilization and triggers scaling policies when thresholds are reached, causing the Auto Scaling Group to add or remove instances.

---

# Interview Question

## Why Use Auto Scaling with ELB?

### Answer

ELB distributes traffic across instances, while Auto Scaling adjusts the number of instances based on demand. Together they provide scalability, fault tolerance, and high availability.

---

# Best Practices

- Use Launch Templates.
- Enable Health Checks.
- Use Target Tracking Policies.
- Deploy across multiple AZs.
- Integrate with ALB.
- Monitor CloudWatch Metrics.
- Set appropriate Min, Max, and Desired capacities.

---

# Memory Trick

```text
Launch Template
       |
Auto Scaling Group
       |
Scaling Policy
       |
CloudWatch
```

---

# Summary

Auto Scaling Components:

```text
Launch Template

Auto Scaling Group

CloudWatch

Scaling Policies
```

Workflow:

```text
High Traffic
      |
Scale Out

Low Traffic
      |
Scale In
```

Production Architecture:

```text
Users
  |
ALB
  |
ASG
  |
EC2
```

AWS Auto Scaling is one of the most important cloud architecture services because it automatically balances performance, availability, and cost, making it a frequent topic in AWS and DevOps interviews.
