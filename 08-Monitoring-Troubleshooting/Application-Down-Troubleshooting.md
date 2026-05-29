# Application Down – Troubleshooting Guide

## Introduction

One of the most common DevOps and SRE interview questions is:

```text
Application is down.
What will you do?
```

Interviewers are not looking for a single command.

They want to see a structured troubleshooting approach.

---

# Golden Rule

Never assume the root cause.

Follow a systematic process:

```text
Identify
   |
Investigate
   |
Isolate
   |
Fix
   |
Verify
```

---

# Troubleshooting Flow

```text
Alert/User Complaint
        |
Check Application
        |
Check Infrastructure
        |
Check Logs
        |
Find Root Cause
        |
Fix Issue
        |
Verify Recovery
```

---

# Step 1: Confirm the Problem

First verify:

```text
Is application really down?
```

---

# Questions

```text
Can users access it?

Is it completely down?

Is it slow?

Which users are affected?
```

---

# Example

Check:

```text
Website URL
```

or

```bash
curl http://application-url
```

---

# Result

```text
200 OK
```

Application working.

---

```text
500 Error
```

Application issue.

---

```text
Connection Timeout
```

Infrastructure issue.

---

# Step 2: Check Monitoring System

Review alerts.

---

# Common Tools

```text
CloudWatch

Prometheus

Grafana

Datadog

New Relic
```

---

# Check

```text
CPU

Memory

Disk

Network
```

---

# Architecture

```text
Application
      |
Monitoring
      |
Alert
```

---

# Step 3: Check Load Balancer

Very common issue.

---

# Architecture

```text
Users
  |
ALB
  |
Application
```

---

# Verify

```text
Target Health

Listener Rules

Security Groups
```

---

# Example

Unhealthy Targets:

```text
ALB
 |
No Healthy Instances
```

Application unavailable.

---

# Step 4: Check Server Health

If application runs on EC2:

---

# Verify

```bash
top

free -h

df -h
```

---

# Check CPU

```bash
top
```

---

Example:

```text
CPU = 99%
```

Problem identified.

---

# Check Memory

```bash
free -h
```

---

Example:

```text
Memory = 100%
```

Possible OOM issue.

---

# Check Disk

```bash
df -h
```

---

Example:

```text
Disk Usage = 100%
```

Application cannot write logs.

---

# Step 5: Check Service Status

Example:

```bash
systemctl status nginx
```

---

Possible Output

```text
inactive
```

Service stopped.

---

Fix:

```bash
systemctl restart nginx
```

---

# Architecture

```text
Server
  |
Application Service
```

---

# Step 6: Check Logs

Most important troubleshooting step.

---

# Linux Logs

```bash
journalctl -xe
```

---

# Application Logs

```bash
tail -f application.log
```

---

# Kubernetes Logs

```bash
kubectl logs pod-name
```

---

# Docker Logs

```bash
docker logs container-id
```

---

# Architecture

```text
Application
      |
Logs
      |
Root Cause
```

---

# Common Log Errors

```text
Database Connection Failed

Out Of Memory

Port Already In Use

Authentication Failure
```

---

# Step 7: Check Database

Applications often fail because databases fail.

---

# Architecture

```text
Application
      |
Database
```

---

# Verify

```text
Database Running?

Connections Available?

Storage Full?
```

---

# Example

```bash
mysql -u root -p
```

Connection fails.

---

Possible Cause:

```text
Database Down
```

---

# Step 8: Check Network

Networking issues are common.

---

# Verify

```text
Security Groups

NACL

Firewall

DNS
```

---

# Connectivity Test

```bash
ping hostname
```

---

```bash
telnet host port
```

---

Example

```bash
telnet database 3306
```

---

Failure:

```text
Connection Refused
```

Networking issue.

---

# Step 9: Check DNS

Sometimes application is healthy but DNS fails.

---

# Verify

```bash
nslookup example.com
```

---

or

```bash
dig example.com
```

---

# Architecture

```text
User
 |
DNS
 |
Application
```

---

# Step 10: Check Recent Deployments

One of the most common causes.

---

# Question

```text
Was anything deployed recently?
```

---

# Architecture

```text
Deployment
     |
Issue
```

---

# Example

```text
Version 2 Released
      |
Application Down
```

---

Fix:

```text
Rollback
```

---

# Kubernetes Troubleshooting

Very common interview topic.

---

# Check Pods

```bash
kubectl get pods
```

---

# Example

```text
CrashLoopBackOff
```

---

Investigate:

```bash
kubectl logs pod-name
```

---

# Check Nodes

```bash
kubectl get nodes
```

---

# Check Events

```bash
kubectl get events
```

---

# Kubernetes Architecture

```text
Node
 |
Pod
 |
Application
```

---

# Docker Troubleshooting

---

# Check Containers

```bash
docker ps -a
```

---

# View Logs

```bash
docker logs container-id
```

---

# Inspect Container

```bash
docker inspect container-id
```

---

# AWS Troubleshooting

---

# Check EC2

```text
Instance Status

CPU

Memory

Storage
```

---

# Check ELB

```text
Healthy Targets

Listener Rules
```

---

# Check RDS

```text
Connections

Storage

Availability
```

---

# Check CloudWatch

```text
Metrics

Alarms

Logs
```

---

# Common Root Causes

## High CPU

```text
Infinite Loop

Heavy Traffic
```

---

## High Memory

```text
Memory Leak

Large Workload
```

---

## Disk Full

```text
Logs Filled Storage
```

---

## Database Failure

```text
Connection Refused
```

---

## Deployment Failure

```text
Bad Release
```

---

## Network Issue

```text
Security Group

Firewall

DNS
```

---

# Real Production Example 1

## Application Returning 500 Errors

Investigation:

```text
Logs
```

Found:

```text
Database Password Incorrect
```

---

Fix:

```text
Update Secret
```

---

Application restored.

---

# Real Production Example 2

## Kubernetes CrashLoopBackOff

Investigation:

```bash
kubectl logs pod-name
```

Found:

```text
Missing Environment Variable
```

---

Fix:

```text
Update ConfigMap
```

---

Application restored.

---

# Real Production Example 3

## Website Slow

CloudWatch:

```text
CPU = 98%
```

---

Fix:

```text
Auto Scaling
```

and optimize application.

---

# Interview Question

## Application Down. What Is Your Troubleshooting Approach?

### Answer

I first confirm the issue and determine its scope. Then I check monitoring dashboards, server health, application logs, database connectivity, networking, and recent deployments. I isolate the root cause, apply mitigation such as restarting services or rolling back deployments, verify recovery, and finally perform RCA to prevent recurrence.

---

# Interview Question

## What Would You Check First?

### Answer

I would first verify whether the application is actually down, check monitoring alerts, and determine whether the issue is related to infrastructure, networking, application code, or dependencies such as databases.

---

# Interview Question

## How Do You Troubleshoot a Kubernetes Application?

### Answer

I check Pods, Nodes, Events, Logs, resource usage, ConfigMaps, Secrets, Services, and Ingress configurations to identify the root cause of the issue.

---

# Best Practices

- Always start with monitoring.
- Check logs before making assumptions.
- Verify recent deployments.
- Use a structured troubleshooting process.
- Document findings.
- Perform RCA after resolution.
- Automate monitoring and alerting.

---

# Memory Trick

```text
Application Down
       |
Monitoring
       |
Logs
       |
Infrastructure
       |
Database
       |
Network
       |
Deployment
       |
Fix
```

---

# Summary

Troubleshooting Workflow:

```text
Confirm Issue
      |
Check Monitoring
      |
Check Infrastructure
      |
Check Logs
      |
Check Database
      |
Check Network
      |
Check Deployment
      |
Resolve
```

Most Common Causes:

```text
Application Bugs

High CPU

Memory Leak

Database Failure

Network Issue

Bad Deployment
```

A structured troubleshooting methodology is one of the most important DevOps and SRE skills because it enables engineers to quickly identify root causes and restore services with minimal downtime.
