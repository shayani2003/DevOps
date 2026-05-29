# Amazon CloudWatch

## Introduction

Amazon CloudWatch is AWS's monitoring and observability service.

It helps monitor:

- AWS Resources
- Applications
- Infrastructure
- Logs
- Metrics
- Events

CloudWatch allows teams to detect issues, monitor performance, and automate responses.

---

# What is CloudWatch?

CloudWatch collects and tracks operational data from AWS resources.

Examples:

```text
EC2 CPU Usage

Memory Usage

Application Logs

Network Traffic

Disk Usage
```

---

# Why CloudWatch?

Without Monitoring:

```text
Application Slow
      |
No Visibility
```

Problem identification becomes difficult.

---

With CloudWatch:

```text
Application Slow
      |
CloudWatch Alert
      |
Investigate Issue
```

---

# CloudWatch Architecture

```text
AWS Resources
      |
CloudWatch
      |
Metrics
Logs
Events
Alarms
```

---

# Core Components

CloudWatch consists of:

```text
Metrics

Logs

Alarms

Dashboards

Events
```

---

# CloudWatch Metrics

## What are Metrics?

Metrics are numerical measurements collected over time.

---

# Examples

```text
CPU Utilization

Network In

Network Out

Disk Read

Disk Write
```

---

# Architecture

```text
EC2
 |
Metrics
 |
CloudWatch
```

---

# Example Metric

```text
CPU Utilization = 75%
```

---

# Metric Visualization

```text
Time
 |
CPU Usage
 |
Graph
```

---

# Default EC2 Metrics

AWS automatically collects:

```text
CPU Utilization

Network Traffic

Disk Operations

Status Checks
```

---

# Custom Metrics

Users can send custom metrics.

Example:

```text
Application Response Time

Active Users

Orders Per Minute
```

---

# Architecture

```text
Application
      |
Custom Metric
      |
CloudWatch
```

---

# CloudWatch Logs

## What are CloudWatch Logs?

CloudWatch Logs store application and system logs.

---

# Examples

```text
Application Logs

Web Server Logs

System Logs

Lambda Logs
```

---

# Architecture

```text
Application
      |
Logs
      |
CloudWatch Logs
```

---

# Example

Application:

```text
Login Failed
```

Stored in:

```text
CloudWatch Logs
```

---

# Log Groups

A collection of log streams.

---

Example:

```text
Application Logs
```

contains:

```text
Server1 Logs

Server2 Logs

Server3 Logs
```

---

# Architecture

```text
Log Group
     |
+----+----+
|         |
Log1    Log2
```

---

# Log Streams

Individual source of logs.

Example:

```text
EC2-Server-1

EC2-Server-2
```

---

# CloudWatch Alarms

## What are Alarms?

Alarms monitor metrics and trigger actions when thresholds are reached.

---

# Example

CPU:

```text
> 80%
```

Trigger:

```text
Alarm
```

---

# Architecture

```text
Metric
   |
Alarm
   |
Notification
```

---

# Alarm States

CloudWatch Alarms have:

```text
OK

ALARM

INSUFFICIENT_DATA
```

---

# Example

CPU:

```text
50%
```

State:

```text
OK
```

---

CPU:

```text
90%
```

State:

```text
ALARM
```

---

# Alarm Workflow

```text
CPU > 80%
      |
Alarm
      |
SNS Notification
```

---

# SNS Integration

CloudWatch can send notifications through SNS.

---

# Architecture

```text
CloudWatch Alarm
        |
SNS
        |
Email
```

---

# Example

CPU exceeds:

```text
80%
```

Email sent automatically.

---

# CloudWatch Dashboards

## What are Dashboards?

Dashboards provide a visual view of metrics.

---

# Example Dashboard

```text
CPU

Memory

Network

Errors
```

on one screen.

---

# Architecture

```text
CloudWatch
     |
Dashboard
     |
Graphs
```

---

# Benefits

- Centralized Monitoring
- Real-Time Visibility

---

# CloudWatch Events

CloudWatch Events is now part of:

```text
Amazon EventBridge
```

---

# Purpose

Respond automatically to AWS events.

---

# Example

```text
EC2 Stopped
```

Trigger:

```text
Lambda Function
```

---

# Architecture

```text
AWS Event
      |
EventBridge
      |
Target
```

---

# Event Sources

Examples:

```text
EC2

S3

Lambda

RDS
```

---

# Event Targets

Examples:

```text
Lambda

SNS

SQS

Step Functions
```

---

# Event Workflow

```text
EC2 State Change
       |
EventBridge
       |
Lambda
```

---

# CloudWatch Agent

## What is CloudWatch Agent?

A software agent installed on EC2.

---

# Purpose

Collect:

```text
Memory Usage

Disk Usage

Application Metrics
```

---

# Why Needed?

Default EC2 metrics do NOT include:

```text
Memory Usage
```

CloudWatch Agent solves this.

---

# Architecture

```text
EC2
 |
CloudWatch Agent
 |
CloudWatch
```

---

# CloudWatch Logs Insights

Used for log analysis.

---

# Example Query

```sql
fields @timestamp, @message
| sort @timestamp desc
```

---

# Benefits

- Fast Searching
- Troubleshooting
- Log Analytics

---

# CloudWatch with Auto Scaling

Most common interview topic.

---

# Architecture

```text
EC2
 |
CPU Metrics
 |
CloudWatch
 |
Alarm
 |
ASG
 |
Add Instance
```

---

# Example

CPU:

```text
85%
```

Action:

```text
Scale Out
```

---

# CloudWatch with Lambda

```text
Lambda
   |
Logs
   |
CloudWatch
```

---

# CloudWatch with ELB

Monitor:

```text
Request Count

Latency

Healthy Hosts
```

---

# CloudWatch with RDS

Monitor:

```text
CPU

Connections

Storage

Read Operations
```

---

# Complete Monitoring Architecture

```text
EC2
RDS
Lambda
S3
 |
CloudWatch
 |
Metrics
Logs
Alarms
Dashboards
```

---

# Real Production Example

E-Commerce Website

Monitor:

```text
CPU

Memory

Application Errors

Response Time
```

Workflow:

```text
Issue Detected
      |
CloudWatch Alarm
      |
SNS Email
      |
Engineer Responds
```

---

# Advantages

## Centralized Monitoring

Monitor all AWS resources.

---

## Real-Time Alerts

Immediate notifications.

---

## Automation

Trigger automated actions.

---

## Visibility

Detailed operational insights.

---

# Interview Question

## What is Amazon CloudWatch?

### Answer

Amazon CloudWatch is AWS's monitoring and observability service that collects metrics, logs, and events from AWS resources and applications, enabling monitoring, alerting, and automation.

---

# Interview Question

## What are CloudWatch Metrics?

### Answer

Metrics are numerical measurements collected over time that represent the performance and health of AWS resources and applications.

---

# Interview Question

## What is a CloudWatch Alarm?

### Answer

A CloudWatch Alarm monitors a metric and automatically performs actions such as sending notifications or triggering Auto Scaling when predefined thresholds are reached.

---

# Interview Question

## What is the Difference Between CloudWatch Logs and Metrics?

### Answer

Metrics are numerical performance measurements such as CPU utilization, while Logs contain detailed event and application information used for troubleshooting and analysis.

---

# Interview Question

## Why Do We Need CloudWatch Agent?

### Answer

The CloudWatch Agent collects additional system-level metrics such as memory usage, disk usage, and custom application metrics that are not available by default.

---

# Interview Question

## How Does CloudWatch Work with Auto Scaling?

### Answer

CloudWatch monitors metrics such as CPU utilization. When thresholds are exceeded, CloudWatch Alarms trigger Auto Scaling policies to add or remove EC2 instances automatically.

---

# Best Practices

- Create meaningful alarms.
- Use CloudWatch Dashboards.
- Enable Log Retention Policies.
- Monitor critical business metrics.
- Integrate alarms with SNS.
- Install CloudWatch Agent on EC2.
- Monitor across multiple Availability Zones.

---

# Memory Trick

```text
CloudWatch
     |
Metrics
Logs
Alarms
Dashboards
Events
```

Workflow:

```text
Metric
   |
Alarm
   |
SNS
   |
Notification
```

---

# Summary

CloudWatch Components:

```text
Metrics

Logs

Alarms

Dashboards

EventBridge
```

Architecture:

```text
AWS Resources
      |
CloudWatch
      |
Monitoring
      |
Alerts
```

Amazon CloudWatch is one of the most important AWS operational services because it provides monitoring, alerting, observability, and automation capabilities, making it a very common topic in AWS and DevOps interviews.
