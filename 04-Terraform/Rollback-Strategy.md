# Terraform Rollback Strategy

## Introduction

One of the most common Terraform interview questions is:

> How do you roll back infrastructure changes in Terraform?

Unlike application deployment tools, Terraform does not provide a direct rollback command.

There is NO command like:

```bash
terraform rollback
```

Therefore, rollback must be handled using Terraform configurations, version control, and state management.

---

# Does Terraform Have Rollback?

No.

Terraform is a declarative Infrastructure as Code tool.

Terraform only knows:

```text
Current State
      |
Desired State
```

It does not maintain a rollback history.

---

# Example

Current Infrastructure:

```text
EC2 = t2.micro
```

Terraform Change:

```text
EC2 = t3.large
```

Apply:

```bash
terraform apply
```

Infrastructure changes.

---

If deployment fails:

Terraform cannot automatically do:

```text
t3.large
     ↓
t2.micro
```

unless previous configuration exists.

---

# Why Rollback is Needed

Common situations:

- Failed deployment
- Incorrect infrastructure change
- Security misconfiguration
- Application outage
- Performance degradation

---

# Rollback Strategy Overview

```text
Infrastructure Change
          |
          v
Issue Detected
          |
          v
Restore Previous Configuration
          |
          v
terraform apply
          |
          v
Infrastructure Restored
```

---

# Method 1: Git-Based Rollback (Most Common)

Terraform code should always be stored in Git.

Example:

```text
Commit 1
EC2 = t2.micro
```

---

Developer changes:

```text
Commit 2
EC2 = t3.large
```

---

Issue occurs.

Rollback:

```bash
git revert <commit-id>
```

or

```bash
git checkout previous-version
```

---

Apply:

```bash
terraform apply
```

Infrastructure restored.

---

# Workflow

```text
Git Repository
       |
Previous Version
       |
terraform apply
       |
Rollback Complete
```

---

# Example

Current:

```hcl
instance_type = "t3.large"
```

Rollback:

```hcl
instance_type = "t2.micro"
```

Apply:

```bash
terraform apply
```

Terraform updates infrastructure.

---

# Method 2: Restore Previous Terraform Configuration

If previous configuration exists:

```text
Old Terraform Files
```

Restore them.

Run:

```bash
terraform plan
```

Review changes.

Apply:

```bash
terraform apply
```

---

# Method 3: Use Terraform Plan Before Apply

Best rollback strategy:

Prevent bad changes.

Always run:

```bash
terraform plan
```

before:

```bash
terraform apply
```

---

Workflow:

```text
Code Change
      |
terraform plan
      |
Review
      |
terraform apply
```

Reduces rollback requirements.

---

# Method 4: Infrastructure Versioning

Store infrastructure code versions.

Example:

```text
v1.0
v1.1
v1.2
```

Rollback:

```text
v1.2
  ↓
v1.1
```

Apply previous version.

---

# Method 5: Terraform State Backup

Important production practice.

Before changes:

```text
terraform.tfstate.backup
```

created automatically.

---

Example:

```text
terraform.tfstate

terraform.tfstate.backup
```

---

Benefits:

- State recovery
- Disaster recovery

---

# Method 6: Remote State Versioning

For AWS:

```text
S3 Backend
```

Enable:

```text
Versioning
```

---

Architecture:

```text
Terraform
      |
S3 Bucket
      |
Version History
```

---

Example

State Versions:

```text
Version 1
Version 2
Version 3
```

---

Problem occurs:

Restore:

```text
Version 2
```

---

# AWS S3 Versioning

Enable:

```text
Bucket Versioning
```

Benefits:

- State recovery
- Accidental deletion protection

---

# Method 7: Blue-Green Infrastructure

Production environments often use:

```text
Blue Environment
```

Current.

```text
Green Environment
```

New infrastructure.

---

Architecture:

```text
Users
   |
Load Balancer
  / \
 /   \
Blue Green
```

---

If Green fails:

```text
Traffic
   |
Blue
```

Instant rollback.

---

# Example Scenario

Current:

```text
Blue Environment
```

Working.

Deploy:

```text
Green Environment
```

Issue detected.

Rollback:

```text
Switch Traffic
      |
Blue
```

No downtime.

---

# Terraform Rollback Workflow

```text
Terraform Change
        |
terraform apply
        |
Problem Detected
        |
Restore Previous Code
        |
terraform apply
        |
Rollback Complete
```

---

# Real Production Example

Developer changes:

```text
Security Group
```

New rule:

```text
Port 22 Open To Internet
```

Security issue detected.

Rollback:

```text
Git Revert
      |
terraform apply
```

Previous configuration restored.

---

# Disaster Recovery Example

State file accidentally deleted.

Recovery:

```text
S3 Versioning
      |
Restore Previous Version
```

Infrastructure management restored.

---

# Common Mistakes

## No Version Control

Without Git:

```text
No Previous Configuration
```

Rollback difficult.

---

## Applying Without Plan

Risk:

```text
Unexpected Changes
```

---

## No State Backup

Recovery becomes harder.

---

## Manual Infrastructure Changes

Can cause:

```text
Configuration Drift
```

Complicates rollback.

---

# Interview Question

## Does Terraform Have a Rollback Command?

### Answer

No. Terraform does not provide a dedicated rollback command. Rollbacks are typically performed by restoring a previous version of the Terraform configuration and applying it again.

---

# Interview Question

## How Do You Roll Back Infrastructure Changes?

### Answer

I usually store Terraform code in Git. If a change causes issues, I revert to a previous stable version of the code using Git and then run terraform apply. Terraform updates the infrastructure to match the previous configuration.

---

# Interview Question

## What is the Best Rollback Strategy?

### Answer

The best strategy is to use Git version control, review terraform plan outputs before deployment, maintain state backups, and enable S3 versioning for remote state storage.

---

# Interview Question

## Why is S3 Versioning Important?

### Answer

S3 Versioning protects Terraform state files by maintaining historical versions. If a state file is corrupted or deleted, a previous version can be restored.

---

# Interview Question

## How Can Blue-Green Deployments Help Rollback?

### Answer

Blue-Green deployments maintain two environments. If the new environment fails, traffic can immediately be redirected to the previous stable environment, providing a fast rollback mechanism.

---

# Best Practices

- Store Terraform code in Git.
- Review terraform plan before apply.
- Enable S3 Versioning.
- Use remote state storage.
- Backup state files.
- Avoid manual infrastructure changes.
- Test rollback procedures regularly.

---

# Production Architecture

```text
Git Repository
      |
Terraform
      |
S3 Backend
      |
Versioned State
      |
AWS Infrastructure
```

---

# Summary

Terraform does not provide:

```bash
terraform rollback
```

Common rollback methods:

```text
Git Revert
Previous Configuration
State Backup
S3 Versioning
Blue-Green Infrastructure
```

Recommended Production Strategy:

```text
Git
 +
terraform plan
 +
S3 Versioning
 +
Remote State
```

These practices ensure infrastructure can be restored quickly and safely when changes cause problems.
