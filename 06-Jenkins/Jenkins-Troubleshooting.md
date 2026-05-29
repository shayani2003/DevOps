# Jenkins Troubleshooting

## Introduction

Troubleshooting Jenkins is one of the most important DevOps skills.

Interviewers frequently ask:

- Build failed. What will you check?
- Jenkins Agent is offline. How do you fix it?
- Pipeline is stuck. What will you do?
- Jenkins is slow. How do you troubleshoot it?

A structured troubleshooting approach helps quickly identify and resolve issues.

---

# Jenkins Troubleshooting Workflow

```text
Issue Reported
      |
Identify Component
      |
Check Logs
      |
Find Root Cause
      |
Apply Fix
      |
Verify Solution
```

---

# Key Jenkins Components to Check

```text
Jenkins Controller

Jenkins Agents

Pipeline

Git Repository

Docker

Kubernetes

Plugins
```

---

# Important Troubleshooting Commands

Check Jenkins Service:

```bash
systemctl status jenkins
```

---

Start Jenkins:

```bash
systemctl start jenkins
```

---

Restart Jenkins:

```bash
systemctl restart jenkins
```

---

View Jenkins Logs:

```bash
journalctl -u jenkins
```

---

Docker Jenkins Logs:

```bash
docker logs jenkins
```

---

# Scenario 1: Jenkins Service Not Starting

## Symptoms

```text
Jenkins UI Not Accessible
```

---

Check Service:

```bash
systemctl status jenkins
```

---

Possible Causes

### Java Not Installed

Jenkins requires Java.

Check:

```bash
java -version
```

---

### Port Already Used

Check:

```bash
netstat -tulnp | grep 8080
```

or

```bash
ss -tulnp | grep 8080
```

---

### Invalid Jenkins Configuration

Review logs:

```bash
journalctl -u jenkins
```

---

# Troubleshooting Flow

```text
Jenkins Down
      |
Check Service
      |
Check Logs
      |
Fix Root Cause
```

---

# Scenario 2: Jenkins Build Failed

## Symptoms

```text
Build = Failed
```

---

Check:

```text
Console Output
```

---

## Common Causes

### Compilation Error

Example:

```text
Java Syntax Error
```

---

### Dependency Issue

Example:

```text
Maven Dependency Missing
```

---

### Test Failure

Example:

```text
Unit Test Failed
```

---

### Permission Issue

Example:

```text
Permission Denied
```

---

# Troubleshooting Workflow

```text
Failed Build
      |
Console Output
      |
Error Message
      |
Fix Problem
```

---

# Scenario 3: Jenkins Agent Offline

## Symptoms

```text
Agent Status = Offline
```

---

Check Agent Page:

```text
Manage Jenkins
      |
Nodes
```

---

## Common Causes

### Network Connectivity Issue

Controller cannot reach Agent.

---

### SSH Failure

Check:

```bash
ssh agent-server
```

---

### Agent Service Down

Verify:

```bash
systemctl status jenkins-agent
```

---

### Disk Full

Check:

```bash
df -h
```

---

# Troubleshooting Workflow

```text
Agent Offline
      |
Check Network
      |
Check SSH
      |
Check Agent Logs
```

---

# Scenario 4: Pipeline Stuck

## Symptoms

```text
Pipeline Running Forever
```

---

Possible Causes

### Waiting for Input

Example:

```groovy
input "Approve Deployment?"
```

---

### Infinite Loop

Bad Script:

```groovy
while(true) {

}
```

---

### External Service Delay

Example:

```text
Docker Registry Slow

GitHub Timeout
```

---

# Troubleshooting

Check:

```text
Stage Logs
```

Find:

```text
Last Executed Step
```

---

# Scenario 5: Git Checkout Failure

## Symptoms

```text
Failed To Clone Repository
```

---

Check:

```text
Git Plugin Configuration
```

---

Possible Causes

### Wrong Repository URL

---

### Invalid Credentials

---

### Network Issue

---

### GitHub Access Denied

---

# Example Error

```text
Authentication Failed
```

---

Solution:

```text
Update Credentials
```

---

# Scenario 6: Docker Build Failure

## Symptoms

```text
Docker Build Failed
```

---

Check:

```bash
docker build .
```

manually.

---

Possible Causes

### Dockerfile Error

### Missing File

### Docker Daemon Down

---

Check:

```bash
systemctl status docker
```

---

# Scenario 7: Kubernetes Deployment Failure

## Symptoms

```text
Deployment Stage Failed
```

---

Check:

```bash
kubectl get pods
```

---

Possible Causes

### Wrong Kubeconfig

### Image Pull Failure

### RBAC Permission Issue

### Invalid YAML

---

Check:

```bash
kubectl describe pod
```

---

# Scenario 8: Jenkins Running Slowly

## Symptoms

```text
Build Queue Increasing
```

---

Possible Causes

### Insufficient CPU

Check:

```bash
top
```

---

### Insufficient Memory

Check:

```bash
free -h
```

---

### Too Many Plugins

Unused plugins consume resources.

---

### Too Few Agents

Controller overloaded.

---

# Troubleshooting Flow

```text
Slow Jenkins
      |
CPU?
Memory?
Plugins?
Agents?
```

---

# Scenario 9: Plugin Failure

## Symptoms

```text
Feature Stops Working
```

after plugin update.

---

Check:

```text
Manage Jenkins
      |
System Logs
```

---

Possible Causes

### Plugin Version Conflict

### Unsupported Plugin

### Dependency Issue

---

Solution:

```text
Rollback Plugin
```

or

```text
Upgrade Dependencies
```

---

# Scenario 10: Webhook Not Triggering Builds

## Symptoms

```text
Git Push

No Jenkins Build
```

---

Check:

```text
GitHub Webhook Delivery Logs
```

---

Possible Causes

### Wrong Webhook URL

### Jenkins Not Reachable

### Firewall Issue

### Missing Plugin

---

# Webhook Flow

```text
Git Push
    |
Webhook
    |
Jenkins
```

If broken:

```text
Build Not Triggered
```

---

# Most Important Jenkins Logs

Controller Logs:

```bash
journalctl -u jenkins
```

---

Docker Jenkins:

```bash
docker logs jenkins
```

---

Pipeline Logs:

```text
Console Output
```

---

Agent Logs:

```text
Node Logs
```

---

# Real Production Example

Problem:

```text
Deployment Pipeline Failed
```

---

Check:

```text
Pipeline Console Output
```

Error:

```text
kubectl: permission denied
```

---

Root Cause:

```text
Invalid Kubernetes Credentials
```

---

Fix:

```text
Update Jenkins Credentials
```

---

Pipeline Successful.

---

# Troubleshooting Decision Tree

```text
Build Failed?
      |
Console Logs
      |
Fix Error
```

---

```text
Agent Offline?
      |
Network
SSH
Disk
Logs
```

---

```text
Pipeline Stuck?
      |
Current Stage
      |
External Dependency
```

---

# Interview Question

## A Jenkins Build Failed. What Will You Check?

### Answer

I first review the Console Output to identify the exact error message. Then I investigate build logs, dependency issues, test failures, permissions, and configuration problems based on the error details.

---

# Interview Question

## Jenkins Agent is Offline. How Will You Troubleshoot?

### Answer

I verify network connectivity, SSH access, agent service status, available disk space, and Jenkins node logs. I also ensure the Controller can communicate with the Agent.

---

# Interview Question

## Jenkins Pipeline is Stuck. What Will You Do?

### Answer

I inspect the currently executing stage, review pipeline logs, check for input steps, infinite loops, or external dependency delays such as Docker, GitHub, or Kubernetes connectivity issues.

---

# Interview Question

## Jenkins is Running Slowly. What Could Be the Reasons?

### Answer

Common reasons include high CPU usage, insufficient memory, too many plugins, overloaded Controllers, insufficient Agents, or large build queues.

---

# Interview Question

## Where Do You Check Jenkins Logs?

### Answer

On Linux systems, Jenkins logs can be viewed using:

```bash
journalctl -u jenkins
```

For containerized Jenkins:

```bash
docker logs jenkins
```

Pipeline-specific issues can be investigated through the Jenkins Console Output.

---

# Best Practices

- Monitor Jenkins regularly.
- Use multiple Agents.
- Review Console Output first.
- Keep plugins updated.
- Backup Jenkins configuration.
- Monitor CPU, Memory, and Disk usage.
- Implement centralized logging.

---

# Memory Trick

```text
Build Failed
      |
Console Output
```

```text
Agent Offline
      |
Network + SSH
```

```text
Pipeline Stuck
      |
Current Stage
```

---

# Summary

Most Common Jenkins Issues:

```text
Build Failure

Agent Offline

Pipeline Stuck

Git Checkout Failure

Docker Failure

Kubernetes Deployment Failure

Slow Jenkins
```

Golden Troubleshooting Sources:

```text
Console Output

Jenkins Logs

Agent Logs

System Logs
```

A systematic troubleshooting approach is highly valued in DevOps interviews because real-world CI/CD environments frequently require diagnosing and resolving Jenkins issues quickly.
