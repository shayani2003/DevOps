# Kubernetes ConfigMaps and Secrets

## Introduction

Applications require configuration values to run.

Examples:

```text
Database Host
API URL
Port Number
Username
Password
API Key
```

Instead of hardcoding these values inside the application, Kubernetes provides:

1. ConfigMap
2. Secret

---

# Why Not Hardcode Configuration?

Bad Example:

```java
String DB_HOST = "database.company.com";

String PASSWORD = "Admin@123";
```

Problems:

- Difficult to change
- Security risk
- Different values for Dev/Test/Prod

---

Better Approach:

```text
Application
      |
ConfigMap / Secret
      |
Configuration
```

---

# What is a ConfigMap?

A ConfigMap stores:

```text
Non-Sensitive Configuration Data
```

Examples:

```text
Application URL
Database Host
Environment Name
Feature Flags
Port Numbers
```

---

# ConfigMap Architecture

```text
ConfigMap
      |
Application Pod
      |
Configuration Values
```

---

# Example ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  APP_ENV: production
  DB_HOST: mysql-service
  PORT: "8080"
```

---

# ConfigMap Components

```text
Key
 |
Value
```

Example:

```text
APP_ENV -> production

PORT -> 8080
```

---

# Creating ConfigMap

From YAML:

```bash
kubectl apply -f configmap.yaml
```

---

Direct Command:

```bash
kubectl create configmap app-config \
--from-literal=APP_ENV=production
```

---

# Viewing ConfigMaps

List:

```bash
kubectl get configmaps
```

---

Details:

```bash
kubectl describe configmap app-config
```

---

# Using ConfigMap as Environment Variables

ConfigMap:

```yaml
data:
  APP_ENV: production
```

---

Pod:

```yaml
env:

- name: APP_ENV
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_ENV
```

---

# Architecture

```text
ConfigMap
      |
Environment Variable
      |
Application
```

---

# Using ConfigMap as Volume

ConfigMap:

```text
Configuration File
```

Mounted:

```text
Pod
 |
Volume
 |
Config File
```

---

# Example

```yaml
volumes:

- name: config-volume

  configMap:
    name: app-config
```

---

# What is a Secret?

A Secret stores:

```text
Sensitive Information
```

Examples:

```text
Passwords
API Keys
Tokens
Certificates
SSH Keys
```

---

# Secret Architecture

```text
Secret
   |
Pod
   |
Sensitive Data
```

---

# Secret Example

```yaml
apiVersion: v1
kind: Secret

metadata:
  name: db-secret

type: Opaque

data:
  username: YWRtaW4=
  password: UGFzc3dvcmQxMjM=
```

---

# Important Note

Secrets are stored using:

```text
Base64 Encoding
```

Example:

```text
admin
```

Encoded:

```text
YWRtaW4=
```

---

# Base64 is NOT Encryption

Interview Question Favorite.

Base64:

```text
Encoding
```

NOT:

```text
Encryption
```

Anyone can decode it.

---

# Creating Secret

Command:

```bash
kubectl create secret generic db-secret \
--from-literal=username=admin \
--from-literal=password=Password123
```

---

# Viewing Secrets

List:

```bash
kubectl get secrets
```

---

Details:

```bash
kubectl describe secret db-secret
```

---

# Viewing Secret Values

Command:

```bash
kubectl get secret db-secret -o yaml
```

Output:

```text
Base64 Encoded Values
```

---

# Using Secret as Environment Variable

```yaml
env:

- name: DB_PASSWORD

  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

---

# Architecture

```text
Secret
   |
Environment Variable
   |
Application
```

---

# Using Secret as Volume

```yaml
volumes:

- name: secret-volume

  secret:
    secretName: db-secret
```

---

Mounted inside container.

---

# ConfigMap vs Secret

| Feature | ConfigMap | Secret |
|----------|------------|---------|
| Purpose | Non-Sensitive Data | Sensitive Data |
| Examples | URL, Port, Hostname | Passwords, API Keys |
| Encoding | Plain Text | Base64 |
| Security | Low | Higher |
| Typical Use | Configuration | Credentials |

---

# Real World Example

Application Needs:

```text
Database Host

Database Password
```

---

Store:

Database Host:

```text
ConfigMap
```

---

Database Password:

```text
Secret
```

---

Architecture:

```text
Application
     |
+----+----+
|         |
ConfigMap Secret
```

---

# Production Architecture

```text
ConfigMap
     |
Application
     |
Secret
     |
Database
```

---

# Security Best Practices

## Use Secrets for Credentials

Never store:

```text
Passwords
API Keys
Tokens
```

inside ConfigMaps.

---

## Enable Encryption at Rest

Protect Secrets inside ETCD.

---

## Restrict Access with RBAC

Only authorized users should access Secrets.

---

## Use External Secret Managers

Examples:

```text
AWS Secrets Manager

HashiCorp Vault

Azure Key Vault
```

---

# Common Production Scenario

Application Fails.

Investigation:

```bash
kubectl describe pod app-pod
```

Output:

```text
Secret Not Found
```

Issue:

```text
db-secret missing
```

Create Secret.

Application works.

---

# Interview Question

## What is a ConfigMap?

### Answer

A ConfigMap is a Kubernetes object used to store non-sensitive configuration data such as application settings, hostnames, and environment variables.

---

# Interview Question

## What is a Secret?

### Answer

A Secret is a Kubernetes object used to store sensitive information such as passwords, API keys, certificates, and access tokens.

---

# Interview Question

## Difference Between ConfigMap and Secret

### Answer

ConfigMaps store non-sensitive configuration data, while Secrets store sensitive information. Secrets are Base64 encoded and should be protected using RBAC and encryption mechanisms.

---

# Interview Question

## Are Kubernetes Secrets Encrypted?

### Answer

By default, Secrets are Base64 encoded, not encrypted. For stronger security, ETCD encryption and external secret management solutions should be used.

---

# Interview Question

## How Can a Pod Access a Secret?

### Answer

A Pod can access a Secret through environment variables or mounted volumes.

---

# Best Practices

- Store passwords in Secrets.
- Store configuration in ConfigMaps.
- Enable ETCD encryption.
- Use RBAC.
- Rotate secrets regularly.
- Use external secret managers for production.

---

# Memory Trick

```text
ConfigMap
     |
Configuration

Secret
     |
Sensitive Information
```

Examples:

```text
ConfigMap
----------
DB_HOST
PORT
API_URL

Secret
-------
PASSWORD
TOKEN
API_KEY
```

---

# Summary

ConfigMap:

```text
Non-Sensitive Data
```

Examples:

```text
Hostname
Port
Application Settings
```

---

Secret:

```text
Sensitive Data
```

Examples:

```text
Passwords
API Keys
Certificates
```

Architecture:

```text
Application
     |
ConfigMap
     |
Secret
```

ConfigMaps and Secrets are fundamental Kubernetes objects and are frequently discussed in DevOps, Kubernetes, and Platform Engineering interviews.
