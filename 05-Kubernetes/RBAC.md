# Kubernetes RBAC

## What is RBAC?

RBAC stands for:

```text
Role-Based Access Control
```

RBAC is a Kubernetes security mechanism used to control:

```text
Who can do What
On Which Resource
```

It allows administrators to define permissions for users, groups, or service accounts.

---

# Why RBAC is Needed

Imagine a Kubernetes cluster with multiple teams:

```text
Developers
DevOps Engineers
QA Team
Security Team
```

Without RBAC:

```text
Everyone Can Access Everything
```

This creates:

- Security Risks
- Accidental Changes
- Unauthorized Access

---

With RBAC:

```text
Developer
    |
Can View Pods

DevOps
    |
Can Manage Deployments

Admin
    |
Full Access
```

---

# RBAC Architecture

```text
User
  |
RoleBinding
  |
Role
  |
Permissions
  |
Resources
```

---

# RBAC Components

Kubernetes RBAC consists of:

1. Role
2. ClusterRole
3. RoleBinding
4. ClusterRoleBinding

---

# RBAC Overview

```text
User
  |
RoleBinding
  |
Role
  |
Permissions
```

---

# What is a Role?

A Role defines permissions within a specific namespace.

Example:

```text
Namespace: dev
```

Permissions:

```text
View Pods
Create Pods
Delete Pods
```

---

## Role Example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role

metadata:
  namespace: dev
  name: pod-reader

rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get","list","watch"]
```

---

# Understanding the Role

Resources:

```text
Pods
```

Allowed Actions:

```text
get
list
watch
```

User cannot:

```text
Delete Pods
```

---

# What is a ClusterRole?

A ClusterRole defines permissions across the entire cluster.

Not limited to one namespace.

---

## Example

Permissions:

```text
View Nodes

View Namespaces

View All Pods
```

Cluster-wide access.

---

## ClusterRole Example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole

metadata:
  name: cluster-reader

rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["get","list"]
```

---

# Role vs ClusterRole

| Feature | Role | ClusterRole |
|----------|---------|-------------|
| Scope | Namespace | Entire Cluster |
| Access | Limited | Cluster-Wide |
| Resource Visibility | Namespace Resources | All Resources |

---

# What is a RoleBinding?

A RoleBinding connects:

```text
User
   |
Role
```

Without RoleBinding:

```text
Role Exists

User Cannot Use It
```

---

# Architecture

```text
User
  |
RoleBinding
  |
Role
```

---

## Example

Role:

```text
pod-reader
```

User:

```text
john
```

RoleBinding:

```text
john -> pod-reader
```

---

## YAML Example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding

metadata:
  name: read-pods

subjects:
- kind: User
  name: john

roleRef:
  kind: Role
  name: pod-reader
```

---

# What is ClusterRoleBinding?

ClusterRoleBinding connects:

```text
User
   |
ClusterRole
```

Provides cluster-wide access.

---

## Architecture

```text
User
  |
ClusterRoleBinding
  |
ClusterRole
```

---

## Example

```text
Admin User
      |
ClusterRoleBinding
      |
Cluster Admin
```

Access:

```text
Entire Cluster
```

---

# Complete RBAC Flow

```text
User
  |
RoleBinding
  |
Role
  |
Permissions
  |
Pods
```

---

# Verbs in RBAC

Verbs define actions.

Common verbs:

```text
get
list
watch
create
update
patch
delete
```

---

# Examples

## Read Only

```yaml
verbs:
- get
- list
- watch
```

---

## Full Control

```yaml
verbs:
- get
- list
- create
- update
- delete
```

---

# Common Kubernetes Resources

```text
pods
deployments
services
configmaps
secrets
nodes
namespaces
```

---

# Real World Example

Developer Requirements:

```text
View Pods

View Logs

Cannot Delete Deployments
```

Solution:

```text
Role
 +
RoleBinding
```

---

# Example Architecture

```text
Developer
     |
RoleBinding
     |
Read-Only Role
     |
Pods
```

---

# Service Accounts and RBAC

Applications running inside Kubernetes often use:

```text
Service Accounts
```

instead of users.

---

## Example

```text
Application Pod
       |
Service Account
       |
RoleBinding
       |
Permissions
```

---

# RBAC and Security

Without RBAC:

```text
Anyone Can Modify Cluster
```

---

With RBAC:

```text
Least Privilege Access
```

Users get only required permissions.

---

# Principle of Least Privilege

Grant only necessary permissions.

Bad:

```text
Developer -> Cluster Admin
```

---

Good:

```text
Developer -> Read Pods Only
```

---

# Checking Permissions

Command:

```bash
kubectl auth can-i create pods
```

Output:

```text
yes
```

or

```text
no
```

---

## Check Another User

```bash
kubectl auth can-i delete deployment --as=john
```

---

# Common Production Setup

```text
Developers
     |
Read Only Access

DevOps Team
     |
Deployment Access

Admins
     |
Cluster Admin Access
```

---

# Interview Question

## What is RBAC in Kubernetes?

### Answer

RBAC (Role-Based Access Control) is a Kubernetes authorization mechanism that controls access to cluster resources based on roles assigned to users, groups, or service accounts.

---

# Interview Question

## Difference Between Role and ClusterRole

### Answer

A Role provides permissions within a specific namespace, while a ClusterRole provides permissions across the entire Kubernetes cluster.

---

# Interview Question

## Difference Between RoleBinding and ClusterRoleBinding

### Answer

RoleBinding assigns a Role to a user within a namespace, whereas ClusterRoleBinding assigns a ClusterRole across the entire cluster.

---

# Interview Question

## Why is RBAC Important?

### Answer

RBAC improves cluster security by ensuring users and applications receive only the permissions they require. This follows the principle of least privilege and reduces the risk of unauthorized access.

---

# Interview Question

## How Can You Check Permissions?

### Answer

Use:

```bash
kubectl auth can-i <action> <resource>
```

Example:

```bash
kubectl auth can-i create pods
```

This verifies whether the current user has the specified permission.

---

# Best Practices

- Follow Least Privilege Principle.
- Avoid unnecessary Cluster Admin access.
- Use Service Accounts for applications.
- Audit permissions regularly.
- Separate developer and administrator roles.
- Protect sensitive resources like Secrets.

---

# Summary

RBAC Components:

```text
Role
ClusterRole
RoleBinding
ClusterRoleBinding
```

Flow:

```text
User
  |
Binding
  |
Role
  |
Permissions
  |
Resource
```

Memory Trick:

```text
Role              -> Namespace

ClusterRole       -> Entire Cluster

RoleBinding       -> Assign Role

ClusterRoleBinding -> Assign ClusterRole
```

RBAC is one of the most important Kubernetes security concepts and is frequently discussed in DevOps, SRE, and Platform Engineer interviews.
