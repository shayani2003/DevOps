# Jenkins Shared Libraries

## Introduction

As Jenkins Pipelines grow larger, the same code is often repeated across multiple projects.

Example:

```text
Build Logic

Docker Build Logic

Deployment Logic

Notification Logic
```

Repeated in many Jenkinsfiles.

This creates:

- Code Duplication
- Maintenance Problems
- Inconsistent Pipelines

To solve this, Jenkins provides:

```text
Shared Libraries
```

---

# What are Jenkins Shared Libraries?

A Shared Library is a reusable collection of Groovy code that can be shared across multiple Jenkins Pipelines.

Think of it as:

```text
Java Project
      |
Reusable Methods

Jenkins
      |
Reusable Pipeline Code
```

---

# Why Shared Libraries?

Without Shared Libraries:

Project A:

```text
Build Code
Deploy Code
Notification Code
```

---

Project B:

```text
Same Build Code
Same Deploy Code
Same Notification Code
```

---

Project C:

```text
Again Same Code
```

---

Problem:

```text
Code Duplication
```

---

With Shared Libraries:

```text
Shared Library
      |
+-----+-----+-----+
|     |     |     |
A     B     C
```

Single reusable codebase.

---

# Shared Library Architecture

```text
Git Repository
      |
Shared Library
      |
Jenkins
      |
Pipeline
```

---

# Benefits

## Code Reusability

Write once.

Use everywhere.

---

## Easier Maintenance

Update library once.

All pipelines benefit.

---

## Standardization

All teams follow same pipeline standards.

---

## Cleaner Jenkinsfiles

Instead of:

```text
500 Lines
```

Can become:

```text
20 Lines
```

---

# Shared Library Structure

Typical Structure:

```text
shared-library/
│
├── vars/
│   ├── buildApp.groovy
│   ├── deployApp.groovy
│
├── src/
│   └── com/company/
│       └── Utils.groovy
│
└── resources/
```

---

# Directory Explanation

## vars/

Contains reusable pipeline functions.

Example:

```text
buildApp.groovy

deployApp.groovy
```

---

## src/

Contains Groovy classes.

Similar to Java packages.

---

## resources/

Stores static files.

Examples:

```text
Templates

Configuration Files
```

---

# vars Directory Example

File:

```text
vars/buildApp.groovy
```

Code:

```groovy
def call() {

    echo "Building Application"

}
```

---

# Using Shared Library

Pipeline:

```groovy
@Library('shared-library') _

pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                buildApp()

            }

        }

    }

}
```

---

# Execution Flow

```text
Pipeline
    |
buildApp()
    |
Shared Library
    |
Execute Function
```

---

# Creating a Deployment Function

File:

```text
vars/deployApp.groovy
```

Code:

```groovy
def call() {

    echo "Deploying Application"

}
```

---

Pipeline:

```groovy
deployApp()
```

---

# Shared Library Repository

Most organizations store libraries in:

```text
GitHub

GitLab

Bitbucket
```

---

Architecture:

```text
Git Repository
       |
Shared Library
       |
Jenkins
```

---

# Global Shared Library

Configured globally in Jenkins.

Available to all pipelines.

---

Architecture:

```text
Jenkins
     |
Global Library
     |
All Pipelines
```

---

# Configure Global Library

Jenkins:

```text
Manage Jenkins
      |
Configure System
      |
Global Pipeline Libraries
```

---

Add:

```text
Library Name

Git Repository URL
```

---

# Example Configuration

Library Name:

```text
shared-library
```

Repository:

```text
https://github.com/company/shared-library.git
```

---

# Loading Library

```groovy
@Library('shared-library') _
```

---

# Dynamic Library Loading

Example:

```groovy
library 'shared-library'
```

Loads library at runtime.

---

# Shared Library Workflow

```text
Developer
      |
Pipeline
      |
Shared Library Function
      |
Execute Logic
```

---

# Real World Example

Suppose organization has:

```text
50 Microservices
```

Each requires:

```text
Build

Docker Build

Push Image

Deploy Kubernetes
```

---

Without Shared Library:

```text
50 Jenkinsfiles

Same Code Repeated
```

---

With Shared Library:

```text
Shared Library
      |
Reusable Functions
      |
50 Pipelines
```

---

# Example Pipeline

Without Shared Library:

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                sh 'mvn clean package'

            }

        }

        stage('Deploy') {

            steps {

                sh 'kubectl apply -f deployment.yaml'

            }

        }

    }

}
```

---

With Shared Library:

```groovy
@Library('shared-library') _

pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                buildApp()

            }

        }

        stage('Deploy') {

            steps {

                deployApp()

            }

        }

    }

}
```

---

# Shared Library Lifecycle

```text
Developer Updates Library
         |
Git Push
         |
Jenkins Pulls Library
         |
Pipelines Use New Logic
```

---

# Advantages

## Reusable Code

Avoid duplication.

---

## Easier Updates

One update affects all pipelines.

---

## Standardized CI/CD

Consistent pipelines.

---

## Better Governance

Centralized control.

---

# Disadvantages

## Learning Curve

Requires Groovy knowledge.

---

## Dependency Risk

Library failure can affect many pipelines.

---

# Best Practices

## Version Libraries

Example:

```text
v1.0

v1.1

v2.0
```

---

## Keep Functions Small

Avoid huge utility methods.

---

## Use Git

Version control all libraries.

---

## Document Functions

Make them easy to use.

---

# Production Architecture

```text
GitHub
    |
Shared Library
    |
Jenkins
    |
100 Pipelines
```

---

# Interview Question

## What are Jenkins Shared Libraries?

### Answer

Jenkins Shared Libraries are reusable collections of Groovy code that allow common pipeline logic to be shared across multiple Jenkins Pipelines. They help reduce duplication and improve maintainability.

---

# Interview Question

## Why Use Shared Libraries?

### Answer

Shared Libraries enable code reuse, simplify pipeline maintenance, improve consistency, and allow organizations to standardize CI/CD practices across multiple projects.

---

# Interview Question

## What are the Main Directories in a Shared Library?

### Answer

The main directories are:

```text
vars/

src/

resources/
```

The vars directory contains reusable pipeline functions, src contains Groovy classes, and resources stores static files.

---

# Interview Question

## How Do You Load a Shared Library?

### Answer

A Shared Library can be loaded using:

```groovy
@Library('shared-library') _
```

This makes library functions available within the pipeline.

---

# Interview Question

## Where Are Shared Libraries Stored?

### Answer

Shared Libraries are typically stored in Git repositories such as GitHub, GitLab, or Bitbucket and configured in Jenkins as Global Pipeline Libraries.

---

# Memory Trick

```text
Shared Library
      |
Write Once
      |
Use Everywhere
```

Structure:

```text
vars/

src/

resources/
```

---

# Summary

Jenkins Shared Libraries provide:

```text
Code Reuse

Standardization

Maintainability

Scalability
```

Architecture:

```text
Git Repository
      |
Shared Library
      |
Jenkins Pipelines
```

They are widely used in enterprise CI/CD environments and are a common interview topic because they demonstrate how large organizations manage and standardize Jenkins Pipelines efficiently.
