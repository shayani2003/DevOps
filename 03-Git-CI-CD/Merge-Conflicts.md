# Merge Conflicts

## What is a Merge Conflict?

A merge conflict occurs when Git cannot automatically combine changes from two branches.

This usually happens when:

- Two developers modify the same file.
- Two developers modify the same line.
- A file is deleted in one branch and modified in another.

Git becomes confused and asks the developer to resolve the conflict manually.

---

# Why Merge Conflicts Occur

Suppose two developers are working on the same file.

Initial Code:

```python
print("Hello World")
```

---

Developer A changes:

```python
print("Welcome User")
```

---

Developer B changes:

```python
print("Hello DevOps")
```

---

When Git tries to merge:

```text
Which version should I keep?
```

Git cannot decide automatically.

Result:

```text
Merge Conflict
```

---

# Conflict Example

Main Branch:

```python
print("Hello World")
```

Feature Branch:

```python
print("Welcome User")
```

After merge:

```text
<<<<<<< HEAD
print("Hello World")
=======
print("Welcome User")
>>>>>>> feature-login
```

This is a conflict marker.

---

# Understanding Conflict Markers

```text
<<<<<<< HEAD
```

Current branch code.

---

```text
=======
```

Separator.

---

```text
>>>>>>> feature-login
```

Incoming branch code.

---

Diagram:

```text
Current Branch
      |
      v
<<<<<<< HEAD

Conflict Area

=======

Incoming Branch

>>>>>>> feature-login
```

---

# How to Resolve Merge Conflicts

Step 1:

Merge branches.

```bash
git merge feature-login
```

Git reports:

```text
CONFLICT (content)
Automatic merge failed.
```

---

Step 2:

Open conflicting file.

Example:

```python
<<<<<<< HEAD
print("Hello World")
=======
print("Welcome User")
>>>>>>> feature-login
```

---

Step 3:

Choose correct code.

Example:

```python
print("Welcome User")
```

Remove conflict markers.

---

Step 4:

Stage file.

```bash
git add app.py
```

---

Step 5:

Commit resolution.

```bash
git commit -m "Resolved merge conflict"
```

Conflict resolved.

---

# Merge Conflict Resolution Workflow

```text
Merge Branches
      |
      v
Conflict Detected
      |
      v
Open File
      |
      v
Choose Correct Code
      |
      v
git add
      |
      v
git commit
```

---

# Real World Example

Developer A:

```text
feature-login
```

Developer B:

```text
feature-payment
```

Both modify:

```text
config.yaml
```

Developer A merges first.

Developer B merges later.

Result:

```text
Merge Conflict
```

Developer B must manually resolve it.

---

# Preventing Merge Conflicts

## Pull Frequently

Before starting work:

```bash
git pull origin develop
```

Keeps branch updated.

---

## Create Small Pull Requests

Smaller changes mean fewer conflicts.

---

## Communicate with Team

Avoid editing the same files simultaneously.

---

## Merge Regularly

Do not keep feature branches open for weeks.

---

## Use Feature Branches

Example:

```text
feature-login
feature-payment
feature-profile
```

Each feature remains isolated.

---

# Merge vs Rebase

## Merge

```bash
git merge feature-login
```

Creates a merge commit.

Diagram:

```text
main
 |
 +---- feature
         |
         v
      merge commit
```

---

## Rebase

```bash
git rebase main
```

Moves commits onto latest main branch.

Diagram:

```text
main
 |
 +---- feature
         |
         v
   linear history
```

---

# Resolving Conflicts During Rebase

Start rebase:

```bash
git rebase main
```

Conflict occurs.

Fix file.

Then:

```bash
git add .
```

Continue:

```bash
git rebase --continue
```

Abort:

```bash
git rebase --abort
```

---

# Useful Commands

Check status:

```bash
git status
```

Merge:

```bash
git merge branch-name
```

Abort merge:

```bash
git merge --abort
```

Continue rebase:

```bash
git rebase --continue
```

Abort rebase:

```bash
git rebase --abort
```

---

# Interview Question

## How Do You Resolve Merge Conflicts?

### Answer

When a merge conflict occurs, I first identify the conflicting files using git status. I then open the files and review the conflict markers. After deciding which changes should be kept, I remove the conflict markers, save the file, stage it using git add, and complete the merge with git commit. Finally, I test the application to ensure everything works correctly.

---

# Interview Question

## How Can You Avoid Merge Conflicts?

### Answer

I reduce merge conflicts by:

- Pulling latest changes frequently
- Using feature branches
- Keeping pull requests small
- Communicating with team members
- Merging changes regularly

---

# Best Practices

- Pull before starting work
- Commit frequently
- Keep branches short-lived
- Review code carefully
- Test after resolving conflicts
- Avoid large pull requests

---

# Summary

Merge conflicts occur when Git cannot automatically combine changes.

Resolution Steps:

1. Identify conflict.
2. Open conflicting file.
3. Choose correct code.
4. Remove conflict markers.
5. git add
6. git commit

Proper branching and regular synchronization significantly reduce merge conflicts.
