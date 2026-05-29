# Kubernetes Health Probes

## Introduction

Kubernetes uses Health Probes to determine whether an application running inside a Pod is healthy and ready to serve traffic.

Health Probes help Kubernetes:

- Detect application failures
- Restart unhealthy containers
- Route traffic only to healthy Pods
- Improve application availability

---

# Why Health Probes are Needed

Suppose a container is running:

```text
Container Running
```

But the application inside has crashed.

Example:

```text
Java Process Stuck

NodeJS Event Loop Blocked

Database Connection Dead
```

From Kubernetes' perspective:

```text
Container = Running
```

But:

```text
Application = Not Working
```

Health Probes solve this problem.

---

# Types of Health Probes

Kubernetes provides:

1. Liveness Probe
2. Readiness Probe
3. Startup Probe

---

# Probe Overview

```text
Application
     |
+----+----+----+
|         |    |
v         v    v
Liveness Readiness Startup
```

---

# What is a Liveness Probe?

A Liveness Probe checks:

```text
Is the Application Alive?
```

---

# Purpose

If application becomes:

```text
Hung
Frozen
Deadlocked
```

Kubernetes:

```text
Restarts Container
```

---

# Workflow

```text
Application
      |
Liveness Check
      |
Healthy?
   /     \
 Yes      No
  |        |
Continue Restart
```

---

# Example

Application hangs.

Liveness Probe fails.

Kubernetes:

```text
Kills Container
       |
Starts New Container
```

---

# Liveness Probe Diagram

```text
Pod
 |
Container
 |
Application
 |
Liveness Probe
 |
Fail
 |
Restart
```

---

# Liveness Probe Example

```yaml
livenessProbe:

  httpGet:
    path: /health
    port: 8080

  initialDelaySeconds: 10

  periodSeconds: 5
```

---

# Request Flow

```text
GET /health

Response:

200 OK
```

Healthy.

---

Response:

```text
500 Error
```

Unhealthy.

Container restarted.

---

# What is a Readiness Probe?

A Readiness Probe checks:

```text
Is the Application Ready To Receive Traffic?
```

---

# Purpose

Determines whether traffic should be sent to a Pod.

---

# Workflow

```text
Pod Started
      |
Readiness Check
      |
Ready?
   /     \
 Yes      No
  |        |
Receive  No Traffic
Traffic
```

---

# Example

Application startup takes:

```text
30 Seconds
```

Container:

```text
Running
```

Application:

```text
Still Loading
```

Readiness Probe:

```text
Failed
```

Traffic not routed.

---

After startup completes:

```text
Readiness Probe Success
```

Traffic starts.

---

# Readiness Probe Diagram

```text
Service
   |
Readiness Probe
   |
Healthy Pod
   |
Receive Traffic
```

---

# Readiness Probe Example

```yaml
readinessProbe:

  httpGet:
    path: /ready
    port: 8080

  initialDelaySeconds: 5

  periodSeconds: 3
```

---

# What Happens When Readiness Fails?

Pod is NOT restarted.

Instead:

```text
Removed From Service Endpoints
```

Traffic stops.

---

# Readiness Failure Diagram

```text
Pod
 |
Readiness Fail
 |
Removed From Load Balancer
 |
No Traffic
```

---

# What is a Startup Probe?

Startup Probe checks:

```text
Has Application Finished Starting?
```

---

# Why Startup Probe?

Some applications need:

```text
30 Seconds
60 Seconds
120 Seconds
```

to start.

Without Startup Probe:

```text
Liveness Probe Fails
```

Container restarts continuously.

---

# Workflow

```text
Container Start
       |
Startup Probe
       |
Application Ready?
   /        \
 Yes         No
  |           |
Enable     Continue
Liveness   Checking
```

---

# Startup Probe Diagram

```text
Application Start
        |
Startup Probe
        |
Success
        |
Enable Liveness Probe
```

---

# Startup Probe Example

```yaml
startupProbe:

  httpGet:
    path: /startup
    port: 8080

  failureThreshold: 30

  periodSeconds: 10
```

---

# Probe Comparison

| Feature | Liveness | Readiness | Startup |
|-----------|-----------|-----------|-----------|
| Purpose | Check if app is alive | Check if app can receive traffic | Check startup completion |
| Failure Action | Restart Container | Remove from Service | Continue Checking |
| Traffic Impact | Indirect | Direct | No Traffic Until Ready |
| Restart Container | Yes | No | No |

---

# Health Probe Architecture

```text
User
 |
Service
 |
Readiness Probe
 |
Pod
 |
Liveness Probe
 |
Container
```

---

# Example Production Scenario

Application:

```text
Spring Boot
```

Startup Time:

```text
90 Seconds
```

Without Startup Probe:

```text
Liveness Fails
      |
Container Restart
      |
Infinite Loop
```

---

With Startup Probe:

```text
Startup Probe Waits
      |
Application Ready
      |
Enable Liveness
```

Works correctly.

---

# Probe Types

Kubernetes supports:

## HTTP Probe

```yaml
httpGet:
```

Most common.

---

## TCP Probe

```yaml
tcpSocket:
```

Checks port availability.

---

Example:

```yaml
tcpSocket:
  port: 3306
```

---

## Command Probe

```yaml
exec:
```

Runs command inside container.

---

Example

```yaml
exec:
  command:
  - cat
  - /tmp/healthy
```

---

# Complete Example

```yaml
containers:

- name: app

  image: nginx

  startupProbe:
    httpGet:
      path: /startup
      port: 80

  readinessProbe:
    httpGet:
      path: /ready
      port: 80

  livenessProbe:
    httpGet:
      path: /health
      port: 80
```

---

# Probe Lifecycle

```text
Container Start
       |
Startup Probe
       |
Readiness Probe
       |
Receive Traffic
       |
Liveness Probe
       |
Monitor Health
```

---

# Real World Example

E-Commerce Application

Requirements:

```text
Startup = 60 Seconds
```

Configuration:

```text
Startup Probe
      |
Readiness Probe
      |
Liveness Probe
```

Result:

- No premature restarts
- No failed traffic routing
- Better reliability

---

# Interview Question

## What is a Liveness Probe?

### Answer

A Liveness Probe checks whether an application is still running correctly. If the probe fails repeatedly, Kubernetes restarts the container automatically.

---

# Interview Question

## What is a Readiness Probe?

### Answer

A Readiness Probe determines whether a Pod is ready to receive traffic. If the probe fails, Kubernetes removes the Pod from Service endpoints but does not restart it.

---

# Interview Question

## What is a Startup Probe?

### Answer

A Startup Probe is used for slow-starting applications. It checks whether the application has completed startup before enabling Liveness and Readiness checks.

---

# Interview Question

## Difference Between Liveness and Readiness Probe

### Answer

Liveness Probes determine whether a container should be restarted, while Readiness Probes determine whether traffic should be routed to a Pod.

---

# Interview Question

## What Happens When a Readiness Probe Fails?

### Answer

The Pod is removed from the Service endpoints and stops receiving traffic, but the container continues running.

---

# Best Practices

- Use Readiness Probes for all production applications.
- Configure Liveness Probes carefully.
- Use Startup Probes for slow-starting applications.
- Avoid aggressive probe timings.
- Monitor probe failures.
- Test probes before production deployment.

---

# Memory Trick

```text
Liveness
    |
Alive?

Readiness
    |
Ready For Traffic?

Startup
    |
Finished Starting?
```

---

# Summary

Kubernetes Health Probes:

```text
Startup Probe
      |
Readiness Probe
      |
Liveness Probe
```

Liveness:

```text
Fail -> Restart Container
```

Readiness:

```text
Fail -> Stop Traffic
```

Startup:

```text
Wait Until Application Starts
```

Health Probes are essential for building reliable, highly available Kubernetes applications and are one of the most frequently asked Kubernetes interview topics.
