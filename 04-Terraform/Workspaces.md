# Terraform Workspaces

## What are Terraform Workspaces?

Terraform Workspaces allow multiple environments to be managed using the same Terraform configuration.

Instead of creating separate Terraform projects for:

```text
Development
Testing
Production
```

we can use:

```text
Terraform Workspaces
```

to manage them efficiently.

---

# Why Workspaces are Needed

Suppose you have:

```text
Dev Environment
Test Environment
Prod Environment
```

Without Workspaces:

```text
dev/
test/
prod/
```

Multiple copies of Terraform code.

Problems:

- Duplicate code
- Difficult maintenance
- Increased errors

---

With Workspaces:

```text
One Code Base
      |
+-----+-----+-----+
|     |     |     |
Dev  Test  Prod
```

Cleaner and easier to manage.

---

# How Workspaces Work

Each Workspace has its own:

```text
Terraform State File
```

Example:

```text
default
dev
test
prod
```

Each environment is isolated.

---

# Workspace Architecture

```text
Terraform Code
       |
       |
+------+------+------+
|      |      |      |
Dev   Test   Prod
```

Each Workspace maintains:

```text
Separate State
```

---

# Default Workspace

When Terraform initializes:

```bash
terraform init
```

Terraform automatically creates:

```text
default
```

workspace.

---

# View Current Workspace

Command:

```bash
terraform workspace show
```

Output:

```text
default
```

---

# List Workspaces

Command:

```bash
terraform workspace list
```

Output:

```text
* default
  dev
  test
  prod
```

Current workspace marked:

```text
*
```

---

# Create Workspace

Command:

```bash
terraform workspace new dev
```

Output:

```text
Created and switched to workspace "dev"
```

---

Create:

```bash
terraform workspace new test
```

Create:

```bash
terraform workspace new prod
```

---

# Switch Workspace

Command:

```bash
terraform workspace select prod
```

Output:

```text
Switched to workspace "prod"
```

---

# Delete Workspace

Command:

```bash
terraform workspace delete dev
```

Removes workspace.

---

# Workspace Workflow

```text
Create Workspace
        |
Select Workspace
        |
Apply Terraform
        |
Separate State Created
```

---

# State File Separation

Without Workspaces:

```text
terraform.tfstate
```

One state file.

---

With Workspaces:

```text
dev.tfstate

test.tfstate

prod.tfstate
```

Separate state management.

---

# Example

Suppose:

```hcl
resource "aws_instance" "web" {
  instance_type = "t2.micro"
}
```

Current Workspace:

```text
dev
```

Apply:

```bash
terraform apply
```

Creates:

```text
Dev EC2 Instance
```

---

Switch:

```bash
terraform workspace select prod
```

Apply:

```bash
terraform apply
```

Creates:

```text
Production EC2 Instance
```

Same code.

Different environments.

---

# Using Workspace in Code

Terraform provides:

```hcl
terraform.workspace
```

Current workspace name.

---

## Example

```hcl
resource "aws_instance" "web" {

  instance_type = terraform.workspace == "prod" ? "t3.medium" : "t2.micro"

}
```

---

Result

Dev:

```text
t2.micro
```

Prod:

```text
t3.medium
```

Automatically selected.

---

# Practical Example

## Development

```text
Workspace = dev

Instance = t2.micro
```

Low cost.

---

## Production

```text
Workspace = prod

Instance = t3.large
```

Higher performance.

---

Same Terraform code.

---

# Real-World Scenario

Company has:

```text
Development
QA
Production
```

Environments.

Using Workspaces:

```text
dev
qa
prod
```

Benefits:

- Single codebase
- Multiple environments
- Separate state files

---

# Workspace Lifecycle

```text
terraform workspace new dev
          |
terraform workspace select dev
          |
terraform apply
          |
Infrastructure Created
```

---

# Important Commands

Create Workspace:

```bash
terraform workspace new dev
```

---

List Workspaces:

```bash
terraform workspace list
```

---

Show Current Workspace:

```bash
terraform workspace show
```

---

Switch Workspace:

```bash
terraform workspace select prod
```

---

Delete Workspace:

```bash
terraform workspace delete dev
```

---

# Workspace vs Separate Directories

## Workspaces

```text
One Code Base
Multiple States
```

Advantages:

- Less duplication
- Easier maintenance

---

## Separate Directories

```text
dev/
test/
prod/
```

Advantages:

- More flexibility
- Better for large projects

---

# Limitations of Workspaces

Workspaces are useful for:

```text
Small
Medium
Projects
```

---

For very large organizations:

Separate repositories are often preferred.

---

# Interview Question

## What are Terraform Workspaces?

### Answer

Terraform Workspaces allow multiple environments to be managed using the same Terraform configuration. Each workspace maintains its own state file, enabling environment isolation while reusing the same infrastructure code.

---

# Interview Question

## Why Use Workspaces?

### Answer

Workspaces reduce code duplication by allowing development, testing, and production environments to share the same Terraform code while maintaining separate state files.

---

# Interview Question

## How Do You Create a Workspace?

### Answer

Use:

```bash
terraform workspace new dev
```

This creates and switches to a new workspace called dev.

---

# Interview Question

## How Do Workspaces Store State?

### Answer

Each workspace maintains a separate Terraform state file, ensuring that resources from one environment do not affect another.

---

# Interview Question

## When Would You Use Workspaces?

### Answer

I use Workspaces when managing multiple environments such as development, testing, and production with the same Terraform codebase. This helps reduce duplication and simplifies infrastructure management.

---

# Best Practices

- Use meaningful workspace names.
- Keep environments isolated.
- Store state remotely.
- Use variables with workspaces.
- Avoid using workspaces for highly complex environments.
- Protect production workspaces.

---

# Common Production Example

```text
Terraform Code
       |
+------+------+------+
|      |      |      |
Dev   QA    Prod
```

Each environment:

```text
Separate State
Separate Resources
```

---

# Summary

Terraform Workspaces provide:

```text
Environment Isolation
+
Single Code Base
+
Separate State Files
```

Key Commands:

```bash
terraform workspace new

terraform workspace list

terraform workspace show

terraform workspace select

terraform workspace delete
```

Workspaces are an effective way to manage multiple environments while keeping infrastructure code clean, reusable, and maintainable.
