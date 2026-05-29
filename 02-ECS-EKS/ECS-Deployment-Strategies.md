# ECS Deployment Strategies

## Introduction

A deployment strategy defines how a new version of an application is released in ECS.

Goals:

- Minimize downtime
- Reduce deployment risk
- Enable quick rollback
- Maintain application availability

---

# Why Deployment Strategies Matter

Without a strategy:

```text
Deploy New Version
       |
Application Fails
       |
Production Outage
```

---

With a strategy:

```text
Deploy New Version
       |
Validate
       |
Switch Traffic
       |
Success
```

---

# ECS Deployment Types

Common deployment strategies:

1. Rolling Deployment
2. Blue-Green Deployment
3. Canary Deployment

---

# 1. Rolling Deployment

## What is Rolling Deployment?

Tasks are replaced gradually.

Old tasks are stopped one by one.

New tasks are started one by one.

---

## Architecture

Before:

```text
Task 1 v1
Task 2 v1
Task 3 v1
Task 4 v1
```

---

Step 1:

```text
Task 1 v2
Task 2 v1
Task 3 v1
Task 4 v1
```

---

Step 2:

```text
Task 1 v2
Task 2 v2
Task 3 v1
Task 4 v1
```

---

Final:

```text
Task 1 v2
Task 2 v2
Task 3 v2
Task 4 v2
```

---

## Advantages

- No downtime
- Simple
- Resource efficient

---

## Disadvantages

- Rollback can take time
- Multiple versions run simultaneously

---

# Rolling Deployment in ECS

Default ECS deployment strategy.

Configuration:

```text
Minimum Healthy Percent

Maximum Percent
```

Example:

```text
Desired Tasks = 4

Minimum Healthy = 50%

Maximum = 200%
```

---

# 2. Blue-Green Deployment

## What is Blue-Green Deployment?

Two identical environments exist.

Blue:

```text
Current Production
```

Green:

```text
New Version
```

---

## Architecture

```text
                 Users
                    |
                    v
             Load Balancer
                /      \
               /        \
          Blue         Green
        Version 1    Version 2
```

---

# Deployment Process

Step 1:

Production uses:

```text
Blue Environment
```

---

Step 2:

Deploy new version to:

```text
Green Environment
```

---

Step 3:

Testing completed.

---

Step 4:

Traffic switched:

```text
Blue
  ↓
Green
```

---

# Rollback

If Green fails:

```text
Traffic
   |
Blue Environment
```

Rollback is immediate.

---

## Advantages

- Zero downtime
- Instant rollback
- Safer deployment

---

## Disadvantages

- Double infrastructure cost
- More resources required

---

# Blue-Green Deployment with ECS

Usually implemented using:

```text
ECS
 +
CodeDeploy
 +
Application Load Balancer
```

---

## Architecture

```text
Users
  |
ALB
 / \
/   \
Blue Green
```

AWS CodeDeploy controls traffic shifting.

---

# 3. Canary Deployment

## What is Canary Deployment?

Deploy new version to a small percentage of users first.

---

## Example

100 Users

```text
90 Users → Version 1

10 Users → Version 2
```

---

If successful:

```text
50 Users → Version 2
```

---

Then:

```text
100 Users → Version 2
```

---

# Canary Workflow

```text
5% Traffic
      |
Monitor
      |
25% Traffic
      |
Monitor
      |
50% Traffic
      |
Monitor
      |
100% Traffic
```

---

## Advantages

- Lowest deployment risk
- Real user testing
- Easy monitoring

---

## Disadvantages

- Complex implementation
- Requires traffic routing

---

# ECS + Canary Deployment

Often implemented using:

```text
ALB
CodeDeploy
CloudWatch
```

Traffic gradually shifts to new tasks.

---

# ECS Deployment Workflow

```text
Developer
      |
Docker Image
      |
ECR
      |
ECS Service
      |
Deployment Strategy
      |
Production
```

---

# AWS CodeDeploy

AWS service used for:

- Blue-Green Deployment
- Canary Deployment
- Traffic Shifting
- Automatic Rollback

---

## Architecture

```text
Developer
      |
CodeDeploy
      |
ECS
      |
ALB
      |
Users
```

---

# Automatic Rollback

If deployment fails:

```text
Health Check Failed
```

CodeDeploy automatically:

```text
Redirect Traffic
```

to previous version.

---

# Health Checks

Health checks verify application status.

Example:

```text
HTTP 200 OK
```

Healthy.

---

Failure:

```text
HTTP 500
```

Unhealthy.

---

# Zero Downtime Deployment

Deployment should not interrupt users.

Requirements:

```text
Multiple Tasks
+
Load Balancer
+
Health Checks
```

---

## Architecture

```text
Users
   |
Load Balancer
   |
+----+----+----+
|    |    |    |
T1   T2   T3
```

During deployment:

```text
T4
T5
T6
```

added before old tasks removed.

---

# Real Production Example

E-commerce website:

Normal:

```text
Version 1
```

New Release:

```text
Version 2
```

Deploy using:

```text
Blue-Green
```

If monitoring detects errors:

```text
Rollback to Version 1
```

within seconds.

---

# Comparison Table

| Feature | Rolling | Blue-Green | Canary |
|----------|----------|------------|---------|
| Downtime | No | No | No |
| Rollback Speed | Medium | Fast | Fast |
| Cost | Low | High | Medium |
| Complexity | Low | Medium | High |
| Risk | Medium | Low | Very Low |

---

# Interview Question

## What Deployment Strategies Are Supported in ECS?

### Answer

ECS commonly supports Rolling Deployments, Blue-Green Deployments, and Canary Deployments. Rolling Deployments gradually replace tasks, Blue-Green Deployments use two environments for safer releases, and Canary Deployments expose new versions to a small percentage of users before full rollout.

---

# Interview Question

## How Does Blue-Green Deployment Work in ECS?

### Answer

Blue-Green Deployment uses two identical environments. The current production environment (Blue) serves users while the new version is deployed to Green. After validation, traffic is switched to Green. If issues occur, traffic can immediately be redirected back to Blue.

---

# Interview Question

## Why Use Canary Deployment?

### Answer

Canary Deployment reduces deployment risk by exposing a new version to a small subset of users first. This allows teams to monitor application behavior before rolling out the update to all users.

---

# Best Practices

- Use Health Checks.
- Configure Automatic Rollback.
- Use AWS CodeDeploy.
- Monitor deployments with CloudWatch.
- Test rollback procedures.
- Prefer Blue-Green for critical applications.

---

# Summary

Deployment Strategies in ECS:

```text
Rolling
   |
Blue-Green
   |
Canary
```

Rolling:
- Simple
- Resource efficient

Blue-Green:
- Fast rollback
- Safer deployment

Canary:
- Lowest risk
- Gradual rollout

For production environments, Blue-Green and Canary deployments are generally preferred because they provide safer releases and better rollback capabilities.
