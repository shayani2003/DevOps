# ECS Auto Scaling

## Introduction

ECS Auto Scaling automatically adjusts the number of running tasks based on application demand.

Instead of manually increasing or decreasing containers, ECS can scale workloads automatically.

This helps:

- Handle traffic spikes
- Reduce infrastructure costs
- Improve application availability
- Maintain performance

---

# Why Auto Scaling?

Without Auto Scaling:

```text
Traffic Increases
       |
       v
Application Becomes Slow
       |
       v
Users Experience Issues
```

---

With Auto Scaling:

```text
Traffic Increases
       |
       v
ECS Adds More Tasks
       |
       v
Load Distributed
       |
       v
Application Remains Healthy
```

---

# ECS Auto Scaling Architecture

```text
Users
   |
   v
Load Balancer
   |
   v
ECS Service
   |
   +------------------+
   |                  |
Task 1            Task 2
   |
CloudWatch Metrics
   |
Auto Scaling Policy
```

---

# How ECS Auto Scaling Works

Step 1:

Users generate traffic.

```text
Users
   |
Application
```

---

Step 2:

CloudWatch monitors metrics.

Examples:

- CPU Utilization
- Memory Utilization
- Request Count

---

Step 3:

Threshold exceeded.

Example:

```text
CPU > 70%
```

---

Step 4:

Auto Scaling policy triggered.

---

Step 5:

Additional ECS Tasks created.

Example:

```text
Before

Task 1
Task 2
```

After Scaling:

```text
Task 1
Task 2
Task 3
Task 4
```

---

# Components of ECS Auto Scaling

## ECS Service

Auto Scaling works only with ECS Services.

Tasks launched manually cannot be automatically scaled.

---

## CloudWatch

Monitors metrics.

Examples:

```text
CPU Utilization
Memory Utilization
Request Count
```

---

## Auto Scaling Policy

Defines scaling rules.

Example:

```text
CPU > 70%
Scale Out

CPU < 30%
Scale In
```

---

# Scaling Out

Scaling Out means:

```text
Add More Tasks
```

Example:

Before:

```text
Task 1
Task 2
```

After:

```text
Task 1
Task 2
Task 3
Task 4
```

Purpose:

Handle increased traffic.

---

# Scaling In

Scaling In means:

```text
Remove Tasks
```

Example:

Before:

```text
Task 1
Task 2
Task 3
Task 4
```

After:

```text
Task 1
Task 2
```

Purpose:

Reduce cost during low traffic.

---

# Auto Scaling Metrics

## CPU Utilization

Most common metric.

Example:

```text
CPU > 70%
```

Scale Out.

---

## Memory Utilization

Example:

```text
Memory > 80%
```

Scale Out.

---

## Request Count

Example:

```text
Requests > 1000/min
```

Scale Out.

---

# Target Tracking Scaling

Most common ECS Auto Scaling method.

Example:

```text
Target CPU = 50%
```

If CPU becomes:

```text
80%
```

ECS adds tasks.

---

If CPU becomes:

```text
20%
```

ECS removes tasks.

---

## Diagram

```text
Target CPU = 50%

CPU 80%
   |
Scale Out

CPU 20%
   |
Scale In
```

---

# Step Scaling

Uses thresholds.

Example:

```text
CPU > 60%
Add 1 Task

CPU > 80%
Add 3 Tasks
```

More aggressive scaling.

---

# Scheduled Scaling

Scale based on time.

Example:

```text
Morning Traffic
9 AM
```

Scale:

```text
2 Tasks
   ↓
10 Tasks
```

Night:

```text
10 Tasks
   ↓
2 Tasks
```

---

# Real-World Example

E-Commerce Website

Normal Traffic:

```text
2 Tasks
```

During Sale Event:

```text
10000 Users
```

CPU reaches:

```text
85%
```

CloudWatch detects threshold.

Auto Scaling adds:

```text
Task 3
Task 4
Task 5
Task 6
```

Application remains responsive.

---

# Auto Scaling Workflow

```text
Users
   |
Traffic Increase
   |
CPU Increases
   |
CloudWatch Alarm
   |
Scaling Policy
   |
Add Tasks
   |
Application Healthy
```

---

# Scale-In Workflow

```text
Traffic Decreases
      |
CPU Drops
      |
CloudWatch Alarm
      |
Scaling Policy
      |
Remove Tasks
      |
Lower Cost
```

---

# Example Configuration

Desired Count:

```text
2
```

Minimum:

```text
2
```

Maximum:

```text
10
```

Result:

```text
Never Below 2 Tasks
Never Above 10 Tasks
```

---

# Interview Question

## How Does ECS Auto Scaling Work?

### Answer

ECS Auto Scaling works by monitoring application metrics through Amazon CloudWatch. When predefined thresholds such as CPU or memory utilization are exceeded, Auto Scaling policies automatically increase the number of running tasks. When utilization decreases, ECS reduces the number of tasks to optimize costs.

---

# Interview Question

## What Metrics Can Be Used for ECS Auto Scaling?

### Answer

Common metrics include:

- CPU Utilization
- Memory Utilization
- Request Count
- Custom CloudWatch Metrics

These metrics help determine when scaling actions should occur.

---

# Interview Question

## What is Target Tracking Scaling?

### Answer

Target Tracking Scaling automatically adjusts the number of tasks to maintain a target metric value, such as keeping CPU utilization around 50%.

---

# Interview Question

## What Happens When CPU Reaches 80%?

### Answer

If an Auto Scaling policy is configured, CloudWatch detects the high CPU utilization and triggers a scale-out event. ECS launches additional tasks to distribute the workload and reduce CPU usage.

---

# Best Practices

- Use Target Tracking Scaling.
- Configure minimum and maximum task counts.
- Monitor CPU and memory utilization.
- Use CloudWatch alarms.
- Combine Auto Scaling with Load Balancers.
- Test scaling policies before production deployment.

---

# Common Production Scenario

Problem:

```text
Application Slow During High Traffic
```

Investigation:

```text
CPU = 92%
```

Solution:

Configure ECS Auto Scaling.

Result:

```text
Additional Tasks Created
```

Application performance improves.

---

# Summary

ECS Auto Scaling automatically adjusts the number of running tasks based on demand.

Workflow:

```text
CloudWatch
      |
Monitor Metrics
      |
Scaling Policy
      |
Add/Remove Tasks
```

Benefits:

- High Availability
- Better Performance
- Cost Optimization
- Reduced Manual Effort

Auto Scaling is one of the most important features for running production workloads on ECS.
