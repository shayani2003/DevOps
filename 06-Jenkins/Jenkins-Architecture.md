# Jenkins Architecture

## Introduction

Jenkins is an open-source automation server used to automate:

- Build
- Test
- Deploy
- CI/CD Pipelines

Jenkins helps developers deliver software faster and more reliably.

---

# What is Jenkins?

Jenkins is a Continuous Integration and Continuous Delivery (CI/CD) tool.

It automates repetitive software development tasks.

Without Jenkins:

```text
Developer
    |
Build Manually
    |
Test Manually
    |
Deploy Manually
```

Time-consuming and error-prone.

---

With Jenkins:

```text
Code Commit
     |
Jenkins
     |
Build
     |
Test
     |
Deploy
```

Automatic.

---

# Why Jenkins?

Benefits:

- Automation
- Faster Releases
- Reduced Human Errors
- Continuous Integration
- Continuous Deployment
- Large Plugin Ecosystem

---

# What is CI/CD?

## Continuous Integration (CI)

Developers frequently merge code into a shared repository.

```text
Developer
     |
Git Commit
     |
Jenkins Build
     |
Automated Testing
```

---

## Continuous Delivery (CD)

Application is always ready for deployment.

```text
Build
  |
Test
  |
Ready For Production
```

---

## Continuous Deployment

Application automatically deployed.

```text
Build
  |
Test
  |
Deploy
```

No manual approval.

---

# Jenkins Architecture Overview

```text
Developer
    |
Git Repository
    |
Jenkins Controller
    |
Jenkins Agent
    |
Build/Test/Deploy
```

---

# Jenkins Components

Main Components:

1. Jenkins Controller (Master)
2. Jenkins Agent (Slave)
3. Jobs
4. Pipelines
5. Plugins

---

# Jenkins Controller

## What is Jenkins Controller?

Controller is the brain of Jenkins.

Responsibilities:

- Manage jobs
- Schedule builds
- Manage agents
- Store configuration
- Provide Web UI

---

## Architecture

```text
Users
   |
Jenkins Controller
```

---

## Responsibilities

### Job Scheduling

Decides when jobs run.

---

### Agent Management

Assigns work to agents.

---

### Configuration Storage

Stores:

```text
Jobs

Credentials

Pipelines
```

---

### Web Interface

Provides Jenkins Dashboard.

---

# Jenkins Agent

## What is an Agent?

An Agent is a machine that executes tasks assigned by the Controller.

---

# Architecture

```text
Jenkins Controller
        |
        |
+-------+-------+
|               |
Agent 1      Agent 2
```

---

# Why Agents?

Suppose:

```text
100 Builds
```

Controller alone becomes overloaded.

Agents distribute workload.

---

# Agent Responsibilities

- Build Applications
- Run Tests
- Execute Scripts
- Deploy Applications

---

# Example

Controller:

```text
Assign Build
```

Agent:

```text
Execute Build
```

---

# Controller-Agent Workflow

```text
Developer
     |
Git Commit
     |
Controller
     |
Agent
     |
Build
```

---

# Jenkins Job

## What is a Job?

A Job is a task Jenkins executes.

Examples:

```text
Build Project

Run Tests

Deploy Application
```

---

# Job Workflow

```text
Job
 |
Build
 |
Test
 |
Deploy
```

---

# Types of Jenkins Jobs

### Freestyle Project

Simple job configuration.

---

### Pipeline

Infrastructure as Code approach.

---

### Multi-Branch Pipeline

Automatically detects branches.

---

# Jenkins Pipeline

A Pipeline defines CI/CD workflow.

Example:

```text
Build
 |
Test
 |
Deploy
```

---

# Pipeline Architecture

```text
Pipeline
   |
+--+--+--+
|     |  |
Build Test Deploy
```

---

# Jenkinsfile

Pipeline definition stored as code.

Example:

```groovy
pipeline {

  agent any

  stages {

    stage('Build') {

      steps {
        echo 'Building'
      }

    }

  }

}
```

---

# Jenkins Plugins

## What are Plugins?

Plugins extend Jenkins functionality.

Examples:

```text
Git Plugin

Docker Plugin

Kubernetes Plugin

AWS Plugin

Slack Plugin
```

---

# Plugin Architecture

```text
Jenkins
    |
Plugins
    |
Additional Features
```

---

# Jenkins Build Workflow

```text
Developer
     |
Git Push
     |
Jenkins Trigger
     |
Build
     |
Test
     |
Deploy
```

---

# Real World Architecture

```text
Developer
     |
GitHub
     |
Webhook
     |
Jenkins Controller
     |
Jenkins Agent
     |
Docker Build
     |
Kubernetes Deployment
```

---

# Jenkins Distributed Architecture

Small Team:

```text
Controller
```

only.

---

Large Team:

```text
Controller
      |
+-----+-----+-----+
|     |     |     |
A1    A2    A3   A4
```

Multiple Agents.

---

# Jenkins Security

Important Components:

```text
Authentication

Authorization

Credentials Management
```

---

# Credentials Management

Stores:

```text
GitHub Token

AWS Keys

Docker Credentials

SSH Keys
```

Securely.

---

# Jenkins High Availability

Production environments often use:

```text
Jenkins Controller
      |
Multiple Agents
```

to distribute workload.

---

# Example CI/CD Pipeline

Application:

```text
Spring Boot
```

Workflow:

```text
Code Commit
      |
GitHub
      |
Webhook
      |
Jenkins
      |
Build
      |
Unit Test
      |
Docker Build
      |
Push To Registry
      |
Deploy To Kubernetes
```

---

# Complete Jenkins Architecture

```text
Developer
      |
GitHub
      |
Webhook
      |
Jenkins Controller
      |
+-----+-----+
|           |
Agent1    Agent2
      |
Build/Test
      |
Deploy
```

---

# Interview Question

## What is Jenkins?

### Answer

Jenkins is an open-source automation server used to implement Continuous Integration and Continuous Delivery (CI/CD). It automates software build, testing, and deployment processes.

---

# Interview Question

## Explain Jenkins Architecture

### Answer

Jenkins architecture consists of a Controller and one or more Agents. The Controller manages jobs, scheduling, configurations, and user interactions, while Agents execute build, test, and deployment tasks assigned by the Controller.

---

# Interview Question

## What is the Difference Between Controller and Agent?

### Answer

The Controller manages Jenkins operations and job scheduling, while Agents perform the actual execution of builds, tests, and deployments.

---

# Interview Question

## Why Are Jenkins Agents Needed?

### Answer

Agents help distribute workloads across multiple machines, improving scalability, performance, and build execution speed.

---

# Interview Question

## What is a Jenkinsfile?

### Answer

A Jenkinsfile is a text file that defines a Jenkins Pipeline as code. It describes build, test, and deployment stages using Groovy syntax.

---

# Best Practices

- Use Pipelines instead of Freestyle Jobs.
- Store Jenkinsfiles in Git.
- Use Agents for scalability.
- Secure credentials properly.
- Backup Jenkins configuration.
- Keep plugins updated.

---

# Memory Trick

```text
Controller
     |
Manages

Agent
     |
Executes
```

Workflow:

```text
Git Push
    |
Jenkins
    |
Build
    |
Test
    |
Deploy
```

---

# Summary

Jenkins Components:

```text
Controller

Agent

Jobs

Pipelines

Plugins
```

Architecture:

```text
Developer
     |
Git
     |
Controller
     |
Agent
     |
Build/Test/Deploy
```

Jenkins is one of the most widely used CI/CD tools and remains a core topic in DevOps interviews because it automates software delivery pipelines efficiently.
