# Amazon RDS (Relational Database Service)

## Introduction

Amazon RDS (Relational Database Service) is a fully managed database service provided by AWS.

It simplifies:

- Database Setup
- Backup Management
- Patching
- Monitoring
- Scaling
- High Availability

Instead of managing database servers manually, AWS manages the infrastructure.

---

# What is RDS?

RDS is a managed service for relational databases.

It allows users to run databases without managing:

```text
Operating System

Database Installation

Patching

Hardware
```

---

# Why RDS?

Without RDS:

```text
Launch Server
      |
Install Database
      |
Configure Backup
      |
Apply Patches
      |
Monitor Health
```

Lots of manual work.

---

With RDS:

```text
Create Database
      |
AWS Manages Everything
```

---

# RDS Architecture

```text
Application
      |
Amazon RDS
      |
Database
```

---

# Supported Database Engines

AWS supports:

```text
MySQL

PostgreSQL

MariaDB

Oracle

Microsoft SQL Server

Amazon Aurora
```

---

# Database Engine Overview

```text
RDS
 |
+----------------------+
|                      |
MySQL
PostgreSQL
MariaDB
Oracle
SQL Server
Aurora
```

---

# MySQL

Most popular open-source database.

---

## Use Cases

```text
Web Applications

CMS

E-Commerce
```

---

# PostgreSQL

Advanced open-source database.

---

## Features

```text
ACID Compliance

JSON Support

Advanced Queries
```

---

## Use Cases

```text
Enterprise Applications

Analytics

Financial Systems
```

---

# MariaDB

MySQL-compatible database.

---

## Benefits

```text
Open Source

High Performance
```

---

# Oracle

Enterprise database solution.

---

## Common Use Cases

```text
Banking

ERP Systems

Large Enterprises
```

---

# SQL Server

Microsoft relational database.

---

## Common Use Cases

```text
.NET Applications

Enterprise Applications
```

---

# Amazon Aurora

AWS-developed relational database.

Compatible with:

```text
MySQL

PostgreSQL
```

---

## Benefits

```text
Higher Performance

Automatic Scaling

Improved Availability
```

---

# RDS Components

Main Components:

```text
DB Instance

Storage

Backup

Security Group

Monitoring
```

---

# DB Instance

The actual database server.

---

Example:

```text
MySQL

db.t3.micro
```

---

# Architecture

```text
Application
      |
DB Instance
```

---

# Storage

Stores database data.

---

Options:

```text
General Purpose SSD

Provisioned IOPS SSD

Magnetic
```

---

# Architecture

```text
RDS
 |
Storage
 |
Data Files
```

---

# Security Groups

Control database access.

---

Example:

Allow:

```text
Port 3306 (MySQL)

Port 5432 (PostgreSQL)
```

---

# Architecture

```text
Application
      |
Security Group
      |
RDS
```

---

# Automated Backups

RDS automatically creates backups.

---

# Benefits

```text
Point-in-Time Recovery

Disaster Recovery
```

---

# Workflow

```text
Database
     |
Automatic Backup
     |
Restore If Needed
```

---

# Backup Retention

Configurable:

```text
1 Day

7 Days

35 Days
```

---

# Manual Snapshots

User-created backups.

---

Benefits:

```text
Permanent

Not Deleted Automatically
```

---

# Architecture

```text
RDS
 |
Snapshot
 |
Backup
```

---

# Multi-AZ Deployment

One of the most important interview topics.

---

# What is Multi-AZ?

Creates a standby database in another Availability Zone.

---

# Architecture

```text
Primary DB
      |
Synchronous Replication
      |
Standby DB
```

---

# Diagram

```text
AZ1
 |
Primary DB
 |
Replication
 |
AZ2
 |
Standby DB
```

---

# Benefits

```text
High Availability

Automatic Failover
```

---

# Failover Workflow

```text
Primary Failure
       |
Standby Promoted
       |
Application Continues
```

---

# Multi-AZ Example

```text
Application
      |
Primary Database
      |
Standby Database
```

---

# Read Replicas

Another extremely important interview topic.

---

# What are Read Replicas?

Read-only copies of a database.

---

# Purpose

Reduce read workload.

---

# Architecture

```text
Primary DB
     |
Asynchronous Replication
     |
Read Replica
```

---

# Diagram

```text
             Write
               |
Application
               |
         Primary DB
               |
      +--------+--------+
      |                 |
Read Replica      Read Replica
```

---

# Benefits

```text
Read Scaling

Reporting

Analytics
```

---

# Multi-AZ vs Read Replica

| Feature | Multi-AZ | Read Replica |
|----------|----------|--------------|
| Purpose | High Availability | Read Scaling |
| Replication | Synchronous | Asynchronous |
| Read Traffic | No | Yes |
| Automatic Failover | Yes | No |

---

# Vertical Scaling

Increase:

```text
CPU

RAM

Storage
```

---

Example:

```text
db.t3.micro
      |
db.t3.large
```

---

# Monitoring

RDS integrates with CloudWatch.

---

Monitor:

```text
CPU

Connections

Storage

Latency
```

---

# Architecture

```text
RDS
 |
CloudWatch
 |
Metrics
```

---

# Encryption

Protects database data.

---

Options:

```text
Encryption At Rest

Encryption In Transit
```

---

# Encryption At Rest

Uses:

```text
AWS KMS
```

---

# Encryption In Transit

Uses:

```text
SSL/TLS
```

---

# Maintenance Window

AWS applies:

```text
Patches

Updates

Fixes
```

during maintenance windows.

---

# High Availability Architecture

```text
Application
      |
RDS Primary
      |
Standby DB
```

---

# Production Architecture

```text
Application
      |
RDS Primary
      |
Read Replicas
```

---

# Complete Architecture

```text
Application
      |
RDS Primary
      |
+-----+-----+
|           |
Standby   Read Replica
```

---

# Real World Example

E-Commerce Application

Requirements:

```text
Orders

Payments

Products
```

Architecture:

```text
Application
      |
RDS Multi-AZ
      |
Read Replica
```

Benefits:

```text
High Availability

Read Scaling
```

---

# Interview Question

## What is Amazon RDS?

### Answer

Amazon RDS (Relational Database Service) is a fully managed database service that simplifies database administration tasks such as provisioning, backups, patching, monitoring, and scaling.

---

# Interview Question

## Which Databases Does RDS Support?

### Answer

Amazon RDS supports:

```text
MySQL

PostgreSQL

MariaDB

Oracle

Microsoft SQL Server

Amazon Aurora
```

---

# Interview Question

## What is Multi-AZ?

### Answer

Multi-AZ is a high availability feature that creates a synchronous standby database in another Availability Zone and automatically performs failover if the primary database becomes unavailable.

---

# Interview Question

## What is a Read Replica?

### Answer

A Read Replica is a read-only copy of a database used to offload read traffic and improve application performance.

---

# Interview Question

## Difference Between Multi-AZ and Read Replica

### Answer

Multi-AZ provides high availability using synchronous replication and automatic failover, while Read Replicas provide read scaling using asynchronous replication.

---

# Interview Question

## What is the Difference Between Automated Backups and Snapshots?

### Answer

Automated backups are managed by AWS and retained for a configurable period, while snapshots are user-created backups that remain until manually deleted.

---

# Interview Question

## Why Use Amazon Aurora?

### Answer

Amazon Aurora provides higher performance, better scalability, and improved availability while maintaining compatibility with MySQL and PostgreSQL.

---

# Best Practices

- Enable Multi-AZ for production databases.
- Use Read Replicas for heavy read workloads.
- Enable automated backups.
- Encrypt databases using KMS.
- Restrict access using Security Groups.
- Monitor using CloudWatch.
- Regularly test backup restoration.

---

# Memory Trick

```text
Multi-AZ
     |
High Availability
```

```text
Read Replica
     |
Read Scaling
```

```text
Snapshot
     |
Backup
```

---

# Summary

Amazon RDS provides:

```text
Managed Databases

Automated Backups

High Availability

Read Scaling

Monitoring
```

Architecture:

```text
Application
      |
RDS Primary
      |
Standby DB
      |
Read Replica
```

Amazon RDS is one of the most important AWS database services and is frequently discussed in AWS, Cloud, DevOps, and Solution Architect interviews because it combines database management, high availability, scalability, and security in a managed service.
