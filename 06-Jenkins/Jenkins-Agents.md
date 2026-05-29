# Jenkins Agents

## Introduction

As Jenkins usage grows, a single Jenkins server may not be enough to handle all build, test, and deployment tasks.

To solve this problem, Jenkins uses:

```text
Agents
```

Agents help distribute workload across multiple machines and improve scalability.

---

# What is a Jenkins Agent?

A Jenkins Agent is a machine that performs tasks assigned by the Jenkins Controller.

The Controller manages jobs, while Agents execute them.

---

# Basic Architecture

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

# Why Are Agents Needed?

Suppose:

```text
100 Developers
```

continuously commit code.

Without Agents:

```text
Controller
     |
Build
Test
Deploy
```

Everything runs on one machine.

Problems:

- Slow builds
- High CPU usage
- Memory bottlenecks
- Single point of failure

---

With Agents:

```text
                Controller
                     |
       +-------------+-------------+
       |             |             |
       v             v             v
    Agent1       Agent2       Agent3
```

Workload distributed.

---

# Controller vs Agent

| Feature | Controller | Agent |
|----------|------------|--------|
| Job Scheduling | Yes | No |
| UI Access | Yes | No |
| Stores Configuration | Yes | No |
| Executes Builds | Can, but not recommended | Yes |
| Manages Pipelines | Yes | No |

---

# Agent Responsibilities

Agents perform:

- Build Execution
- Unit Testing
- Integration Testing
- Docker Builds
- Deployments
- Security Scans

---

# Workflow

```text
Git Push
    |
Controller
    |
Assign Job
    |
Agent
    |
Execute Build
```

---

# Example

Developer pushes code.

```text
GitHub
   |
Webhook
   |
Jenkins Controller
   |
Agent
   |
Build Application
```

---

# Types of Jenkins Agents

1. Permanent Agent
2. Dynamic Agent
3. Docker Agent
4. Kubernetes Agent

---

# Permanent Agent

Always running.

Example:

```text
Linux VM

Windows VM

Physical Server
```

---

## Architecture

```text
Controller
     |
Permanent Agent
```

---

## Advantages

- Stable
- Easy setup

---

## Disadvantages

- Resource consumption
- Costly

---

# Dynamic Agent

Created only when needed.

---

## Workflow

```text
Build Request
      |
Create Agent
      |
Run Build
      |
Destroy Agent
```

---

## Advantages

- Cost efficient
- Scalable

---

## Disadvantages

- Slight startup delay

---

# Docker Agent

Uses Docker containers as build environments.

---

## Architecture

```text
Controller
      |
Docker Container
      |
Build
```

---

# Example

```groovy
pipeline {

    agent {

        docker {

            image 'maven:3.9'

        }

    }

}
```

---

# Benefits

- Consistent Environment
- Fast Setup
- Easy Cleanup

---

# Kubernetes Agent

Most modern Jenkins setups use Kubernetes.

---

## Architecture

```text
Jenkins Controller
        |
Kubernetes Cluster
        |
Temporary Pod
        |
Build
```

---

# Workflow

```text
Pipeline Start
      |
Create Pod
      |
Run Build
      |
Delete Pod
```

---

# Advantages

- Highly Scalable
- Cloud Native
- Cost Efficient

---

# Example

```text
Controller
     |
Kubernetes
     |
Pod Agent
     |
Build
```

---

# Agent Labels

Labels help Jenkins choose the correct Agent.

---

## Example

Agent Label:

```text
linux
```

Pipeline:

```groovy
agent {

    label 'linux'

}
```

---

Jenkins chooses:

```text
Linux Agent
```

---

# Multiple Agent Example

```text
Controller
    |
+---+---+---+
|       |   |
Linux Windows Docker
```

---

Pipeline:

```groovy
agent {

    label 'docker'

}
```

Jenkins selects Docker Agent.

---

# Agent Communication

Controller and Agents communicate using:

```text
SSH

JNLP

WebSocket
```

---

# SSH Agent

Most common setup.

---

## Architecture

```text
Controller
     |
SSH
     |
Agent
```

---

# JNLP Agent

Agent initiates connection.

Useful behind firewalls.

---

## Architecture

```text
Agent
   |
JNLP
   |
Controller
```

---

# Real Production Example

Application:

```text
Spring Boot
```

Pipeline:

```text
Build
Test
Docker Build
Deploy
```

Architecture:

```text
Controller
     |
+----+----+
|         |
Agent1  Agent2
```

Agent1:

```text
Build + Test
```

Agent2:

```text
Docker + Deploy
```

---

# Jenkins Agent Workflow

```text
Developer
      |
Git Push
      |
Controller
      |
Select Agent
      |
Execute Pipeline
      |
Result
```

---

# Why Avoid Running Builds on Controller?

Bad Practice:

```text
Controller
      |
Build Everything
```

Problems:

- Performance issues
- Security concerns
- Reduced availability

---

Best Practice:

```text
Controller
      |
Schedule Only
      |
Agents Execute Jobs
```

---

# Scaling Jenkins

Small Team:

```text
Controller
```

Only.

---

Medium Team:

```text
Controller
      |
2-3 Agents
```

---

Large Enterprise:

```text
Controller
      |
Kubernetes Agents
      |
Auto Scaling
```

---

# Interview Question

## What is a Jenkins Agent?

### Answer

A Jenkins Agent is a machine that executes build, test, and deployment tasks assigned by the Jenkins Controller. It helps distribute workload and improve scalability.

---

# Interview Question

## Why Are Jenkins Agents Used?

### Answer

Agents allow Jenkins to distribute workloads across multiple machines, improving performance, scalability, and resource utilization.

---

# Interview Question

## Difference Between Controller and Agent

### Answer

The Controller manages Jenkins configuration, scheduling, and user interactions, while Agents execute the actual build, test, and deployment tasks.

---

# Interview Question

## What Are Dynamic Agents?

### Answer

Dynamic Agents are created on demand for a build and destroyed after the job completes. They help reduce infrastructure costs and improve scalability.

---

# Interview Question

## Why Are Kubernetes Agents Popular?

### Answer

Kubernetes Agents are highly scalable and can automatically create temporary Pods for builds. They are ideal for cloud-native CI/CD environments.

---

# Best Practices

- Avoid running builds on the Controller.
- Use Agent Labels.
- Use Dynamic Agents when possible.
- Use Docker or Kubernetes Agents.
- Monitor Agent resource usage.
- Secure Agent communication.

---

# Memory Trick

```text
Controller
     |
Manages
```

```text
Agent
     |
Executes
```

Workflow:

```text
Git Push
     |
Controller
     |
Agent
     |
Build/Test/Deploy
```

---

# Summary

Jenkins Agents:

```text
Execute Jobs
```

Types:

```text
Permanent Agent

Dynamic Agent

Docker Agent

Kubernetes Agent
```

Architecture:

```text
Controller
      |
Agents
      |
Build/Test/Deploy
```

Jenkins Agents are essential for scaling CI/CD pipelines and are a very common topic in DevOps interviews because they directly impact performance, scalability, and automation.
