# Jenkins Pipeline

## Introduction

A Jenkins Pipeline is a set of automated steps that define how software moves from source code to production.

It is one of the most important Jenkins concepts and is heavily used in CI/CD.

Instead of manually:

```text
Build
Test
Deploy
```

Jenkins Pipelines automate the entire process.

---

# What is a Jenkins Pipeline?

A Jenkins Pipeline is a collection of stages and steps that automate software delivery.

Example:

```text
Code
  |
Build
  |
Test
  |
Deploy
```

Each stage executes automatically.

---

# Why Pipelines?

Without Pipeline:

```text
Developer
    |
Build Manually
    |
Test Manually
    |
Deploy Manually
```

Problems:

- Time consuming
- Human errors
- Inconsistent deployments

---

With Pipeline:

```text
Code Commit
      |
Jenkins Pipeline
      |
Build
      |
Test
      |
Deploy
```

Fully automated.

---

# Pipeline Architecture

```text
Developer
     |
Git Push
     |
Jenkins Pipeline
     |
+-----+-----+-----+
|     |     |     |
Build Test Deploy
```

---

# Pipeline as Code

Jenkins Pipelines are stored in:

```text
Jenkinsfile
```

inside the Git repository.

Benefits:

- Version Control
- Reproducibility
- Easy Maintenance

---

# Jenkinsfile Example

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {
                echo 'Building Application'
            }

        }

        stage('Test') {

            steps {
                echo 'Running Tests'
            }

        }

        stage('Deploy') {

            steps {
                echo 'Deploying Application'
            }

        }

    }

}
```

---

# Pipeline Components

Main Components:

1. Agent
2. Stages
3. Steps
4. Post Actions

---

# Agent

Defines where pipeline runs.

Example:

```groovy
agent any
```

Meaning:

```text
Run On Any Available Agent
```

---

# Stage

A Stage represents a logical phase.

Examples:

```text
Build

Test

Deploy
```

---

# Example

```groovy
stage('Build')
```

---

# Stage Architecture

```text
Pipeline
    |
+---+---+---+
|       |   |
Build Test Deploy
```

---

# Steps

Steps are actual commands executed.

Example:

```groovy
steps {

    echo 'Building'

}
```

---

# Example

```groovy
steps {

    sh 'mvn clean package'

}
```

Runs:

```bash
mvn clean package
```

---

# Pipeline Workflow

```text
Git Push
    |
Pipeline Trigger
    |
Build
    |
Test
    |
Deploy
```

---

# Real CI/CD Example

Spring Boot Application

Pipeline:

```text
Checkout Code
       |
Build
       |
Unit Tests
       |
Docker Build
       |
Push Image
       |
Deploy Kubernetes
```

---

# Declarative Pipeline

Most commonly used.

---

## Example

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {
                echo 'Build Stage'
            }

        }

    }

}
```

---

# Advantages

- Easy to read
- Structured
- Recommended by Jenkins

---

# Scripted Pipeline

More flexible.

Uses Groovy scripting.

---

## Example

```groovy
node {

    stage('Build') {

        echo 'Building'

    }

}
```

---

# Declarative vs Scripted

| Feature | Declarative | Scripted |
|----------|------------|------------|
| Syntax | Simple | Flexible |
| Learning Curve | Easy | Hard |
| Recommended | Yes | Advanced Use Cases |
| Structure | Strict | Flexible |

---

# Multi-Stage Pipeline

Example:

```text
Build
  |
Test
  |
Security Scan
  |
Deploy
```

---

# Pipeline Diagram

```text
Pipeline
   |
+--+--+--+--+
|     |     |
Build Test Scan Deploy
```

---

# Post Section

Used after pipeline completion.

Example:

```groovy
post {

    success {

        echo 'Pipeline Successful'

    }

}
```

---

# Common Post Actions

## Success

```groovy
success {

}
```

Runs when pipeline succeeds.

---

## Failure

```groovy
failure {

}
```

Runs when pipeline fails.

---

## Always

```groovy
always {

}
```

Runs every time.

---

# Complete Pipeline Example

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                sh 'mvn clean package'

            }

        }

        stage('Test') {

            steps {

                sh 'mvn test'

            }

        }

        stage('Deploy') {

            steps {

                echo 'Deploying'

            }

        }

    }

}
```

---

# Pipeline with Docker

Example:

```groovy
stage('Docker Build') {

    steps {

        sh 'docker build -t app .'

    }

}
```

---

# Pipeline with Kubernetes

Example:

```groovy
stage('Deploy') {

    steps {

        sh 'kubectl apply -f deployment.yaml'

    }

}
```

---

# Pipeline Execution Flow

```text
Developer
     |
Git Push
     |
Webhook
     |
Jenkins
     |
Pipeline
     |
Build
     |
Test
     |
Deploy
```

---

# Real Production Architecture

```text
GitHub
   |
Webhook
   |
Jenkins
   |
Pipeline
   |
Docker
   |
ECR
   |
Kubernetes
```

---

# Pipeline Benefits

## Automation

Reduces manual work.

---

## Faster Delivery

Continuous deployments.

---

## Consistency

Same process every time.

---

## Version Control

Stored in Git.

---

## Scalability

Supports large teams.

---

# Interview Question

## What is a Jenkins Pipeline?

### Answer

A Jenkins Pipeline is a collection of automated stages and steps that define the software delivery process, including build, test, and deployment activities. It enables Continuous Integration and Continuous Delivery.

---

# Interview Question

## What is a Jenkinsfile?

### Answer

A Jenkinsfile is a text file stored in a source code repository that defines a Jenkins Pipeline using Groovy-based syntax.

---

# Interview Question

## What are the Main Components of a Pipeline?

### Answer

The main components are:

```text
Agent

Stages

Steps

Post Actions
```

---

# Interview Question

## Difference Between Declarative and Scripted Pipeline

### Answer

Declarative Pipelines use a structured and simplified syntax and are recommended for most use cases. Scripted Pipelines provide greater flexibility but require more Groovy scripting knowledge.

---

# Interview Question

## Why Use Pipeline as Code?

### Answer

Pipeline as Code allows CI/CD workflows to be version-controlled, reviewed, shared, and maintained just like application code.

---

# Best Practices

- Store Jenkinsfile in Git.
- Use Declarative Pipelines.
- Keep stages small and focused.
- Use reusable Shared Libraries.
- Secure credentials properly.
- Implement failure notifications.

---

# Memory Trick

```text
Pipeline
   |
Build
   |
Test
   |
Deploy
```

Core Components:

```text
Agent

Stage

Step

Post
```

---

# Summary

Jenkins Pipeline:

```text
Code
  |
Build
  |
Test
  |
Deploy
```

Pipeline Types:

```text
Declarative

Scripted
```

Stored In:

```text
Jenkinsfile
```

Jenkins Pipelines are the foundation of modern CI/CD and are one of the most frequently asked topics in DevOps interviews.
