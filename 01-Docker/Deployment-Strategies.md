# Deployment Strategies

## Introduction

A deployment strategy defines how a new version of an application is released into production.

Goals:

- Minimize downtime
- Reduce risk
- Enable rollback
- Improve user experience

---

# Why Deployment Strategies Matter

Without a deployment strategy:

- Service interruption
- Failed releases
- Difficult rollbacks

With a deployment strategy:

- Controlled releases
- Easy rollback
- Better reliability

---

# Types of Deployment Strategies

1. Recreate Deployment
2. Rolling Deployment
3. Blue-Green Deployment
4. Canary Deployment

---

# 1. Recreate Deployment

## How It Works

Old version is stopped first.

New version is started afterward.

Diagram:

```text
Version 1 Running
        |
        v
Version 1 Stopped
        |
        v
Version 2 Started
```

---

## Advantages

- Very simple
- Easy to implement

---

## Disadvantages

- Downtime occurs
- Users experience interruption

---

## Use Cases

- Internal tools
- Non-critical applications

---

# 2. Rolling Deployment

## How It Works

Instances are updated gradually.

Example:

```text
Before

Pod1 v1
Pod2 v1
Pod3 v1
Pod4 v1
```

Update Process:

```text
Step 1

Pod1 v2
Pod2 v1
Pod3 v1
Pod4 v1
```

```text
Step 2

Pod1 v2
Pod2 v2
Pod3 v1
Pod4 v1
```

```text
Step 3

Pod1 v2
Pod2 v2
Pod3 v2
Pod4 v2
```

---

## Advantages

- No downtime
- Efficient resource usage
- Easy implementation

---

## Disadvantages

- Rollback may take time
- Multiple versions run simultaneously

---

## Kubernetes Example

```bash
kubectl rollout restart deployment app
```

---

# 3. Blue-Green Deployment

## How It Works

Two identical environments exist.

Blue = Current Production

Green = New Version

Diagram:

```text
             Users
                |
                v
         Load Balancer
            /      \
           /        \
        Blue       Green
       Version     Version
```

---

## Deployment Process

Step 1

```text
Users
  |
Blue Environment
```

Step 2

Deploy new version:

```text
Blue Environment
Green Environment
```

Step 3

Testing completed.

Switch traffic:

```text
Users
  |
Green Environment
```

---

## Advantages

- Instant rollback
- No downtime
- Safe deployment

---

## Disadvantages

- Double infrastructure cost
- Requires duplicate environment

---

## Rollback

If Green fails:

```text
Traffic
   |
   v
Blue Environment
```

Rollback is immediate.

---

# 4. Canary Deployment

## How It Works

Release application to a small percentage of users first.

Diagram:

```text
100 Users

90 Users --> Version 1

10 Users --> Version 2
```

If successful:

```text
50% Users --> Version 2
```

Then:

```text
100% Users --> Version 2
```

---

## Deployment Flow

```text
5% Traffic
      |
     Success
      |
25% Traffic
      |
     Success
      |
50% Traffic
      |
     Success
      |
100% Traffic
```

---

## Advantages

- Reduced deployment risk
- Real user testing
- Easy monitoring

---

## Disadvantages

- More complex
- Requires traffic routing

---

## Common Tools

- Kubernetes
- Istio
- AWS ALB
- AWS App Mesh

---

# Comparison Table

| Feature | Recreate | Rolling | Blue-Green | Canary |
|----------|-----------|----------|------------|---------|
| Downtime | Yes | No | No | No |
| Rollback Speed | Slow | Medium | Fast | Fast |
| Complexity | Low | Medium | Medium | High |
| Cost | Low | Low | High | Medium |

---

# Zero Downtime Deployment

## What Is It?

Users should not experience service interruption during deployment.

---

## How To Achieve It

### Load Balancer

Distributes traffic.

---

### Health Checks

Only healthy instances receive traffic.

---

### Multiple Instances

At least two application instances.

Example:

```text
Load Balancer
      |
+-----------+
| Instance1 |
| Instance2 |
+-----------+
```

---

### Rolling Updates

Update one instance at a time.

---

### Blue-Green Deployment

Switch traffic after validation.

---

### Canary Deployment

Gradual traffic shifting.

---

# Real Interview Question

## Difference Between Blue-Green, Rolling and Canary Deployment

### Answer

### Rolling Deployment

Updates instances gradually.

Advantages:

- No downtime
- Lower resource usage

---

### Blue-Green Deployment

Maintains two environments.

Advantages:

- Instant rollback
- Safe deployments

---

### Canary Deployment

Releases application to a small group of users first.

Advantages:

- Lowest deployment risk
- Real user validation

---

# Real Production Example

Suppose Netflix releases a new feature.

Instead of sending it to millions of users:

```text
1% Users
    |
Monitor Errors
    |
10% Users
    |
Monitor Errors
    |
50% Users
    |
Monitor Errors
    |
100% Users
```

This is Canary Deployment.

---

# Best Practices

- Always use health checks
- Enable monitoring
- Keep rollback plan ready
- Automate deployments
- Use load balancers
- Monitor logs and metrics

---

# Summary

## Recreate

- Simple
- Causes downtime

## Rolling

- Gradual update
- Most common

## Blue-Green

- Two environments
- Instant rollback

## Canary

- Small user group first
- Lowest risk

For production systems, Blue-Green and Canary deployments are preferred because they provide safer releases and faster rollback capabilities.
