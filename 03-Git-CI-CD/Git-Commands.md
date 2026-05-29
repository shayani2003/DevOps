# Git Commands

## What is Git?

Git is a distributed version control system used to track changes in source code and enable collaboration among developers.

It helps teams:

- Track code changes
- Collaborate efficiently
- Manage versions
- Roll back changes when needed

---

# Git Workflow

```text
Working Directory
       |
       v
Staging Area
       |
       v
Local Repository
       |
       v
Remote Repository (GitHub/GitLab)
```

---

# Common Git Commands

## Check Git Version

```bash
git --version
```

Purpose:

Verify Git installation.

---

## Configure Git

Set username:

```bash
git config --global user.name "Shayani Bhowmik"
```

Set email:

```bash
git config --global user.email "example@gmail.com"
```

Check configuration:

```bash
git config --list
```

---

# Initialize Repository

```bash
git init
```

Creates:

```text
.git/
```

Git starts tracking the project.

---

# Clone Repository

```bash
git clone <repository-url>
```

Example:

```bash
git clone https://github.com/user/project.git
```

Downloads repository locally.

---

# Check Status

```bash
git status
```

Most frequently used command.

Shows:

- Modified files
- Untracked files
- Staged files

---

# Add Files

Add specific file:

```bash
git add app.py
```

Add all files:

```bash
git add .
```

Moves files to staging area.

---

# Commit Changes

```bash
git commit -m "Added login feature"
```

Creates a snapshot of changes.

---

# View Commit History

```bash
git log
```

Compact view:

```bash
git log --oneline
```

Example:

```text
a1b2c3 Added login page
d4e5f6 Fixed API bug
```

---

# View Differences

Before commit:

```bash
git diff
```

Shows modified lines.

---

# Branch Commands

Create branch:

```bash
git branch feature-login
```

Switch branch:

```bash
git checkout feature-login
```

Create and switch:

```bash
git checkout -b feature-login
```

List branches:

```bash
git branch
```

Delete branch:

```bash
git branch -d feature-login
```

---

# Push Changes

```bash
git push origin main
```

Uploads commits to remote repository.

---

# Pull Changes

```bash
git pull origin main
```

Downloads latest changes.

---

# Fetch Changes

```bash
git fetch
```

Downloads changes without merging.

Difference:

```text
git fetch -> Download only

git pull -> Download + Merge
```

---

# Merge Branches

Switch to target branch:

```bash
git checkout develop
```

Merge:

```bash
git merge feature-login
```

Combines changes.

---

# Rebase

```bash
git rebase main
```

Moves commits to a new base.

---

## Merge vs Rebase

Merge:

```text
Main
  |
  +---- Feature
           |
           v
         Merge Commit
```

Rebase:

```text
Main
  |
  +---- Feature
           |
           v
      Linear History
```

---

# Stash Changes

Save temporary changes:

```bash
git stash
```

View stashes:

```bash
git stash list
```

Restore stash:

```bash
git stash pop
```

---

# Reset Changes

Unstage file:

```bash
git reset file.txt
```

Reset commit:

```bash
git reset --soft HEAD~1
```

Remove commit completely:

```bash
git reset --hard HEAD~1
```

---

# Restore File

Discard changes:

```bash
git restore file.txt
```

Restore staged file:

```bash
git restore --staged file.txt
```

---

# Tagging

Create tag:

```bash
git tag v1.0
```

Push tags:

```bash
git push --tags
```

Used for releases.

---

# Remote Repository Commands

View remotes:

```bash
git remote -v
```

Add remote:

```bash
git remote add origin <url>
```

Change remote:

```bash
git remote set-url origin <url>
```

---

# Real Workflow Example

Step 1

Clone repository:

```bash
git clone <repo-url>
```

Step 2

Create branch:

```bash
git checkout -b feature-login
```

Step 3

Modify files.

Step 4

Check status:

```bash
git status
```

Step 5

Add changes:

```bash
git add .
```

Step 6

Commit:

```bash
git commit -m "Added login functionality"
```

Step 7

Push:

```bash
git push origin feature-login
```

Step 8

Create Pull Request.

---

# Most Important Commands for DevOps Engineers

```bash
git clone

git status

git add .

git commit -m ""

git pull

git push

git checkout

git branch

git merge

git rebase

git log

git stash
```

---

# Interview Questions

## What Git Commands Do You Use Daily?

### Answer

The commands I use most frequently are:

```bash
git clone
git status
git add .
git commit
git pull
git push
git checkout
git branch
git merge
git log
```

These commands help me manage code changes, collaborate with team members, and maintain version control.

---

## Difference Between git pull and git fetch

### git fetch

Downloads changes only.

### git pull

Downloads and merges changes.

---

## Difference Between git merge and git rebase

### Merge

Creates a merge commit.

### Rebase

Creates a cleaner linear history.

---

# Best Practices

- Commit frequently
- Write meaningful commit messages
- Use feature branches
- Review code before merging
- Pull latest changes before pushing
- Avoid force push on main branch

---

# Summary

Git commands allow developers to:

- Track changes
- Collaborate efficiently
- Manage branches
- Resolve conflicts
- Maintain project history

The most commonly used commands are:

```bash
git status
git add
git commit
git pull
git push
git checkout
git merge
```
