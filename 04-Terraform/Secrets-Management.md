# Terraform Secrets Management

## What is Secrets Management?

Secrets Management is the process of securely storing, accessing, and managing sensitive information used by applications and infrastructure.

Examples of secrets:

```text
Passwords
API Keys
Database Credentials
SSH Keys
Access Tokens
Certificates
```

---

# Why Secrets Management is Important

Suppose we create an RDS database.

Bad Example:

```hcl
resource "aws_db_instance" "db" {

  username = "admin"

  password = "Admin@123"

}
```

Problem:

```text
Password Visible
```

Anyone with repository access can see it.

This is a major security risk.

---

# Risks of Hardcoding Secrets

## Security Breach

Secrets exposed publicly.

---

## Git History Exposure

Even if removed later:

```text
Git History Still Contains Secret
```

---

## Compliance Violations

May violate:

```text
SOC2
ISO27001
PCI-DSS
```

---

## Credential Leakage

Attackers can gain:

```text
Database Access
Cloud Access
API Access
```

---

# Bad Practice

Never do:

```hcl
password = "Admin@123"
```

or

```hcl
access_key = "AKIA123"
```

---

# Good Practice

Store secrets outside Terraform code.

---

# Method 1: Terraform Variables

## variables.tf

```hcl
variable "db_password" {
  sensitive = true
}
```

---

## terraform.tfvars

```hcl
db_password = "Admin@123"
```

---

Usage:

```hcl
password = var.db_password
```

---

## Benefits

- Better than hardcoding
- Reusable

---

## Limitation

Still stored in plain text.

Not ideal for production.

---

# Method 2: Environment Variables

Terraform automatically reads:

```text
TF_VAR_<variable_name>
```

Example:

```bash
export TF_VAR_db_password="Admin@123"
```

Windows:

```powershell
$env:TF_VAR_db_password="Admin@123"
```

---

Terraform:

```hcl
variable "db_password" {
  sensitive = true
}
```

Used as:

```hcl
password = var.db_password
```

---

## Benefits

- Not stored in code
- Easy CI/CD integration

---

# Method 3: AWS Secrets Manager (Recommended)

Most common production approach.

---

## Architecture

```text
Terraform
     |
AWS Secrets Manager
     |
Database Password
```

---

## Create Secret

AWS Console:

```text
Secrets Manager
      |
Create Secret
```

Store:

```text
Username
Password
```

---

## Terraform Example

```hcl
data "aws_secretsmanager_secret" "db" {

  name = "prod-db-password"

}
```

---

Retrieve Secret Value:

```hcl
data "aws_secretsmanager_secret_version" "db" {

  secret_id = data.aws_secretsmanager_secret.db.id

}
```

---

Use:

```hcl
password = data.aws_secretsmanager_secret_version.db.secret_string
```

---

## Advantages

- Encrypted
- Centralized
- Automatic Rotation
- Auditing

---

# Method 4: AWS Systems Manager Parameter Store

Alternative AWS solution.

---

## Architecture

```text
Terraform
     |
Parameter Store
     |
Secure String
```

---

Store:

```text
Database Password
API Key
```

---

Terraform Example

```hcl
data "aws_ssm_parameter" "db_password" {

  name = "/prod/db/password"

}
```

---

Usage:

```hcl
password = data.aws_ssm_parameter.db_password.value
```

---

# Method 5: HashiCorp Vault

Enterprise-grade solution.

Most popular secrets platform.

---

## Architecture

```text
Terraform
     |
HashiCorp Vault
     |
Secrets
```

---

## Advantages

- Dynamic Credentials
- Encryption
- Secret Rotation
- Fine-Grained Access Control

---

# Secret Rotation

## What is Rotation?

Changing secrets automatically.

Example:

```text
Old Password
     |
Rotate
     |
New Password
```

---

Benefits:

- Improved security
- Reduced credential exposure

---

AWS Secrets Manager supports:

```text
Automatic Rotation
```

---

# Terraform Sensitive Variables

Mark variables as sensitive.

Example:

```hcl
variable "db_password" {

  sensitive = true

}
```

---

Output:

```text
(sensitive value)
```

instead of showing password.

---

# Example Project

Bad:

```hcl
resource "aws_db_instance" "db" {

  password = "Admin@123"

}
```

---

Good:

```hcl
variable "db_password" {

  sensitive = true

}
```

Use:

```hcl
password = var.db_password
```

---

Better:

```text
AWS Secrets Manager
```

---

# Terraform State File Risk

Important Interview Topic.

Even if variables are sensitive:

```text
terraform.tfstate
```

may still contain secrets.

---

Example:

```text
Database Password
```

stored inside state file.

---

# Protection Methods

Use:

```text
Remote State
Encryption
Access Control
```

---

Example:

```text
S3 Bucket
     +
Encryption
     +
IAM Policies
```

---

# Production Architecture

```text
Terraform
      |
AWS Secrets Manager
      |
Database Password
      |
RDS Instance
```

---

# Interview Question

## How Do You Manage Secrets in Terraform?

### Answer

I avoid hardcoding secrets in Terraform code. For production environments, I store secrets in AWS Secrets Manager, AWS Systems Manager Parameter Store, or HashiCorp Vault and retrieve them securely through Terraform. This improves security and enables centralized secret management.

---

# Interview Question

## Why Shouldn't Secrets Be Hardcoded?

### Answer

Hardcoding secrets exposes sensitive information in source code repositories, Git history, and configuration files. This creates significant security risks and can lead to credential leakage.

---

# Interview Question

## What is the Best Way to Store Secrets in AWS?

### Answer

AWS Secrets Manager is generally the preferred solution because it provides encryption, access control, auditing, and automatic secret rotation.

---

# Interview Question

## What Does sensitive = true Do?

### Answer

The sensitive attribute prevents Terraform from displaying secret values in command outputs, helping reduce accidental exposure.

---

# Interview Question

## Are Secrets Stored in Terraform State?

### Answer

Yes. Some secrets may still appear in the Terraform state file. Therefore, state files should be stored securely using encrypted remote backends and restricted access controls.

---

# Best Practices

- Never hardcode passwords.
- Use AWS Secrets Manager.
- Use HashiCorp Vault for enterprise environments.
- Encrypt Terraform state files.
- Restrict access using IAM.
- Enable secret rotation.
- Mark variables as sensitive.
- Store state remotely.

---

# Common Production Setup

```text
Terraform
     |
AWS Secrets Manager
     |
Encrypted Secret
     |
Application / Database
```

Benefits:

- Secure
- Scalable
- Auditable
- Easy to manage

---

# Summary

Secrets include:

```text
Passwords
API Keys
Tokens
Certificates
```

Avoid:

```hcl
password = "Admin@123"
```

Use:

```text
AWS Secrets Manager
Parameter Store
HashiCorp Vault
Environment Variables
```

The most common production approach in AWS environments is AWS Secrets Manager because it provides secure storage, encryption, access control, and automatic secret rotation.
