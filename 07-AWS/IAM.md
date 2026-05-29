# AWS IAM (Identity and Access Management)

## Introduction

AWS IAM (Identity and Access Management) is a service used to securely control access to AWS resources.

IAM helps answer:

```text
Who can access AWS?

What can they access?

What actions can they perform?
```

IAM is one of the most important AWS security services.

---

# What is IAM?

IAM allows you to manage:

```text
Users

Groups

Roles

Policies
```

and control permissions for AWS resources.

---

# Why IAM?

Without IAM:

```text
Everyone
     |
Full Access
```

Huge security risk.

---

With IAM:

```text
Developer
     |
EC2 Access

Database Team
     |
RDS Access

Admin
     |
Full Access
```

---

# IAM Architecture

```text
IAM
 |
+------+------+------+
|      |      |      |
Users Groups Roles Policies
```

---

# IAM Components

Main Components:

```text
Users

Groups

Roles

Policies
```

---

# IAM User

## What is an IAM User?

An IAM User represents a person or application that needs access to AWS.

---

# Examples

```text
John

Alice

CI/CD Pipeline
```

---

# Architecture

```text
User
  |
Login
  |
AWS Resources
```

---

# Example

Developer:

```text
Shayani
```

gets:

```text
IAM User
```

---

# IAM Group

## What is an IAM Group?

A collection of IAM Users.

Used to manage permissions efficiently.

---

# Example

```text
Developers Group

Admins Group

QA Group
```

---

# Architecture

```text
Developers Group
      |
+-----+-----+
|           |
User1     User2
```

---

# Benefits

Instead of assigning permissions individually:

```text
Assign Once
      |
Group
```

---

# IAM Role

## What is an IAM Role?

A Role provides temporary permissions.

Unlike users:

```text
No Username

No Password
```

---

# Architecture

```text
AWS Service
      |
IAM Role
      |
AWS Resource
```

---

# Example

EC2 needs S3 access.

Bad Practice:

```text
Store Access Keys
Inside EC2
```

---

Good Practice:

```text
EC2
 |
IAM Role
 |
S3 Access
```

---

# Role Workflow

```text
EC2
 |
Assume Role
 |
Temporary Credentials
 |
Access S3
```

---

# Real World Example

```text
EC2
 |
Download File
 |
S3
```

using IAM Role.

---

# IAM Policy

## What is a Policy?

A Policy is a JSON document that defines permissions.

---

# Architecture

```text
User
 |
Policy
 |
Permissions
```

---

# Example Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "*"
    }
  ]
}
```

---

# Policy Components

## Effect

```text
Allow

Deny
```

---

## Action

What operation is allowed.

Example:

```text
s3:GetObject

ec2:StartInstances
```

---

## Resource

Which resource is affected.

Example:

```text
S3 Bucket

EC2 Instance
```

---

# Managed Policies

Created and maintained by AWS.

Example:

```text
AmazonS3ReadOnlyAccess

AdministratorAccess
```

---

# Customer Managed Policies

Created by organization.

Example:

```text
Custom EC2 Access
```

---

# Inline Policies

Attached directly to one user, group, or role.

---

# IAM Policy Architecture

```text
User
 |
Policy
 |
Action
 |
Resource
```

---

# Authentication vs Authorization

## Authentication

Who are you?

Example:

```text
Username

Password

MFA
```

---

## Authorization

What can you do?

Example:

```text
Read S3

Create EC2
```

---

# Multi-Factor Authentication (MFA)

## What is MFA?

Additional security layer.

---

# Workflow

```text
Username
Password
    +
OTP
```

---

# Example

Login:

```text
Password
```

and:

```text
Google Authenticator Code
```

---

# Benefits

- Improved Security
- Reduced Account Compromise Risk

---

# Least Privilege Principle

One of the most important IAM concepts.

---

# Meaning

Give only required permissions.

---

Bad:

```text
Developer
     |
AdministratorAccess
```

---

Good:

```text
Developer
     |
Read S3 Only
```

---

# Example

Need:

```text
Read Bucket
```

Allow:

```text
s3:GetObject
```

Do NOT allow:

```text
s3:DeleteBucket
```

---

# Access Keys

Used for programmatic access.

---

# Components

```text
Access Key ID

Secret Access Key
```

---

# Example

Used by:

```text
AWS CLI

SDK

Applications
```

---

# Best Practice

Avoid long-term access keys.

Use:

```text
IAM Roles
```

instead.

---

# IAM Role for EC2

Most common interview topic.

---

# Architecture

```text
EC2
 |
IAM Role
 |
S3
```

---

# Benefits

- No hardcoded credentials
- Automatic rotation
- More secure

---

# IAM Role for Lambda

```text
Lambda
   |
IAM Role
   |
DynamoDB
```

---

# IAM Role for EKS

```text
Pod
 |
IAM Role
 |
AWS Services
```

---

# IAM Permission Evaluation

AWS evaluates:

```text
User

Group

Role

Policy
```

before allowing access.

---

# Evaluation Flow

```text
Request
   |
Policy Check
   |
Allow?
 /     \
Yes     No
 |       |
Access  Denied
```

---

# Real Production Architecture

```text
Developers
      |
IAM Users
      |
Groups
      |
Policies
      |
AWS Resources
```

---

# Secure EC2 Architecture

```text
EC2
 |
IAM Role
 |
S3
```

No access keys stored.

---

# Interview Question

## What is IAM?

### Answer

AWS IAM (Identity and Access Management) is a service used to securely manage access to AWS resources through users, groups, roles, and policies.

---

# Interview Question

## Difference Between IAM User and IAM Role

### Answer

An IAM User represents a person or application with permanent credentials, while an IAM Role provides temporary credentials and is assumed when needed.

---

# Interview Question

## What is an IAM Policy?

### Answer

An IAM Policy is a JSON document that defines permissions by specifying allowed or denied actions on AWS resources.

---

# Interview Question

## How Does an EC2 Instance Access S3 Securely?

### Answer

The recommended approach is to attach an IAM Role to the EC2 instance. The instance assumes the role and receives temporary credentials to access S3 securely without storing access keys.

---

# Interview Question

## What is the Principle of Least Privilege?

### Answer

The Principle of Least Privilege means granting only the minimum permissions required to perform a task, reducing security risks and preventing unauthorized access.

---

# Interview Question

## Why is MFA Important?

### Answer

MFA adds an additional authentication factor beyond a password, significantly improving account security and reducing the risk of unauthorized access.

---

# Best Practices

- Enable MFA for all users.
- Use IAM Roles instead of access keys.
- Follow Least Privilege Principle.
- Rotate credentials regularly.
- Avoid using the root account.
- Use Groups for permission management.
- Audit permissions periodically.

---

# Memory Trick

```text
User
  |
Permanent Identity
```

```text
Role
  |
Temporary Access
```

```text
Policy
  |
Permissions
```

---

# Summary

IAM Components:

```text
Users

Groups

Roles

Policies
```

Security Features:

```text
MFA

Least Privilege

Temporary Credentials
```

Most Important Interview Concept:

```text
EC2
 |
IAM Role
 |
S3
```

AWS IAM is one of the most critical AWS services because it controls authentication, authorization, and security across the entire AWS environment, making it a guaranteed interview topic for cloud and DevOps roles.
