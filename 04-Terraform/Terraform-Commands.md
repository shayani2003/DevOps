# Terraform Commands

## Introduction

Terraform commands are used to manage the complete lifecycle of infrastructure.

A typical Terraform workflow is:

```text
Write Configuration
        |
terraform init
        |
terraform validate
        |
terraform plan
        |
terraform apply
        |
Infrastructure Created
```

---

# Most Important Terraform Commands

1. terraform init
2. terraform validate
3. terraform plan
4. terraform apply
5. terraform destroy

These five commands are asked in almost every Terraform interview.

---

# Terraform Workflow

```text
Configuration Files
        |
        v
terraform init
        |
        v
terraform validate
        |
        v
terraform plan
        |
        v
terraform apply
        |
        v
Infrastructure
```

---

# 1. terraform init

## What is terraform init?

Initializes a Terraform project.

Command:

```bash
terraform init
```

---

## What Does It Do?

Downloads:

- Providers
- Modules
- Dependencies

Creates:

```text
.terraform/
```

directory.

---

## Example

```bash
terraform init
```

Output:

```text
Initializing provider plugins...

Terraform has been successfully initialized!
```

---

## When to Run?

Run:

```bash
terraform init
```

Before using Terraform in a new project.

---

## Diagram

```text
Project
   |
terraform init
   |
Download Providers
   |
Ready
```

---

# 2. terraform validate

## What is terraform validate?

Checks configuration syntax.

Command:

```bash
terraform validate
```

---

## Purpose

Detects:

- Syntax errors
- Missing blocks
- Invalid references

---

## Example

Valid:

```hcl
resource "aws_instance" "web" {
  instance_type = "t2.micro"
}
```

Output:

```text
Success! Configuration is valid.
```

---

## Example Error

```hcl
resource aws_instance web {
}
```

Output:

```text
Invalid syntax
```

---

# 3. terraform plan

## What is terraform plan?

Shows changes before applying them.

Command:

```bash
terraform plan
```

---

## Purpose

Preview infrastructure changes.

No resources are created.

---

## Example

Current:

```text
No EC2 Instance
```

Configuration:

```hcl
resource "aws_instance" "web" {}
```

Run:

```bash
terraform plan
```

Output:

```text
+ create aws_instance.web
```

---

## Why Important?

Prevents accidental changes.

---

## Diagram

```text
Current State
      |
terraform plan
      |
Preview Changes
      |
No Resources Created
```

---

# 4. terraform apply

## What is terraform apply?

Creates or modifies infrastructure.

Command:

```bash
terraform apply
```

---

## Example

```bash
terraform apply
```

Output:

```text
aws_instance.web: Creating...

Apply complete!
```

---

## Confirmation

Terraform asks:

```text
Do you want to perform these actions?
```

Type:

```text
yes
```

---

## Auto Approve

```bash
terraform apply -auto-approve
```

Skips confirmation.

---

## Diagram

```text
Configuration
      |
terraform apply
      |
Infrastructure Created
```

---

# 5. terraform destroy

## What is terraform destroy?

Removes infrastructure.

Command:

```bash
terraform destroy
```

---

## Example

Before:

```text
EC2
S3
VPC
```

Run:

```bash
terraform destroy
```

After:

```text
Resources Deleted
```

---

## Diagram

```text
Infrastructure
      |
terraform destroy
      |
Resources Removed
```

---

# Additional Important Commands

# terraform fmt

Formats Terraform code.

Command:

```bash
terraform fmt
```

Before:

```hcl
resource "aws_instance" "web"{
}
```

After:

```hcl
resource "aws_instance" "web" {
}
```

---

# terraform show

Displays state information.

Command:

```bash
terraform show
```

---

# terraform output

Displays outputs.

Command:

```bash
terraform output
```

Example:

```text
public_ip = 54.123.45.67
```

---

# terraform state list

Lists resources in state file.

Command:

```bash
terraform state list
```

Output:

```text
aws_instance.web
aws_s3_bucket.demo
```

---

# terraform refresh

Updates state from actual infrastructure.

Command:

```bash
terraform refresh
```

---

# terraform import

Imports existing resources.

Example:

```bash
terraform import aws_instance.web i-123456
```

---

# Complete Example

Step 1:

Initialize.

```bash
terraform init
```

---

Step 2:

Validate.

```bash
terraform validate
```

---

Step 3:

Preview.

```bash
terraform plan
```

---

Step 4:

Create infrastructure.

```bash
terraform apply
```

---

Step 5:

View outputs.

```bash
terraform output
```

---

Step 6:

Destroy resources.

```bash
terraform destroy
```

---

# Command Flow Diagram

```text
terraform init
        |
terraform validate
        |
terraform plan
        |
terraform apply
        |
Infrastructure
        |
terraform destroy
```

---

# Interview Question

## What Terraform Commands Do You Use Regularly?

### Answer

The commands I use most frequently are:

```bash
terraform init
terraform validate
terraform plan
terraform apply
terraform destroy
terraform fmt
terraform output
```

These commands help initialize, validate, preview, deploy, and manage infrastructure efficiently.

---

# Interview Question

## Difference Between Plan and Apply

### Answer

terraform plan only shows what changes Terraform will make. It does not create resources.

terraform apply actually creates, modifies, or deletes infrastructure based on the configuration.

---

# Interview Question

## Why Do We Use terraform validate?

### Answer

terraform validate checks the syntax and structure of Terraform configurations before deployment. It helps detect errors early and prevents deployment failures.

---

# Interview Question

## What Happens During terraform init?

### Answer

terraform init initializes the working directory by downloading required providers, modules, and dependencies, preparing Terraform for execution.

---

# Best Practices

- Always run validate before apply.
- Review plan output carefully.
- Use fmt for clean code.
- Avoid auto-approve in production.
- Store state remotely.
- Use version control.

---

# Summary

Most Important Commands:

```bash
terraform init
terraform validate
terraform plan
terraform apply
terraform destroy
```

Workflow:

```text
Init
  |
Validate
  |
Plan
  |
Apply
  |
Infrastructure
```

Understanding these commands is essential because they form the foundation of every Terraform deployment workflow.
