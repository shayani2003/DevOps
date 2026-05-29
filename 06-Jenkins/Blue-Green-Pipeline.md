# Blue-Green Deployment Pipeline in Jenkins

## Introduction

Blue-Green Deployment is a deployment strategy used to release new application versions with:

- Zero Downtime
- Fast Rollback
- Reduced Deployment Risk

It works by maintaining two identical environments:

```text
Blue Environment
Green Environment
```

Only one environment serves production traffic at a time.

---

# What is Blue-Green Deployment?

Suppose:

Current Production:

```text
Blue Environment
```

New Version:

```text
Green Environment
```

Deploy the new application to Green.

After testing:

```text
Switch Traffic
```

from Blue to Green.

---

# Architecture

```text
                 Users
                    |
                    |
            Load Balancer
               /      \
              /        \
             /          \
        Blue Env     Green Env
        Version 1    Version 2
```

---

# Why Use Blue-Green Deployment?

Traditional Deployment:

```text
Stop Old Version
      |
Deploy New Version
      |
Users Impacted
```

---

Blue-Green Deployment:

```text
Deploy New Version
      |
Test
      |
Switch Traffic
      |
No Downtime
```

---

# Benefits

## Zero Downtime

Users continue accessing application.

---

## Fast Rollback

Traffic can instantly switch back.

---

## Safer Releases

Production not impacted during deployment.

---

## Easy Validation

New version tested before exposure.

---

# Blue-Green Workflow

```text
Version 1 Running
        |
Deploy Version 2
        |
Test Version 2
        |
Switch Traffic
        |
Version 2 Live
```

---

# Jenkins and Blue-Green Deployment

Jenkins automates:

```text
Build
 |
Test
 |
Docker Build
 |
Push Image
 |
Deploy Green
 |
Validate
 |
Switch Traffic
```

---

# Complete Architecture

```text
Developer
     |
GitHub
     |
Webhook
     |
Jenkins
     |
Docker Build
     |
Docker Registry
     |
Kubernetes
     |
Blue-Green Deployment
```

---

# Real Production Example

Current Production:

```text
Blue

Frontend v1
Backend v1
```

---

New Release:

```text
Frontend v2
Backend v2
```

Deploy:

```text
Green Environment
```

---

After Testing:

```text
Load Balancer

Blue -> Green
```

Traffic switched.

---

# Kubernetes Blue-Green Architecture

```text
Users
  |
Load Balancer
  |
+------+------+
|             |
Blue       Green
Pods        Pods
```

---

# Jenkins Pipeline Stages

Typical Pipeline:

```text
Checkout
   |
Build
   |
Test
   |
Docker Build
   |
Push Image
   |
Deploy Green
   |
Validate
   |
Switch Traffic
```

---

# Stage 1: Checkout

```groovy
stage('Checkout') {

    steps {

        git 'https://github.com/company/app.git'

    }

}
```

---

# Stage 2: Build

```groovy
stage('Build') {

    steps {

        sh 'mvn clean package'

    }

}
```

---

# Stage 3: Test

```groovy
stage('Test') {

    steps {

        sh 'mvn test'

    }

}
```

---

# Stage 4: Docker Build

```groovy
stage('Docker Build') {

    steps {

        sh 'docker build -t app:v2 .'

    }

}
```

---

# Stage 5: Push Image

```groovy
stage('Push Image') {

    steps {

        sh 'docker push company/app:v2'

    }

}
```

---

# Stage 6: Deploy Green

Deploy new version.

```groovy
stage('Deploy Green') {

    steps {

        sh 'kubectl apply -f green-deployment.yaml'

    }

}
```

---

# Architecture

```text
Blue Environment
      |
Still Serving Traffic

Green Environment
      |
New Version Deploying
```

---

# Stage 7: Validation

Verify:

```text
Health Checks

API Checks

Smoke Tests
```

---

Example:

```groovy
stage('Validate') {

    steps {

        sh 'curl http://green-app/health'

    }

}
```

---

# Stage 8: Switch Traffic

Update service selector.

Before:

```yaml
selector:
  version: blue
```

---

After:

```yaml
selector:
  version: green
```

---

Result:

```text
Traffic
   |
Green Environment
```

---

# Traffic Switch Diagram

Before:

```text
Users
  |
Blue
```

---

After:

```text
Users
  |
Green
```

---

# Rollback Strategy

Suppose Green fails.

---

Traffic Switch:

```text
Green
   |
Errors
```

---

Rollback:

```text
Users
  |
Blue
```

Immediate recovery.

---

# Rollback Workflow

```text
Green Failure
      |
Switch Back
      |
Blue Active
```

---

# Jenkins Rollback Stage

Example:

```groovy
stage('Rollback') {

    steps {

        sh 'kubectl apply -f blue-service.yaml'

    }

}
```

---

# Complete Jenkins Pipeline Example

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

        stage('Docker Build') {

            steps {

                sh 'docker build -t app:v2 .'

            }

        }

        stage('Deploy Green') {

            steps {

                sh 'kubectl apply -f green.yaml'

            }

        }

        stage('Validate') {

            steps {

                sh 'curl http://green-app/health'

            }

        }

        stage('Switch Traffic') {

            steps {

                sh 'kubectl apply -f green-service.yaml'

            }

        }

    }

}
```

---

# Production Architecture

```text
GitHub
   |
Webhook
   |
Jenkins
   |
Build
   |
Docker
   |
Registry
   |
Kubernetes
   |
Blue Environment
Green Environment
```

---

# Common Validation Checks

Before switching traffic:

```text
Health Check

Smoke Test

API Test

Security Scan

Performance Check
```

---

# Advantages

## Zero Downtime

No user interruption.

---

## Fast Rollback

Traffic switch takes seconds.

---

## Reduced Risk

Production environment remains available.

---

## Better Testing

Validate before release.

---

# Disadvantages

## Higher Cost

Two environments required.

---

## More Resources

Duplicate infrastructure.

---

# Interview Question

## What is Blue-Green Deployment?

### Answer

Blue-Green Deployment is a release strategy that maintains two identical environments. The current version runs in the Blue environment while the new version is deployed to the Green environment. After validation, traffic is switched to Green. If issues occur, traffic can quickly be redirected back to Blue.

---

# Interview Question

## How Would You Implement Blue-Green Deployment in Jenkins?

### Answer

I would create a Jenkins Pipeline that builds the application, runs tests, creates a Docker image, deploys the new version to the Green environment, performs validation checks, and then updates the load balancer or Kubernetes service to route traffic from Blue to Green.

---

# Interview Question

## How Do You Roll Back a Failed Deployment?

### Answer

If the Green environment fails validation or experiences production issues, I switch traffic back to the Blue environment using the load balancer or Kubernetes service configuration.

---

# Interview Question

## Why is Blue-Green Deployment Better Than Traditional Deployment?

### Answer

Blue-Green Deployment provides zero downtime, faster rollback, safer releases, and better validation because the new version is tested before receiving production traffic.

---

# Best Practices

- Always validate Green before switching traffic.
- Automate rollback procedures.
- Use health checks.
- Monitor application metrics.
- Use Infrastructure as Code.
- Keep Blue environment available until deployment is verified.

---

# Memory Trick

```text
Blue
  |
Current Production
```

```text
Green
  |
New Version
```

Workflow:

```text
Deploy Green
      |
Validate
      |
Switch Traffic
      |
Success
```

---

# Summary

Blue-Green Deployment:

```text
Blue = Current Version

Green = New Version
```

Pipeline:

```text
Build
 |
Test
 |
Docker Build
 |
Deploy Green
 |
Validate
 |
Switch Traffic
```

Benefits:

```text
Zero Downtime

Fast Rollback

Safer Releases
```

Blue-Green Deployment is a highly valued DevOps concept because it enables reliable and low-risk production deployments while maintaining application availability.
