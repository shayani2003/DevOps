# Branching Strategy

## What is a Branching Strategy?

A branching strategy is a workflow that defines how developers create, manage, and merge branches in a Git repository.

It helps teams:

- Develop features independently
- Avoid conflicts
- Maintain code quality
- Enable controlled releases

---

# Why Do We Need Branches?

Without branches:

- Developers work on the same code simultaneously
- Frequent conflicts occur
- Production code becomes unstable

With branches:

- Features are developed independently
- Production remains stable
- Easier testing and reviews

---

# Common Branching Strategies

1. Git Flow
2. GitHub Flow
3. GitLab Flow
4. Trunk-Based Development

---

# Git Flow (Most Common Interview Topic)

## Structure

```text
main
 |
 +---- develop
         |
         +---- feature/login
         |
         +---- feature/payment

main
 |
 +---- hotfix/security-fix
```

---

## Branch Types

### Main Branch

```text
main
```

Purpose:

- Production-ready code
- Stable releases

---

### Develop Branch

```text
develop
```

Purpose:

- Integration branch
- Combines feature branches

---

### Feature Branch

Example:

```text
feature/login
feature/payment
feature/profile
```

Purpose:

- New feature development

---

### Release Branch

Example:

```text
release/v1.0
```

Purpose:

- Final testing before production

---

### Hotfix Branch

Example:

```text
hotfix/security-patch
```

Purpose:

- Emergency production fixes

---

# Git Flow Lifecycle

```text
main
 |
 +------ develop
           |
           +------ feature/login
           |
           +------ feature/payment
```

After development:

```text
feature/login
      |
      v
develop
```

After testing:

```text
develop
     |
     v
release/v1.0
```

Production deployment:

```text
release/v1.0
      |
      v
main
```

---

# GitHub Flow

Simpler than Git Flow.

Structure:

```text
main
 |
 +---- feature-branch
```

Workflow:

1. Create branch
2. Make changes
3. Open Pull Request
4. Code Review
5. Merge to main

---

## Diagram

```text
main
 |
 +---- feature-login
          |
          v
      Pull Request
          |
          v
         main
```

---

# Trunk-Based Development

All developers work on a single branch.

Structure:

```text
main
 |
 +---- Small Short-Lived Branches
```

Features merged frequently.

---

## Advantages

- Faster delivery
- Smaller merges
- Continuous Integration friendly

---

## Disadvantages

- Requires strong testing
- Requires automated pipelines

---

# Real-World Example

Suppose three developers are working.

Developer A:

```text
feature-login
```

Developer B:

```text
feature-payment
```

Developer C:

```text
feature-profile
```

Each works independently.

After testing:

```text
feature branch
      |
      v
develop
      |
      v
main
```

---

# Branch Naming Best Practices

Feature:

```text
feature/login
feature/payment
```

Bug Fix:

```text
bugfix/login-error
```

Hotfix:

```text
hotfix/security-patch
```

Release:

```text
release/v1.0
```

---

# Important Git Commands

Create Branch

```bash
git checkout -b feature/login
```

Switch Branch

```bash
git checkout develop
```

View Branches

```bash
git branch
```

Delete Branch

```bash
git branch -d feature/login
```

Push Branch

```bash
git push origin feature/login
```

---

# Pull Request Workflow

```text
Feature Branch
      |
      v
Pull Request
      |
Code Review
      |
Approval
      |
Merge
      |
Deploy
```

---

# Interview Question

## What Branching Strategy Are You Using?

### Answer

In most projects, I use Git Flow.

The workflow consists of:

- main branch for production
- develop branch for integration
- feature branches for development
- release branches for testing
- hotfix branches for emergency fixes

This approach keeps production stable while allowing multiple developers to work simultaneously.

---

# When to Use GitHub Flow?

Use GitHub Flow when:

- Small teams
- Fast deployments
- Continuous delivery

Structure:

```text
main
 |
 +---- feature branch
```

---

# Comparison

| Feature | Git Flow | GitHub Flow | Trunk-Based |
|----------|-----------|-------------|-------------|
| Complexity | High | Low | Medium |
| Releases | Scheduled | Continuous | Continuous |
| Best For | Large Teams | Small Teams | DevOps Teams |

---

# Best Practices

- Protect main branch
- Use Pull Requests
- Use meaningful branch names
- Delete merged branches
- Perform code reviews
- Keep branches short-lived

---

# Summary

Git branching strategies help teams manage code efficiently.

Most common strategies:

- Git Flow
- GitHub Flow
- Trunk-Based Development

For DevOps interviews, Git Flow is the most frequently discussed because it clearly demonstrates feature development, testing, releases, and hotfix workflows.
