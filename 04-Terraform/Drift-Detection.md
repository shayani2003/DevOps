# Terraform Drift Detection

## What is Terraform Drift?

Terraform Drift (also called Configuration Drift) occurs when the actual infrastructure differs from what is defined in Terraform code or stored in the Terraform state file.

In simple words:

```text
Terraform Code
      ≠
Actual Infrastructure
```

This difference is called:

```text
Configuration Drift
```

---

# Why Does Drift Occur?

Drift usually happens when someone manually changes infrastructure outside Terraform.

Examples:

- Creating resources from AWS Console
- Modifying Security Groups manually
- Deleting EC2 instances manually
- Updating S3 bucket settings manually

---

# Example Scenario

Terraform Code:

```hcl
resource "aws_instance" "web" {
  instance_type = "t2.micro"
}
```

Expected:

```text
EC2 = t2.micro
```

---

Someone manually changes it in AWS Console:

```text
EC2 = t3.large
```

Now:

```text
Terraform Code = t2.micro

Actual Infrastructure = t3.large
```

Drift has occurred.

---

# Architecture Diagram

```text
Terraform Code
        |
        |
        v
Terraform State
        |
        |
        v
AWS Infrastructure

      (Manual Change)

Terraform Code
       ≠
Infrastructure
```

---

# Common Causes of Drift

## Manual Infrastructure Changes

Most common reason.

Example:

```text
AWS Console
      |
Manual Update
      |
Drift
```

---

## Emergency Production Fixes

Engineer changes infrastructure quickly.

Example:

```text
Production Issue
      |
Manual Fix
      |
Terraform Not Updated
```

---

## Resources Deleted Manually

Example:

```text
EC2 Deleted
```

Terraform still believes:

```text
EC2 Exists
```

---

## Infrastructure Modified by Other Tools

Examples:

```text
CloudFormation
AWS Console
Ansible
Scripts
```

---

# How to Detect Drift?

Most common method:

```bash
terraform plan
```

---

# Example

Terraform expects:

```text
EC2 = t2.micro
```

Actual:

```text
EC2 = t3.large
```

Run:

```bash
terraform plan
```

Output:

```text
~ instance_type

t3.large -> t2.micro
```

Terraform detects drift.

---

# Why terraform plan Works

Terraform compares:

```text
Terraform Code
        |
Terraform State
        |
Actual Infrastructure
```

Differences are reported.

---

# Drift Detection Workflow

```text
terraform plan
       |
Compare Infrastructure
       |
Drift Found?
       |
      Yes
       |
Show Changes
```

---

# Example: Resource Deleted

Terraform State:

```text
EC2 Exists
```

Actual AWS:

```text
EC2 Deleted
```

Run:

```bash
terraform plan
```

Output:

```text
+ create aws_instance.web
```

Terraform wants to recreate it.

---

# Example: Security Group Changed

Terraform Code:

```text
Port 80 Open
```

AWS Console:

```text
Port 22 Added
```

Run:

```bash
terraform plan
```

Output:

```text
Port 22 will be removed
```

Drift detected.

---

# How to Fix Drift?

There are several methods.

---

# Method 1: Apply Terraform Configuration

If Terraform configuration is correct:

```bash
terraform apply
```

Terraform restores infrastructure.

Example:

```text
Infrastructure
      |
Drift
      |
terraform apply
      |
Correct State Restored
```

---

# Method 2: Update Terraform Code

If manual change is intentional:

Update Terraform code.

Example:

Old:

```hcl
instance_type = "t2.micro"
```

New:

```hcl
instance_type = "t3.large"
```

Run:

```bash
terraform apply
```

Now:

```text
Code = Infrastructure
```

---

# Method 3: Import Existing Resource

Suppose resource exists but Terraform doesn't know about it.

Import:

```bash
terraform import aws_instance.web i-123456
```

Terraform starts managing it.

---

# Method 4: Refresh State

Command:

```bash
terraform refresh
```

Updates Terraform state with actual infrastructure.

---

# Real Production Example

Production Environment:

```text
EC2 Instance
```

Terraform:

```text
t2.micro
```

Engineer manually changes:

```text
t3.medium
```

After few days:

```bash
terraform plan
```

Output:

```text
Drift Detected
```

Team decides:

- Keep t3.medium

Update Terraform code.

Problem solved.

---

# Preventing Drift

## Use Terraform Only

Avoid manual changes.

Good:

```text
Terraform
      |
Infrastructure
```

Bad:

```text
Terraform
      |
AWS Console
      |
Infrastructure
```

---

## Restrict Console Access

Reduce permissions.

Example:

```text
Read Only Access
```

for most users.

---

## Use Code Reviews

All infrastructure changes go through:

```text
Git Pull Request
```

---

## Run terraform plan Regularly

Detect drift early.

---

## Enable Monitoring

Track infrastructure changes.

Examples:

```text
CloudTrail
AWS Config
```

---

# Drift Detection Architecture

```text
Terraform Code
       |
       v
Terraform Plan
       |
       v
Compare Actual Infrastructure
       |
+------+------+
|             |
No Drift   Drift Found
```

---

# Interview Question

## What is Terraform Drift?

### Answer

Terraform Drift occurs when the actual infrastructure differs from the Terraform configuration or state file. This usually happens when resources are modified manually outside Terraform.

---

# Interview Question

## How Do You Detect Drift?

### Answer

The most common way is using:

```bash
terraform plan
```

Terraform compares the infrastructure with the configuration and reports any differences.

---

# Interview Question

## How Do You Fix Drift?

### Answer

Drift can be fixed by:

1. Running terraform apply to restore the desired state.
2. Updating Terraform code if the manual change is intended.
3. Importing resources into Terraform.
4. Refreshing the state file.

---

# Interview Question

## How Can You Prevent Drift?

### Answer

I prevent drift by ensuring all infrastructure changes are performed through Terraform, restricting manual console changes, implementing code reviews, and regularly running terraform plan to identify differences.

---

# Best Practices

- Use Terraform as the single source of truth.
- Avoid manual infrastructure changes.
- Review terraform plan output regularly.
- Restrict direct cloud console access.
- Store Terraform code in Git.
- Use monitoring and auditing tools.

---

# Common Production Scenario

Problem:

```text
Application Stops Working
```

Investigation:

```bash
terraform plan
```

Output:

```text
Security Group Drift Detected
```

Someone manually changed firewall rules.

Solution:

```bash
terraform apply
```

Infrastructure restored.

---

# Summary

Terraform Drift means:

```text
Terraform Code
      ≠
Infrastructure
```

Detection:

```bash
terraform plan
```

Fix:

```bash
terraform apply
```

or

```text
Update Terraform Code
```

Drift Detection is a critical Terraform skill because it helps maintain infrastructure consistency and prevents configuration-related production issues.
