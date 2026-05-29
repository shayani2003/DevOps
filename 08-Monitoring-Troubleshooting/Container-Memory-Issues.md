# Container Memory Issues (OOMKilled)

## Introduction

One of the most common Docker and Kubernetes interview questions is:

```text
What happens when a container consumes all available memory?
```

Understanding memory management is critical because memory-related issues are among the most common causes of production outages.

---

# What Happens When Memory is Exhausted?

When a container uses more memory than allowed:

```text
Memory Limit Reached
        |
Kernel OOM Killer Activated
        |
Container Killed
```

---

# What is OOM?

OOM stands for:

```text
Out Of Memory
```

---

# What is OOM Killer?

Linux contains a mechanism called:

```text
OOM Killer
```

Its job is to protect the operating system when memory is exhausted.

---

# Workflow

```text
System Memory Full
        |
OOM Killer
        |
Kill Process
```

---

# Container Scenario

```text
Container
     |
Consumes Memory
     |
Memory Limit Reached
     |
OOMKilled
```

---

# Docker Memory Management

## Without Limits

Example:

```bash
docker run nginx
```

Container can consume large amounts of host memory.

---

# With Limits

Example:

```bash
docker run -m 512m nginx
```

Maximum memory:

```text
512 MB
```

---

# Architecture

```text
Host
 |
Docker
 |
Container
 |
512 MB Limit
```

---

# Example

Container Limit:

```text
512 MB
```

Application Uses:

```text
700 MB
```

Result:

```text
OOMKilled
```

---

# Kubernetes Memory Management

Kubernetes provides:

```text
Requests

Limits
```

---

# Memory Request

Minimum memory guaranteed.

---

Example:

```yaml
resources:
  requests:
    memory: "256Mi"
```

---

Meaning:

```text
Scheduler Reserves
256 MB
```

---

# Memory Limit

Maximum memory allowed.

---

Example:

```yaml
resources:
  limits:
    memory: "512Mi"
```

---

Meaning:

```text
Container Cannot Exceed
512 MB
```

---

# Full Example

```yaml
resources:
  requests:
    memory: "256Mi"

  limits:
    memory: "512Mi"
```

---

# Architecture

```text
Request = 256 MB

Limit = 512 MB
```

---

# What Happens When Limit is Exceeded?

```text
Container Uses
600 MB
```

Limit:

```text
512 MB
```

Result:

```text
OOMKilled
```

---

# OOMKilled Status

Very common Kubernetes interview topic.

---

# Check Pods

```bash
kubectl get pods
```

---

Example:

```text
STATUS
OOMKilled
```

---

# Detailed Information

```bash
kubectl describe pod pod-name
```

---

Output:

```text
Reason: OOMKilled
```

---

# Viewing Logs

Before restart:

```bash
kubectl logs pod-name --previous
```

---

Useful for finding root cause.

---

# Common Causes

## Memory Leak

Application continuously consumes memory.

---

Example:

```java
Objects Never Released
```

---

Result:

```text
Memory Consumption Increases
```

---

Eventually:

```text
OOMKilled
```

---

## Large Data Processing

Example:

```text
Reading Huge File
```

into memory.

---

Result:

```text
Memory Exhaustion
```

---

## Incorrect Memory Limits

Example:

```text
Application Needs 2 GB
```

Limit:

```text
512 MB
```

---

Result:

```text
OOMKilled
```

---

## Traffic Spike

Example:

```text
100 Users
```

becomes:

```text
10,000 Users
```

---

Memory usage increases.

---

# Troubleshooting Process

## Step 1

Check Pod Status

```bash
kubectl get pods
```

---

## Step 2

Describe Pod

```bash
kubectl describe pod pod-name
```

---

## Step 3

Check Previous Logs

```bash
kubectl logs pod-name --previous
```

---

## Step 4

Check Resource Usage

```bash
kubectl top pod
```

---

## Step 5

Review Memory Limits

```bash
kubectl get pod pod-name -o yaml
```

---

# Architecture

```text
OOMKilled
     |
Logs
     |
Metrics
     |
Root Cause
```

---

# Monitoring Memory Usage

Use:

```text
Prometheus

Grafana

CloudWatch
```

---

# Architecture

```text
Container
      |
Metrics
      |
Monitoring
```

---

# Prometheus Example

Monitor:

```text
Memory Usage

CPU Usage

Container Restarts
```

---

# Grafana Dashboard

Visualize:

```text
Memory Trends

Spikes

Restarts
```

---

# Docker Troubleshooting

Check Containers:

```bash
docker ps -a
```

---

Inspect Container:

```bash
docker inspect container-id
```

---

View Memory Usage:

```bash
docker stats
```

---

Example Output

```text
MEM USAGE

450MB / 512MB
```

---

Danger:

```text
Close To Limit
```

---

# Kubernetes Best Practices

## Set Requests

Always define requests.

---

## Set Limits

Always define limits.

---

## Monitor Usage

Track memory continuously.

---

## Use HPA

Scale applications.

---

# Example

```text
High Memory Usage
       |
Horizontal Pod Autoscaler
       |
More Pods
```

---

# Memory Requests vs Limits

Very common interview question.

---

| Feature | Request | Limit |
|----------|----------|--------|
| Purpose | Guaranteed Memory | Maximum Memory |
| Scheduler Uses | Yes | No |
| OOMKill Trigger | No | Yes |

---

# Example

```yaml
requests:
  memory: "256Mi"

limits:
  memory: "512Mi"
```

---

Interpretation:

```text
Guaranteed = 256 MB

Maximum = 512 MB
```

---

# Real Production Example

Microservice:

```text
Payment Service
```

---

Problem:

```text
Pods Restarting
```

---

Investigation:

```bash
kubectl describe pod
```

Output:

```text
OOMKilled
```

---

Root Cause:

```text
Memory Limit Too Low
```

---

Fix:

```text
Increase Memory Limit
```

and optimize code.

---

# Another Production Example

Traffic Spike:

```text
Black Friday Sale
```

---

Users:

```text
1000
   |
50000
```

---

Result:

```text
Memory Usage Increased
```

---

Fix:

```text
Scale Pods
```

---

# Interview Question

## What Happens When a Container Consumes All Memory?

### Answer

When a container exceeds its configured memory limit, the Linux OOM Killer terminates the container process to protect the system. In Kubernetes, the Pod status typically shows OOMKilled.

---

# Interview Question

## What is OOMKilled?

### Answer

OOMKilled indicates that a container was terminated because it exceeded its memory limit and the Linux Out Of Memory (OOM) Killer stopped the process.

---

# Interview Question

## Difference Between Memory Requests and Limits

### Answer

Memory Requests define the guaranteed memory allocated to a container, while Memory Limits define the maximum memory the container can use before being terminated.

---

# Interview Question

## How Would You Troubleshoot OOMKilled Pods?

### Answer

I would check pod status, describe the pod, review previous logs, inspect memory metrics, verify resource requests and limits, and determine whether the issue is caused by a memory leak, traffic spike, or insufficient memory allocation.

---

# Best Practices

- Always define memory requests and limits.
- Monitor memory usage continuously.
- Use Prometheus and Grafana.
- Investigate memory leaks.
- Enable autoscaling.
- Review restart counts.
- Analyze OOMKilled events.

---

# Memory Trick

```text
Request
   |
Guaranteed Memory
```

```text
Limit
   |
Maximum Memory
```

```text
Limit Exceeded
      |
OOM Killer
      |
OOMKilled
```

---

# Summary

Container Memory Flow:

```text
Container
     |
Memory Usage
     |
Limit Exceeded
     |
OOM Killer
     |
OOMKilled
```

Key Concepts:

```text
Memory Requests

Memory Limits

OOM Killer

OOMKilled

Monitoring
```

Understanding container memory management is essential for DevOps and SRE engineers because memory-related failures are one of the most common causes of application instability in Docker and Kubernetes environments.
