# Terraform Basics

## What is Terraform?

Terraform is an Infrastructure as Code (IaC) tool developed by HashiCorp.

It allows you to create, update, and manage infrastructure using configuration files instead of manually creating resources through cloud consoles.

---

# What is Infrastructure as Code (IaC)?

Infrastructure as Code means managing infrastructure through code.

Instead of:

```text
AWS Console
     |
Click
Click
Click
Create EC2
```

We write:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

Terraform creates the infrastructure automatically.

---

# Why Terraform?

Without Terraform:

```text
Manual Configuration
        |
Human Errors
        |
Inconsistent Environments
```

---

With Terraform:

```text
Code
  |
Terraform
  |
Infrastructure
```

Benefits:

- Automation
- Consistency
- Version Control
- Reusability
- Faster Deployments

---

# Terraform Architecture

```text
Terraform Configuration
           |
           v
     Terraform CLI
           |
           v
      Provider
           |
           v
Cloud Platform
(AWS/Azure/GCP)
```

---

# How Terraform Works

Step 1:

Write configuration.

```hcl
resource "aws_instance" "web" {
  instance_type = "t2.micro"
}
```

---

Step 2:

Initialize Terraform.

```bash
terraform init
```

---

Step 3:

Preview changes.

```bash
terraform plan
```

---

Step 4:

Apply changes.

```bash
terraform apply
```

---

Step 5:

Infrastructure created.

```text
EC2
VPC
S3
Database
```

---

# Core Terraform Components

Terraform consists of:

1. Provider
2. Resource
3. Variable
4. Output
5. State

---

# Provider

A provider allows Terraform to interact with platforms.

Examples:

```text
AWS
Azure
GCP
GitHub
Kubernetes
```

Example:

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

---

# Resource

A resource is any infrastructure component managed by Terraform.

Examples:

```text
EC2 Instance
S3 Bucket
VPC
Load Balancer
```

Example:

```hcl
resource "aws_s3_bucket" "demo" {
  bucket = "my-demo-bucket"
}
```

---

# Variable

Variables make configurations reusable.

Example:

```hcl
variable "instance_type" {
  default = "t2.micro"
}
```

Usage:

```hcl
instance_type = var.instance_type
```

---

# Output

Outputs display useful information after deployment.

Example:

```hcl
output "public_ip" {
  value = aws_instance.web.public_ip
}
```

Output:

```text
54.123.45.67
```

---

# State File

Terraform stores infrastructure information in:

```text
terraform.tfstate
```

Purpose:

- Tracks resources
- Maintains mappings
- Detects changes

---

# Terraform Workflow

```text
Write Code
     |
terraform init
     |
terraform plan
     |
terraform apply
     |
Infrastructure Created
```

---

# Terraform File Structure

```text
project/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── provider.tf
└── terraform.tfvars
```

---

# Common Terraform Files

## main.tf

Contains resources.

Example:

```hcl
resource "aws_instance" "web" {
  instance_type = "t2.micro"
}
```

---

## variables.tf

Stores variables.

---

## outputs.tf

Stores outputs.

---

## provider.tf

Defines cloud provider.

---

## terraform.tfvars

Stores variable values.

Example:

```hcl
instance_type = "t3.micro"
```

---

# Example: Create an EC2 Instance

Provider:

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

---

Resource:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

---

Apply:

```bash
terraform apply
```

Result:

```text
EC2 Instance Created
```

---

# Terraform Lifecycle

```text
Code
  |
Plan
  |
Apply
  |
Infrastructure
  |
Destroy
```

---

Destroy Infrastructure

```bash
terraform destroy
```

Removes resources.

---

# Benefits of Terraform

## Multi-Cloud Support

Supports:

```text
AWS
Azure
GCP
```

---

## Version Control

Infrastructure stored in Git.

---

## Automation

No manual resource creation.

---

## Reusability

Modules can be reused.

---

## Consistency

Same infrastructure every time.

---

# Terraform vs CloudFormation

| Feature | Terraform | CloudFormation |
|-----------|------------|---------------|
| Provider Support | Multi-Cloud | AWS Only |
| Language | HCL | JSON/YAML |
| Learning Curve | Easy | Medium |
| Portability | High | Low |

---

# Real-World Example

Developer needs:

```text
1 VPC
2 EC2 Instances
1 RDS Database
```

Without Terraform:

```text
Manual Creation
```

May take hours.

---

With Terraform:

```bash
terraform apply
```

Everything created automatically.

---

# Interview Question

## What is Terraform?

### Answer

Terraform is an Infrastructure as Code tool developed by HashiCorp that allows infrastructure to be defined, provisioned, and managed using code. It supports multiple cloud providers and helps automate infrastructure deployment.

---

# Interview Question

## What is Infrastructure as Code?

### Answer

Infrastructure as Code is the practice of managing infrastructure through configuration files instead of manual processes. It improves consistency, automation, and version control.

---

# Interview Question

## What is a Provider in Terraform?

### Answer

A Provider is a plugin that allows Terraform to interact with external platforms such as AWS, Azure, Google Cloud, Kubernetes, and GitHub.

---

# Interview Question

## What is Terraform State?

### Answer

Terraform State is a file that stores information about managed infrastructure resources. It helps Terraform track existing resources and determine what changes are needed.

---

# Best Practices

- Store code in Git.
- Use remote state storage.
- Avoid hardcoding secrets.
- Use variables.
- Use modules for reusability.
- Review plans before applying.

---

# Summary

Terraform is an Infrastructure as Code tool used to automate infrastructure provisioning.

Core Components:

```text
Provider
Resource
Variable
Output
State
```

Workflow:

```text
Write Code
     |
Init
     |
Plan
     |
Apply
     |
Infrastructure
```

Terraform is one of the most important DevOps tools because it enables automated, repeatable, and version-controlled infrastructure management.
