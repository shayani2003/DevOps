# Zero Downtime Deployment

## What is Zero Downtime Deployment?

Zero Downtime Deployment is a deployment approach where a new version of an application is released without interrupting service for users.

Users continue using the application while the deployment occurs.

---

# Why Zero Downtime is Important

Without Zero Downtime Deployment:

```text
Deploy New Version
       |
Stop Old Application
       |
Users Cannot Access Service
       |
Start New Version
```

Result:

```text
Downtime
```

---

With Zero Downtime Deployment:

```text
Deploy New Version
       |
Run Both Versions
       |
Switch Traffic
       |
Remove Old Version
```

Result:

```text
No Downtime
```

---

# Real World Example

Imagine:

```text
Amazon
Netflix
Google
Facebook
```

cannot stop serving users during deployments.

Even a few minutes of downtime can cause:

- Revenue loss
- Customer dissatisfaction
- SLA violations

Therefore, Zero Downtime Deployment is essential.

---

# Requirements for Zero Downtime Deployment

## 1. Multiple Application Instances

Bad:

```text
Application
     |
Single Instance
```

If deployment starts:

```text
Service Unavailable
```

---

Good:

```text
Application
      |
+-----+-----+
|           |
Instance1 Instance2
```

One instance remains available while another is updated.

---

# 2. Load Balancer

Load Balancer distributes traffic.

Diagram:

```text
Users
  |
Load Balancer
  |
+----+----+----+
|    |    |    |
A1   A2   A3
```

Benefits:

- Traffic distribution
- High availability
- Health monitoring

---

# 3. Health Checks

Before receiving traffic, the new version must pass health checks.

Healthy:

```text
HTTP 200
```

Unhealthy:

```text
HTTP 500
```

---

## Deployment Flow

```text
New Version
      |
Health Check
      |
Pass
      |
Receive Traffic
```

---

# 4. Deployment Strategy

Common strategies:

- Rolling Deployment
- Blue-Green Deployment
- Canary Deployment

---

# Rolling Deployment

Old tasks replaced gradually.

Example:

Before:

```text
Task1 v1
Task2 v1
Task3 v1
Task4 v1
```

---

Step 1:

```text
Task1 v2
Task2 v1
Task3 v1
Task4 v1
```

---

Step 2:

```text
Task1 v2
Task2 v2
Task3 v1
Task4 v1
```

---

Final:

```text
Task1 v2
Task2 v2
Task3 v2
Task4 v2
```

Users always have available instances.

---

# Blue-Green Deployment

Two environments:

```text
Blue = Current Version

Green = New Version
```

---

Architecture:

```text
Users
  |
Load Balancer
 / \
/   \
Blue Green
```

---

Deployment Process

Step 1:

```text
Traffic → Blue
```

---

Step 2:

Deploy new version to Green.

---

Step 3:

Run tests.

---

Step 4:

Switch traffic.

```text
Blue
  ↓
Green
```

---

Result:

```text
No Downtime
```

---

# Canary Deployment

New version released gradually.

Example:

```text
95% Users → Version 1

5% Users → Version 2
```

---

If healthy:

```text
50% Users → Version 2
```

---

Then:

```text
100% Users → Version 2
```

---

# ECS Zero Downtime Deployment

ECS provides:

- Services
- Load Balancers
- Health Checks
- Auto Scaling

to support zero downtime deployments.

---

## ECS Architecture

```text
Users
  |
Application Load Balancer
  |
ECS Service
  |
+----+----+----+
|    |    |    |
T1   T2   T3
```

---

During deployment:

```text
Users
  |
ALB
  |
+----+----+----+----+----+----+
|    |    |    |    |    |    |
T1   T2   T3   T4   T5   T6
```

New tasks start first.

Old tasks removed later.

---

# ECS Rolling Update

Default ECS deployment.

Configuration:

```text
Desired Count = 4

Minimum Healthy Percent = 100

Maximum Percent = 200
```

---

Meaning:

Before removing old tasks:

```text
New Tasks Started First
```

Result:

```text
Zero Downtime
```

---

# Health Check Example

ALB sends request:

```text
GET /health
```

Healthy Response:

```text
200 OK
```

Task receives traffic.

---

Unhealthy Response:

```text
500 Error
```

Task removed from load balancer.

---

# Auto Scaling and Zero Downtime

During traffic spikes:

```text
CPU > 80%
```

ECS launches:

```text
Additional Tasks
```

Maintains availability.

---

# Automatic Rollback

Suppose deployment fails.

Health Check:

```text
FAILED
```

AWS CodeDeploy:

```text
Rollback Triggered
```

Traffic redirected back to stable version.

---

# Complete Zero Downtime Workflow

```text
Developer
    |
Deploy New Version
    |
Create New Tasks
    |
Run Health Checks
    |
Pass
    |
Shift Traffic
    |
Remove Old Tasks
    |
Deployment Complete
```

---

# Common Mistakes

## Single Instance Deployment

```text
1 Instance
```

If restarted:

```text
Downtime
```

---

## No Health Checks

Traffic reaches broken containers.

---

## No Load Balancer

Users may hit unhealthy instances.

---

## No Rollback Strategy

Failed deployment impacts users.

---

# Real Production Example

E-commerce Application:

Current:

```text
Version 1
```

New Release:

```text
Version 2
```

Deployment:

```text
Blue-Green
```

Steps:

1. Deploy Green.
2. Run tests.
3. Shift traffic.
4. Monitor.
5. Remove Blue.

Users experience:

```text
0 Seconds Downtime
```

---

# Interview Question

## What is Zero Downtime Deployment?

### Answer

Zero Downtime Deployment is a deployment strategy where a new version of an application is released without interrupting service availability. This is achieved using load balancers, multiple instances, health checks, and deployment strategies such as Rolling, Blue-Green, or Canary deployments.

---

# Interview Question

## How Would You Configure Zero Downtime Deployment in ECS?

### Answer

I would use an ECS Service behind an Application Load Balancer. Multiple tasks would be running, and health checks would verify new tasks before receiving traffic. I would configure Rolling Updates or Blue-Green Deployments using AWS CodeDeploy to ensure traffic is only routed to healthy instances, preventing downtime during deployment.

---

# Interview Question

## Why Are Health Checks Important?

### Answer

Health checks ensure that only healthy application instances receive traffic. During deployment, new tasks must pass health checks before the load balancer routes user requests to them.

---

# Best Practices

- Use Application Load Balancers.
- Run multiple tasks.
- Configure Health Checks.
- Enable Auto Scaling.
- Use Blue-Green Deployments for critical applications.
- Implement Automatic Rollback.
- Monitor deployments with CloudWatch.

---

# Summary

Zero Downtime Deployment ensures users experience no service interruption during releases.

Key Components:

```text
Load Balancer
      +
Health Checks
      +
Multiple Instances
      +
Deployment Strategy
```

Common Strategies:

- Rolling Deployment
- Blue-Green Deployment
- Canary Deployment

For ECS, combining ECS Services, ALB, Health Checks, and CodeDeploy provides a reliable Zero Downtime Deployment solution suitable for production environments.
