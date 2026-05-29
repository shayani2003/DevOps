# Kubernetes Pod Lifecycle

## Introduction

A Pod is the smallest deployable unit in Kubernetes.

Every application running in Kubernetes runs inside a Pod.

A Pod does not remain in the same state forever. It goes through multiple phases during its lifecycle.

Understanding the Pod Lifecycle is important for:

- Troubleshooting
- Monitoring
- Interview Questions
- Production Operations

---

# What is a Pod?

A Pod is a wrapper around one or more containers.

Example:

```text
Pod
 |
 +---- Container
```

or

```text
Pod
 |
 +---- Container A
 |
 +---- Container B
```

---

# Pod Lifecycle Overview

```text
Pending
   |
   v
Running
   |
   v
Succeeded
   |
   OR
   v
Failed
```

---

# Complete Pod Lifecycle

```text
Pod Created
      |
      v
Pending
      |
      v
Container Creating
      |
      v
Running
      |
      +-----------+
      |           |
      v           v
Succeeded      Failed
```

---

# Pod Creation Workflow

```text
kubectl apply
      |
API Server
      |
Scheduler
      |
Worker Node
      |
Pod Created
```

---

# Pod Phase 1: Pending

## What is Pending?

The Pod has been accepted by Kubernetes but has not started running.

---

## Reasons

### Image Downloading

```text
Docker Image Pulling
```

---

### No Available Node

```text
CPU Not Available

Memory Not Available
```

---

### Volume Attachment

```text
Persistent Volume Not Ready
```

---

## Diagram

```text
Pod
 |
Pending
 |
Waiting For Resources
```

---

# Example

Command:

```bash
kubectl get pods
```

Output:

```text
NAME      READY   STATUS

nginx     0/1     Pending
```

---

# Pod Phase 2: Running

## What is Running?

Pod has been scheduled.

Containers are running.

At least one container is active.

---

## Diagram

```text
Pod
 |
Running
 |
Application Active
```

---

# Example

```bash
kubectl get pods
```

Output:

```text
NAME      READY   STATUS

nginx     1/1     Running
```

---

# What Happens During Running?

Kubernetes performs:

```text
Liveness Probe

Readiness Probe

Monitoring
```

---

# Pod Phase 3: Succeeded

## What is Succeeded?

All containers completed successfully.

Exit Code:

```text
0
```

---

## Common Use Cases

Batch Jobs

```text
Backup Job

Report Generation

Data Processing
```

---

## Diagram

```text
Job Started
      |
Completed
      |
Succeeded
```

---

# Example

```bash
kubectl get pods
```

Output:

```text
backup-job   0/1   Completed
```

---

# Pod Phase 4: Failed

## What is Failed?

One or more containers exited with an error.

Exit Code:

```text
Non-Zero
```

---

## Common Causes

### Application Crash

```text
Null Pointer Exception

Runtime Error
```

---

### Missing Environment Variable

```text
Database Password Missing
```

---

### Configuration Error

```text
Wrong ConfigMap
```

---

## Diagram

```text
Application Error
      |
Container Exit
      |
Failed
```

---

# Example

```bash
kubectl get pods
```

Output:

```text
app-pod   0/1   Error
```

---

# Special Pod States

# CrashLoopBackOff

One of the most common interview topics.

---

## What is CrashLoopBackOff?

Container starts.

Fails.

Restarts.

Fails again.

Kubernetes enters restart loop.

---

## Diagram

```text
Start
  |
Crash
  |
Restart
  |
Crash
  |
Restart
```

---

# Example

```bash
kubectl get pods
```

Output:

```text
app-pod   0/1   CrashLoopBackOff
```

---

# Common Causes

### Wrong Application Configuration

### Missing Secrets

### Database Connection Failure

### Application Bug

---

# Troubleshooting

```bash
kubectl logs pod-name
```

---

# ImagePullBackOff

## What is ImagePullBackOff?

Kubernetes cannot download container image.

---

## Diagram

```text
Pod
 |
Pull Image
 |
Failed
 |
ImagePullBackOff
```

---

# Example

```bash
kubectl get pods
```

Output:

```text
nginx   0/1   ImagePullBackOff
```

---

# Common Causes

### Wrong Image Name

Example:

```text
ngnix
```

instead of:

```text
nginx
```

---

### Private Repository Access Issue

---

### Network Issue

---

# Troubleshooting

Describe Pod:

```bash
kubectl describe pod pod-name
```

---

# Pod Deletion Lifecycle

Delete Pod:

```bash
kubectl delete pod nginx
```

---

Workflow:

```text
Running
   |
Termination Signal
   |
Grace Period
   |
Pod Removed
```

---

# Graceful Shutdown

Kubernetes sends:

```text
SIGTERM
```

Application gets time to shut down.

Default:

```text
30 Seconds
```

---

If application doesn't stop:

```text
SIGKILL
```

sent.

---

# Pod Lifecycle with Deployment

Deployment:

```text
Replicas = 3
```

Current:

```text
Pod1

Pod2

Pod3
```

---

Pod2 crashes:

```text
Pod1

Pod3
```

---

Deployment Controller creates:

```text
Pod4
```

---

Result:

```text
3 Running Pods
```

again.

---

# Pod Lifecycle Architecture

```text
Deployment
      |
ReplicaSet
      |
Pods
      |
Containers
```

---

# Important Commands

View Pods:

```bash
kubectl get pods
```

---

Detailed Information:

```bash
kubectl describe pod pod-name
```

---

View Logs:

```bash
kubectl logs pod-name
```

---

Delete Pod:

```bash
kubectl delete pod pod-name
```

---

Watch Pod Status:

```bash
kubectl get pods -w
```

---

# Real Production Example

Application:

```text
Spring Boot Service
```

Deployment:

```text
3 Replicas
```

One Pod crashes:

```text
CrashLoopBackOff
```

Investigation:

```bash
kubectl logs pod-name
```

Output:

```text
Database Connection Refused
```

Issue fixed.

Pod returns to:

```text
Running
```

---

# Interview Question

## What is the Lifecycle of a Kubernetes Pod?

### Answer

A Pod moves through several phases: Pending, Running, Succeeded, or Failed. Initially, the Pod is created and scheduled to a node. After containers start successfully, the Pod enters the Running state. If the workload completes successfully, it reaches Succeeded; otherwise, it enters Failed.

---

# Interview Question

## What is CrashLoopBackOff?

### Answer

CrashLoopBackOff occurs when a container repeatedly starts, crashes, and restarts. Kubernetes gradually increases the restart delay to prevent excessive restart attempts.

---

# Interview Question

## What is ImagePullBackOff?

### Answer

ImagePullBackOff occurs when Kubernetes cannot pull a container image. Common causes include incorrect image names, authentication issues, or network connectivity problems.

---

# Interview Question

## What Happens When a Pod Crashes?

### Answer

If the Pod is managed by a Deployment, ReplicaSet, StatefulSet, or DaemonSet, Kubernetes automatically creates a replacement Pod to maintain the desired state.

---

# Interview Question

## How Do You Troubleshoot a Failed Pod?

### Answer

I typically use:

```bash
kubectl get pods

kubectl describe pod <pod-name>

kubectl logs <pod-name>
```

These commands help identify scheduling issues, container failures, and application errors.

---

# Best Practices

- Use Liveness Probes.
- Use Readiness Probes.
- Monitor Pod Restarts.
- Configure Resource Limits.
- Check Logs Regularly.
- Use Centralized Logging.

---

# Memory Trick

```text
Pending
   |
Running
   |
Succeeded / Failed
```

Special States:

```text
CrashLoopBackOff
ImagePullBackOff
```

---

# Summary

Pod Lifecycle:

```text
Create
  |
Pending
  |
Running
  |
Succeeded / Failed
```

Common Troubleshooting States:

```text
CrashLoopBackOff

ImagePullBackOff
```

Important Commands:

```bash
kubectl get pods

kubectl describe pod

kubectl logs
```

Understanding the Pod Lifecycle is essential because almost every Kubernetes troubleshooting scenario begins with checking the current Pod state.
