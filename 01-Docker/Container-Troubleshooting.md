# Container Troubleshooting

## Introduction

In production environments, containers may fail due to:

- Application errors
- Configuration issues
- Resource exhaustion
- Networking problems
- Image issues

A DevOps Engineer should follow a systematic troubleshooting process.

---

# Troubleshooting Flow

```text
Container Crashed
       |
       v
Check Status
       |
       v
Check Logs
       |
       v
Check Exit Code
       |
       v
Check Resources
       |
       v
Check Configuration
       |
       v
Find Root Cause
       |
       v
Fix & Verify
```

---

# Step 1: Check Container Status

List running containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

Example Output:

```text
Exited (1) 2 minutes ago
```

This means the container crashed.

---

# Step 2: Check Container Logs

Most common troubleshooting command:

```bash
docker logs <container_id>
```

Example:

```bash
docker logs my-container
```

Possible Output:

```text
ModuleNotFoundError: No module named flask
```

Root Cause:

Dependency missing.

---

# Step 3: Check Exit Code

Inspect container:

```bash
docker inspect <container_id>
```

Example:

```text
ExitCode: 137
```

---

## Common Exit Codes

| Exit Code | Meaning |
|------------|----------|
| 0 | Successful Exit |
| 1 | Application Error |
| 125 | Docker Run Failure |
| 126 | Command Cannot Execute |
| 127 | Command Not Found |
| 137 | Out Of Memory (OOMKilled) |
| 143 | Graceful Shutdown |

---

# Exit Code 137 (Very Common Interview Question)

Meaning:

Container exceeded memory limit.

Diagram:

```text
Application
     |
Consumes Memory
     |
Memory Limit Reached
     |
OOM Killer
     |
Container Stopped
```

Check:

```bash
docker stats
```

Solution:

- Increase memory
- Fix memory leak
- Optimize application

---

# Step 4: Verify Environment Variables

Check environment variables:

```bash
docker exec -it <container_id> env
```

Common Issues:

- Missing database URL
- Missing API keys
- Incorrect credentials

Example:

```text
DATABASE_URL missing
```

Application cannot connect to database.

---

# Step 5: Verify Port Mapping

Check:

```bash
docker ps
```

Example:

```text
0.0.0.0:8080->80/tcp
```

Meaning:

```text
Host Port       Container Port
8080     --->         80
```

Common Mistake:

```bash
docker run myapp
```

Correct:

```bash
docker run -p 8080:80 myapp
```

---

# Step 6: Verify Application Inside Container

Open shell:

```bash
docker exec -it <container_id> sh
```

or

```bash
docker exec -it <container_id> bash
```

Check:

```bash
ls
cat config.yaml
```

Verify:

- Files exist
- Configuration correct

---

# Step 7: Verify Image Version

Check image:

```bash
docker images
```

Possible Issue:

Wrong image version deployed.

Example:

```text
Application expects v2
Running v1
```

Solution:

Deploy correct image.

---

# Step 8: Check Resource Usage

Check:

```bash
docker stats
```

Example:

```text
CPU Usage = 95%
Memory Usage = 98%
```

Possible Causes:

- Infinite loops
- Memory leak
- High traffic

---

# Step 9: Verify Network Connectivity

List networks:

```bash
docker network ls
```

Inspect network:

```bash
docker network inspect bridge
```

Test connectivity:

```bash
ping database-container
```

Possible Issue:

Application container cannot reach database container.

---

# Step 10: Check Mounted Volumes

Inspect container:

```bash
docker inspect <container_id>
```

Possible Issue:

```text
Configuration file not mounted
```

Result:

Application fails to start.

---

# Common Production Scenarios

## Scenario 1

Question:

Container continuously restarting.

Check:

```bash
docker logs <container_id>
```

Possible Cause:

Application startup failure.

---

## Scenario 2

Question:

Container exits immediately.

Cause:

Main process completed.

Example:

```dockerfile
CMD ["echo","Hello"]
```

Container exits after printing message.

---

## Scenario 3

Question:

Container is running but application inaccessible.

Check:

- Port mapping
- Firewall
- Security groups
- Application listening port

---

## Scenario 4

Question:

Container suddenly stopped.

Check:

```bash
docker inspect <container_id>
```

Look for:

```text
ExitCode: 137
```

Likely memory issue.

---

# Real Interview Question

## Container Continuously Crashing. How Will You Troubleshoot?

### Answer

I follow a structured approach:

1. Check container status using docker ps -a.
2. Review logs using docker logs.
3. Inspect exit codes.
4. Verify environment variables.
5. Check port mappings.
6. Check CPU and memory utilization.
7. Verify networking and volume mounts.
8. Confirm correct image version.
9. Identify root cause.
10. Fix and redeploy.

This helps isolate the issue quickly in production environments.

---

# Best Practices

- Implement health checks
- Monitor resource usage
- Use centralized logging
- Set resource limits
- Use proper image versioning
- Configure alerts

---

# Important Commands Cheat Sheet

```bash
docker ps

docker ps -a

docker logs <container_id>

docker inspect <container_id>

docker exec -it <container_id> bash

docker stats

docker network ls

docker volume ls
```

---

# Summary

Container troubleshooting usually involves:

- Logs
- Exit codes
- Resources
- Networking
- Environment variables
- Volumes

The most important command is:

```bash
docker logs <container_id>
```

because logs often reveal the root cause immediately.
