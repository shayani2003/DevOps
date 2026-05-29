# CI/CD Pipeline

## What is CI/CD?

CI/CD stands for:

- CI → Continuous Integration
- CD → Continuous Delivery / Continuous Deployment

CI/CD is a software development practice that automates the process of building, testing, and deploying applications.

---

# Why CI/CD is Needed

Before CI/CD:

```text
Developer
    |
Manual Build
    |
Manual Testing
    |
Manual Deployment
    |
Production
```

Problems:

- Slow releases
- Human errors
- Inconsistent deployments

---

With CI/CD:

```text
Developer
    |
Git Push
    |
CI/CD Pipeline
    |
Build
    |
Test
    |
Deploy
    |
Production
```

Benefits:

- Faster delivery
- Automated testing
- Reduced human errors
- Reliable deployments

---

# Continuous Integration (CI)

Continuous Integration means developers frequently merge code into a shared repository.

Each code change automatically triggers:

- Build
- Test
- Validation

---

## CI Workflow

```text
Developer
    |
Git Push
    |
Source Repository
    |
Build
    |
Unit Tests
    |
Success/Failure
```

---

## Goals of CI

- Detect bugs early
- Improve code quality
- Reduce integration problems
- Automate testing

---

# Continuous Delivery (CD)

Continuous Delivery automatically prepares applications for deployment.

Workflow:

```text
Build
   |
Test
   |
Package
   |
Ready For Production
```

Human approval may still be required.

---

# Continuous Deployment

Continuous Deployment goes one step further.

Every successful change is automatically deployed to production.

```text
Build
   |
Test
   |
Deploy
   |
Production
```

No manual approval required.

---

# Continuous Delivery vs Continuous Deployment

| Feature | Continuous Delivery | Continuous Deployment |
|-----------|-------------------|-----------------------|
| Manual Approval | Required | Not Required |
| Production Deployment | Manual Trigger | Automatic |
| Risk | Lower | Higher |

---

# CI/CD Pipeline Stages

Most pipelines contain:

```text
Source
   |
Build
   |
Test
   |
Security Scan
   |
Package
   |
Deploy
   |
Monitor
```

---

# Stage 1: Source

Developer pushes code.

Example:

```bash
git push origin main
```

Triggers pipeline.

---

# Stage 2: Build

Source code is compiled.

Examples:

Java:

```bash
mvn package
```

NodeJS:

```bash
npm build
```

Python:

```bash
python setup.py build
```

---

# Stage 3: Test

Automated tests run.

Types:

- Unit Tests
- Integration Tests
- Functional Tests

Example:

```bash
pytest
```

---

# Stage 4: Security Scan

Checks for vulnerabilities.

Tools:

- SonarQube
- Snyk
- Trivy

Example:

```bash
trivy image myapp
```

---

# Stage 5: Package

Application packaged into an artifact.

Examples:

```text
JAR
WAR
Docker Image
ZIP
```

Docker Example:

```bash
docker build -t myapp .
```

---

# Stage 6: Deploy

Deploy application.

Examples:

```bash
kubectl apply -f deployment.yaml
```

or

```bash
aws ecs update-service
```

---

# Stage 7: Monitoring

Monitor deployed application.

Tools:

- Prometheus
- Grafana
- CloudWatch
- ELK Stack

---

# Complete Pipeline Architecture

```text
Developer
     |
     v
GitHub
     |
     v
CI/CD Tool
     |
     +---- Build
     |
     +---- Test
     |
     +---- Security Scan
     |
     +---- Docker Build
     |
     +---- Push to Registry
     |
     +---- Deploy
     |
     v
Production
```

---

# Popular CI/CD Tools

## Jenkins

Most popular CI/CD tool.

Features:

- Open Source
- Plugin ecosystem
- Highly customizable

---

## GitHub Actions

Integrated with GitHub.

Example:

```text
GitHub Repository
        |
        v
GitHub Actions
        |
        v
Deployment
```

---

## GitLab CI/CD

Built into GitLab.

---

## Azure DevOps

Popular in Microsoft environments.

---

# Real-World Example

Developer pushes code:

```bash
git push origin main
```

Pipeline automatically:

```text
Build
   |
Test
   |
Docker Build
   |
Push to ECR
   |
Deploy to ECS
   |
Health Check
```

If successful:

```text
Production Updated
```

---

# CI/CD Pipeline Failure

Possible reasons:

- Build errors
- Failed tests
- Security vulnerabilities
- Deployment issues

Pipeline stops immediately.

---

# Benefits of CI/CD

### Faster Releases

Deployment becomes automated.

---

### Better Quality

Automated testing catches issues.

---

### Reduced Human Errors

Less manual intervention.

---

### Faster Feedback

Developers receive immediate results.

---

### Consistent Deployments

Same deployment process every time.

---

# Interview Question

## What is CI/CD?

### Answer

CI/CD is a software development practice that automates the process of integrating, testing, building, and deploying applications.

Continuous Integration focuses on automatically validating code changes, while Continuous Delivery and Continuous Deployment focus on releasing software efficiently and reliably.

---

# Interview Question

## What Are the Stages of a CI/CD Pipeline?

### Answer

A typical CI/CD pipeline consists of:

1. Source
2. Build
3. Test
4. Security Scan
5. Package
6. Deploy
7. Monitoring

These stages ensure that code changes are validated and deployed safely.

---

# Interview Question

## Explain a CI/CD Pipeline You Worked On

### Answer

In a typical pipeline, developers push code to GitHub. GitHub Actions triggers the pipeline, which builds the application, runs tests, performs security scans, builds a Docker image, pushes it to AWS ECR, and deploys it to ECS or Kubernetes. After deployment, monitoring tools verify application health.

---

# Best Practices

- Automate everything possible
- Fail fast on errors
- Include security scans
- Use infrastructure as code
- Monitor deployments
- Maintain rollback strategy

---

# Summary

CI/CD automates:

Code → Build → Test → Package → Deploy → Monitor

Benefits:

- Faster delivery
- Better quality
- Reduced errors
- Reliable deployments

CI/CD is a fundamental DevOps practice and one of the most important topics in DevOps interviews.
