# Pipeline Troubleshooting

## Introduction

A CI/CD pipeline can fail due to:

- Build errors
- Test failures
- Configuration issues
- Permission problems
- Infrastructure issues
- Deployment failures

A DevOps Engineer should follow a structured troubleshooting approach.

---

# Troubleshooting Workflow

```text
Pipeline Failed
      |
      v
Check Logs
      |
      v
Identify Failed Stage
      |
      v
Find Root Cause
      |
      v
Fix Issue
      |
      v
Re-run Pipeline
```

---

# Step 1: Check Pipeline Logs

First thing to check:

```text
Pipeline Logs
```

Example:

```text
Build Failed
```

or

```text
Permission Denied
```

Logs usually reveal the root cause.

---

# Step 2: Identify Failed Stage

Example Pipeline:

```text
Source
  |
Build
  |
Test
  |
Security Scan
  |
Docker Build
  |
Deploy
```

Find exactly where the failure occurred.

Example:

```text
Build ✓
Test ✓
Docker Build ✗
Deploy Not Executed
```

---

# Common Pipeline Issues

# Issue 1: Pipeline Not Triggering

## Symptoms

Developer pushes code.

```bash
git push origin main
```

Pipeline does not start.

---

## Possible Causes

### Webhook Problem

GitHub webhook not configured.

Example:

```text
GitHub
   |
Webhook Missing
   |
Pipeline Not Triggered
```

---

### Wrong Branch Filter

Example:

Pipeline configured for:

```text
main
```

Developer pushes to:

```text
develop
```

Pipeline won't run.

---

### YAML Syntax Error

GitHub Actions example:

```yaml
jobs
 build:
```

Missing colon causes failure.

---

### Runner Offline

Example:

```text
GitHub Actions Runner Offline
```

No worker available.

---

## Troubleshooting Steps

1. Check webhook configuration.
2. Verify branch settings.
3. Validate pipeline YAML.
4. Check runner status.
5. Verify permissions.

---

# Issue 2: Build Failure

## Example

Java Build:

```bash
mvn package
```

Error:

```text
Compilation Failed
```

---

## Causes

- Syntax error
- Missing dependency
- Wrong Java version

---

## Solution

Check:

```bash
mvn clean package
```

locally first.

---

# Issue 3: Test Failure

Example:

```text
10 Tests Passed

1 Test Failed
```

Pipeline stops.

---

## Troubleshooting

Review:

```text
Test Reports
```

Check:

- Assertions
- Database connectivity
- API responses

---

# Issue 4: Docker Build Failure

Example:

```bash
docker build -t app .
```

Error:

```text
requirements.txt not found
```

---

## Causes

- Incorrect path
- Missing file
- Wrong Dockerfile

---

## Troubleshooting

Verify:

```bash
ls
```

Check:

```dockerfile
COPY requirements.txt .
```

---

# Issue 5: Deployment Failure

Example:

```text
Deployment Failed
```

---

## Causes

- Kubernetes issue
- ECS issue
- Missing permissions
- Network problem

---

## Kubernetes Example

Check:

```bash
kubectl get pods
```

Inspect:

```bash
kubectl describe pod pod-name
```

View logs:

```bash
kubectl logs pod-name
```

---

# Issue 6: Permission Denied

Example:

```text
Access Denied
```

---

## Common Causes

Missing IAM permissions.

Example:

```text
Pipeline
   |
Push Docker Image
   |
ECR Permission Missing
```

---

## Solution

Verify IAM Role.

Required permissions:

```text
ECR Access
S3 Access
Deployment Permissions
```

---

# Pipeline Performance Issues

# Pipeline Is Slow

Common interview question.

---

## Causes

### Too Many Sequential Jobs

Bad:

```text
Job1
  |
Job2
  |
Job3
  |
Job4
```

---

Better:

```text
Job1 -----|
          |
Job2 -----|--> Parallel
          |
Job3 -----|
```

---

### Reinstalling Dependencies

Every build:

```bash
npm install
```

or

```bash
pip install
```

takes time.

---

## Solution

Use caching.

Example:

```text
Cache Dependencies
      |
 Faster Pipeline
```

---

### Large Docker Images

Example:

```text
2 GB Image
```

Builds slowly.

---

## Solution

- Multi-stage builds
- Smaller base images
- Layer caching

---

### Too Many Tests

Example:

```text
5000 Tests
```

Pipeline becomes slow.

---

## Solution

Run tests in parallel.

---

# Pipeline Optimization Techniques

## Parallel Execution

Before:

```text
Build
  |
Test
  |
Security Scan
```

After:

```text
          Build
             |
      +------+------+
      |             |
     Test      Security Scan
```

---

## Dependency Caching

Example:

```text
First Build -> 10 minutes

Second Build -> 3 minutes
```

Because dependencies are cached.

---

## Docker Layer Caching

Good Dockerfile:

```dockerfile
COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .
```

Improves build speed.

---

## Incremental Builds

Build only changed modules.

---

# Real-World Scenario

Question:

Pipeline suddenly became slow.

Investigation:

```text
Pipeline Duration

Before -> 8 min

After -> 25 min
```

Root Cause:

```text
Dependency cache disabled
```

Solution:

```text
Re-enable cache
```

Pipeline returned to 8 minutes.

---

# Interview Question

## Pipeline Is Not Triggering. How Will You Troubleshoot?

### Answer

I first verify whether the pipeline trigger event occurred correctly. Then I check webhook configuration, branch filters, YAML syntax, runner availability, and permissions. I review pipeline logs and repository settings to identify why the trigger was not activated.

---

# Interview Question

## Pipeline Has Become Slow. How Will You Optimize It?

### Answer

I would analyze pipeline execution time and identify bottlenecks. Common optimizations include enabling dependency caching, using Docker layer caching, running jobs in parallel, reducing unnecessary tests, and optimizing Docker images. These improvements significantly reduce pipeline execution time.

---

# Best Practices

- Monitor pipeline duration
- Use caching
- Run jobs in parallel
- Keep Docker images small
- Validate YAML files
- Implement proper logging
- Use automated alerts

---

# Important Commands

GitHub Actions:

```text
Actions Tab
Workflow Logs
Runner Status
```

Kubernetes:

```bash
kubectl get pods

kubectl logs pod-name

kubectl describe pod pod-name
```

Docker:

```bash
docker logs container-id

docker inspect container-id
```

---

# Summary

Pipeline troubleshooting involves:

1. Checking logs
2. Finding failed stage
3. Identifying root cause
4. Fixing issue
5. Re-running pipeline

Most common problems:

- Pipeline not triggering
- Build failures
- Test failures
- Docker build failures
- Deployment failures
- Slow pipelines

A structured troubleshooting process is essential for DevOps engineers.
