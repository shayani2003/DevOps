# Kubernetes Troubleshooting

## Introduction

Troubleshooting is one of the most important skills for a DevOps Engineer.

Interviewers often ask scenario-based questions such as:

- Pod is not starting. What will you do?
- Pod is in CrashLoopBackOff. How will you troubleshoot?
- Service is not reachable. What will you check?
- Deployment is not creating Pods. Why?

A systematic troubleshooting approach is essential.

---

# Kubernetes Troubleshooting Workflow

```text
Problem Reported
       |
       v
Identify Component
       |
       v
Check Status
       |
       v
Inspect Logs
       |
       v
Find Root Cause
       |
       v
Fix Issue
       |
       v
Verify Solution
```

---

# Most Important Troubleshooting Commands

View Pods:

```bash
kubectl get pods
```

---

Pod Details:

```bash
kubectl describe pod <pod-name>
```

---

Pod Logs:

```bash
kubectl logs <pod-name>
```

---

Deployment Details:

```bash
kubectl describe deployment <deployment-name>
```

---

Service Details:

```bash
kubectl describe service <service-name>
```

---

View Events:

```bash
kubectl get events
```

---

Node Status:

```bash
kubectl get nodes
```

---

# Scenario 1: Pod is Stuck in Pending State

## Symptoms

```text
STATUS = Pending
```

---

Check:

```bash
kubectl describe pod <pod-name>
```

---

Possible Causes

### Insufficient CPU

```text
0/3 nodes available
Insufficient CPU
```

---

### Insufficient Memory

```text
0/3 nodes available
Insufficient Memory
```

---

### Persistent Volume Issue

```text
Volume Not Bound
```

---

### Node Selector Problem

Pod requests a node that does not exist.

---

## Troubleshooting Workflow

```text
Pending
   |
Describe Pod
   |
Check Events
   |
Identify Resource Issue
```

---

# Scenario 2: Pod in CrashLoopBackOff

## Symptoms

```text
CrashLoopBackOff
```

---

## Meaning

Application starts.

```text
Start
  |
Crash
  |
Restart
  |
Crash
```

Repeatedly.

---

## Check Logs

```bash
kubectl logs <pod-name>
```

---

## Common Causes

### Application Error

```text
Null Pointer Exception
```

---

### Missing Environment Variable

```text
DATABASE_URL missing
```

---

### Database Connection Failure

```text
Connection Refused
```

---

### Incorrect Secret

```text
Invalid Password
```

---

## Troubleshooting Workflow

```text
CrashLoopBackOff
      |
kubectl logs
      |
Find Error
      |
Fix Application
```

---

# Scenario 3: ImagePullBackOff

## Symptoms

```text
ImagePullBackOff
```

---

## Meaning

Kubernetes cannot pull image.

---

Check:

```bash
kubectl describe pod <pod-name>
```

---

## Common Causes

### Wrong Image Name

Bad:

```text
ngnix
```

Good:

```text
nginx
```

---

### Private Registry Authentication Failure

---

### Image Does Not Exist

---

### Network Problem

---

## Troubleshooting

```bash
kubectl describe pod pod-name
```

Look at:

```text
Events Section
```

---

# Scenario 4: Pod Running But Application Not Working

## Symptoms

```text
Pod = Running

Application = Unreachable
```

---

Check:

```bash
kubectl logs pod-name
```

---

Possible Causes

### Application Crash

### Wrong Port

### Readiness Probe Failure

### Database Connectivity Issue

---

## Workflow

```text
Running Pod
      |
Check Logs
      |
Check Probes
      |
Verify Application
```

---

# Scenario 5: Service Not Reachable

## Symptoms

```text
Pod Running

Service Not Working
```

---

Check Services:

```bash
kubectl get svc
```

---

Describe Service:

```bash
kubectl describe svc service-name
```

---

## Common Causes

### Wrong Selector

Service:

```yaml
selector:
  app: nginx
```

Pod:

```yaml
labels:
  app: apache
```

Mismatch.

---

### No Endpoints

Check:

```bash
kubectl get endpoints
```

Output:

```text
No Endpoints
```

---

### Wrong Port Mapping

Service:

```text
80
```

Container:

```text
8080
```

Mismatch.

---

## Troubleshooting Workflow

```text
Service
   |
Check Selector
   |
Check Endpoints
   |
Check Ports
```

---

# Scenario 6: Deployment Not Creating Pods

## Symptoms

```text
Deployment Exists

No Pods Created
```

---

Check:

```bash
kubectl describe deployment deployment-name
```

---

## Possible Causes

### Image Pull Failure

### Resource Constraints

### Invalid YAML

### ReplicaSet Issue

---

Check ReplicaSets:

```bash
kubectl get rs
```

---

# Scenario 7: Node Not Ready

## Symptoms

```text
STATUS = NotReady
```

---

Check:

```bash
kubectl describe node node-name
```

---

## Common Causes

### Kubelet Down

### Disk Full

### Network Issue

### Resource Exhaustion

---

Check:

```bash
systemctl status kubelet
```

---

# Scenario 8: DNS Resolution Failure

## Symptoms

Application cannot find service.

Example:

```text
backend-service
```

not resolving.

---

Check:

```bash
kubectl get pods -n kube-system
```

Verify:

```text
CoreDNS Running
```

---

Test DNS:

```bash
nslookup backend-service
```

inside Pod.

---

# Scenario 9: Persistent Volume Not Mounting

## Symptoms

```text
Pod Pending
```

---

Check:

```bash
kubectl get pv

kubectl get pvc
```

---

Describe:

```bash
kubectl describe pvc pvc-name
```

---

Common Causes

### Storage Class Issue

### Capacity Mismatch

### Access Mode Mismatch

---

# Scenario 10: High CPU or Memory Usage

## Check Metrics

```bash
kubectl top pod
```

---

Node Metrics:

```bash
kubectl top node
```

---

## Possible Causes

### Memory Leak

### Infinite Loop

### High Traffic

---

# Kubernetes Troubleshooting Architecture

```text
Problem
   |
Pod?
Service?
Deployment?
Node?
Storage?
Network?
   |
Logs + Describe + Events
   |
Root Cause
```

---

# Important Debug Commands

Get Pods:

```bash
kubectl get pods -A
```

---

Describe Pod:

```bash
kubectl describe pod pod-name
```

---

Logs:

```bash
kubectl logs pod-name
```

---

Previous Logs:

```bash
kubectl logs pod-name --previous
```

---

Execute Inside Pod:

```bash
kubectl exec -it pod-name -- sh
```

---

Events:

```bash
kubectl get events
```

---

# Real Production Example

Problem:

```text
Application Down
```

---

Check:

```bash
kubectl get pods
```

Output:

```text
CrashLoopBackOff
```

---

Logs:

```bash
kubectl logs app-pod
```

Output:

```text
Database Connection Refused
```

---

Fix:

```text
Database Configuration Updated
```

---

Result:

```text
Pod Running
```

---

# Interview Question

## Pod is Not Starting. What Will You Do?

### Answer

I first check the Pod status using:

```bash
kubectl get pods
```

Then I inspect detailed information using:

```bash
kubectl describe pod <pod-name>
```

and review logs using:

```bash
kubectl logs <pod-name>
```

This helps identify scheduling issues, image pull failures, resource constraints, or application errors.

---

# Interview Question

## How Do You Troubleshoot CrashLoopBackOff?

### Answer

I use:

```bash
kubectl logs <pod-name>
```

to inspect application logs and:

```bash
kubectl describe pod <pod-name>
```

to review events. Common causes include application crashes, missing environment variables, incorrect Secrets, and database connectivity issues.

---

# Interview Question

## Service is Not Reachable. What Will You Check?

### Answer

I verify the Service configuration, selectors, endpoints, and port mappings using:

```bash
kubectl get svc

kubectl describe svc

kubectl get endpoints
```

I also ensure the target Pods are healthy and running.

---

# Interview Question

## What Commands Do You Use Most for Troubleshooting?

### Answer

Most frequently used commands:

```bash
kubectl get pods

kubectl describe pod

kubectl logs

kubectl get events

kubectl get svc

kubectl get endpoints

kubectl top pod
```

---

# Best Practices

- Always check Events.
- Read application logs first.
- Verify Service selectors.
- Monitor resource utilization.
- Use Health Probes.
- Enable centralized logging.
- Monitor cluster health continuously.

---

# Memory Trick

```text
Pod Issue
   |
Describe
   |
Logs
   |
Events
   |
Fix
```

---

# Summary

Most Common Kubernetes Problems:

```text
Pending

CrashLoopBackOff

ImagePullBackOff

Service Not Reachable

Node Not Ready

Volume Mount Failure
```

Golden Troubleshooting Commands:

```bash
kubectl get pods

kubectl describe pod

kubectl logs

kubectl get events

kubectl get svc
```

A structured troubleshooting approach is one of the most valuable Kubernetes skills and is heavily tested in DevOps, SRE, and Platform Engineering interviews.
