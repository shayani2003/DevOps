# Cherry Picking

## What is Cherry Picking?

Cherry-picking is a Git operation that allows you to copy a specific commit from one branch and apply it to another branch.

Instead of merging the entire branch, only selected commits are transferred.

---

# Why Use Cherry Picking?

Suppose a branch contains multiple commits:

```text
feature-login

A --> B --> C --> D
```

But only commit C is needed in another branch.

Instead of merging the entire branch:

```text
A --> B --> C --> D
```

We can cherry-pick only:

```text
C
```

This avoids bringing unwanted changes.

---

# How Cherry Pick Works

Source Branch:

```text
feature-login

A --> B --> C --> D
```

Target Branch:

```text
main

X --> Y
```

After Cherry Pick:

```text
main

X --> Y --> C
```

Only commit C is copied.

---

# Syntax

```bash
git cherry-pick <commit-id>
```

Example:

```bash
git cherry-pick a1b2c3
```

Git copies that commit to the current branch.

---

# Step-by-Step Example

## Step 1

View commit history:

```bash
git log --oneline
```

Output:

```text
a1b2c3 Added login page

d4e5f6 Added payment page

g7h8i9 Fixed bug
```

---

## Step 2

Switch to target branch:

```bash
git checkout main
```

---

## Step 3

Cherry-pick commit:

```bash
git cherry-pick a1b2c3
```

---

## Result

The login feature commit is now available in main.

---

# Real-World Scenario

Suppose:

Production Branch:

```text
main
```

Feature Branch:

```text
feature-payment
```

The feature branch contains:

```text
Commit 1 -> Payment UI
Commit 2 -> Payment API
Commit 3 -> Critical Security Fix
```

The security fix is urgently needed in production.

Instead of merging the entire feature branch:

```text
Commit 1
Commit 2
Commit 3
```

We cherry-pick:

```text
Commit 3
```

Only the security fix reaches production.

---

# Cherry Picking Multiple Commits

Single Commit:

```bash
git cherry-pick commit-id
```

---

Multiple Commits:

```bash
git cherry-pick commit1 commit2 commit3
```

Example:

```bash
git cherry-pick a1b2c3 d4e5f6
```

---

# Cherry Pick Range

Example:

```bash
git cherry-pick A^..D
```

Commits copied:

```text
A
B
C
D
```

---

# Cherry Pick Workflow

```text
Feature Branch
      |
      |
      v
Select Commit
      |
      v
Cherry Pick
      |
      v
Target Branch
```

---

# What Happens Internally?

Git:

1. Finds selected commit.
2. Copies changes.
3. Creates a new commit.
4. Applies it to current branch.

Important:

Commit hash changes.

Original:

```text
a1b2c3
```

New:

```text
z9y8x7
```

Because it becomes a new commit.

---

# Cherry Pick vs Merge

## Merge

```bash
git merge feature-login
```

Result:

Entire branch merged.

Diagram:

```text
feature-login

A --> B --> C

       |
       v

main
```

---

## Cherry Pick

```bash
git cherry-pick C
```

Result:

Only commit C copied.

Diagram:

```text
feature-login

A --> B --> C

         |
         v

main --> C
```

---

# Cherry Pick vs Rebase

## Cherry Pick

Copies specific commits.

---

## Rebase

Moves entire commit history.

---

# Handling Cherry Pick Conflicts

Sometimes conflicts occur.

Git displays:

```text
CONFLICT (content)
```

---

## Resolve Conflict

Open file.

Fix conflict.

Stage file:

```bash
git add .
```

Continue:

```bash
git cherry-pick --continue
```

---

## Abort Cherry Pick

```bash
git cherry-pick --abort
```

Returns repository to previous state.

---

# Useful Commands

Cherry-pick commit:

```bash
git cherry-pick commit-id
```

Continue:

```bash
git cherry-pick --continue
```

Abort:

```bash
git cherry-pick --abort
```

View commits:

```bash
git log --oneline
```

---

# Interview Question

## What is Git Cherry Pick?

### Answer

Git cherry-pick is a command that allows us to copy a specific commit from one branch and apply it to another branch. It is useful when we need only selected changes instead of merging an entire branch.

---

# Interview Question

## When Would You Use Cherry Picking?

### Answer

I use cherry-picking when a specific bug fix, security patch, or feature commit needs to be moved to another branch without merging all changes from the source branch.

For example, if a critical production fix exists in a development branch, I can cherry-pick that commit directly into the production branch.

---

# Advantages

- Copies specific commits
- Avoids unnecessary merges
- Useful for hotfixes
- Faster than merging entire branches

---

# Disadvantages

- Can create duplicate commits
- May cause conflicts
- Excessive use can complicate history

---

# Best Practices

- Use for isolated fixes
- Avoid overusing it
- Verify commit before cherry-picking
- Test after applying changes
- Prefer merge when entire branch is needed

---

# Summary

Cherry-picking allows developers to copy selected commits from one branch to another.

Most common use cases:

- Production hotfixes
- Security patches
- Bug fixes
- Selective feature migration

Command:

```bash
git cherry-pick <commit-id>
```

It is a powerful tool when only specific changes are required.
