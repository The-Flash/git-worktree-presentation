---

title: Git Worktrees
sub_title: Multiple Branches, One Repository
author: Augustine Smith
---


# Git Worktrees

## Multiple Branches, One Repository

### Stop switching branches. Stop cloning repositories.

<!-- end_slide -->

# A Familiar Situation

You're working on:

```text
feature/payment-redesign
```

Then suddenly:

* A production bug is reported
* A teammate asks for a PR review
* QA finds an issue in the release branch

You need another branch immediately.

<!-- end_slide -->

# The Question

What do you do with your current work?

Most developers use one of three approaches:

1. Stash
2. Switch branches
3. Clone the repository

Let's examine each.

<!-- end_slide -->

# Solution #1: Stash

Save your work temporarily.

```bash
git stash

git checkout main

# do some work

git checkout feature/payment-redesign

git stash pop
```

<!-- end_slide -->

# Problems With Stashing

Stashing is useful, but:

* Easy to forget stashes exist
* Merge conflicts when applying
* Stashes accumulate over time
* Interrupts your workflow
* Only one branch can be active

Most importantly:

You still cannot work on multiple branches simultaneously.

<!-- end_slide -->


# Solution #2: Multiple Clones

Create several copies of the repository.

```text
projects/

├── myapp-main
├── myapp-feature
├── myapp-release
└── myapp-hotfix
```

<!-- end_slide -->

# Problems With Multiple Clones

* Wasted disk space
* Multiple `.git` directories
* Multiple fetches and pulls
* More maintenance
* Easy to lose track of which clone is current

<!-- end_slide -->

# Enter Git Worktrees

Git Worktrees provide:

* Multiple working directories
* Multiple active branches
* One Git repository

No stashing.

No repeated cloning.

No constant branch switching.

<!-- end_slide -->

# What Is A Worktree?

A worktree is:

> An additional working directory attached to the same Git repository.

Each worktree can have its own checked-out branch.

<!-- end_slide -->

# Traditional Repository

```text
myapp/

├── .git
└── working directory
```

Only one branch can be checked out.

<!-- end_slide -->

# Repository With Worktrees

```text
myapp/
myapp-payment/
myapp-hotfix/
myapp-release/
```

All connected to:

```text
myapp/.git
```

One repository.

Multiple working directories.

<!-- end_slide -->

# Real Example

Branches:

```text
main
feature/payment
bugfix/login
release/v2
```

Worktrees:

```text
myapp/
myapp-payment/
myapp-login-fix/
myapp-release/
```

All available at the same time.

<!-- end_slide -->

# Why Developers Love Worktrees

You can:

* Work on a feature
* Review a PR
* Fix production bugs
* Compare implementations

Without touching your current workspace.

<!-- end_slide -->

# Creating A Worktree

Syntax:

```bash
git worktree add <path> <branch>
```

Example:

```bash
git worktree add ../myapp-payment feature/payment
```

<!-- end_slide -->

# What Gets Created?

Before:

```text
myapp/
```

After:

```text
myapp/
myapp-payment/
```

The new directory immediately contains:

```text
feature/payment
```

checked out.

<!-- end_slide -->

# Creating A Branch And Worktree Together

```bash
git worktree add \
  -b feature/new-api \
  ../new-api
```

Creates:

* A new branch
* A new worktree

In one command.

<!-- end_slide -->

# Listing Worktrees

```bash
git worktree list
```

Example output:

```text
/home/user/myapp          [main]
/home/user/myapp-payment  [feature/payment]
/home/user/myapp-hotfix   [bugfix/login]
```

<!-- end_slide -->

# Removing A Worktree

Remove it safely:

```bash
git worktree remove ../myapp-payment
```

Clean stale references:

```bash
git worktree prune
```

<!-- end_slide -->

# Important Rule

A branch can only be checked out once.

Example:

```text
main
```

cannot exist in two worktrees simultaneously.

Git prevents this automatically.

<!-- end_slide -->

# Worktrees vs Stash

| Task                     | Stash | Worktree |
| ------------------------ | ----- | -------- |
| Save work temporarily    | ✅     | ❌        |
| Work on another branch   | ⚠️    | ✅        |
| Review PRs               | ⚠️    | ✅        |
| Multiple active branches | ❌     | ✅        |
| Compare implementations  | ❌     | ✅        |
| Parallel development     | ❌     | ✅        |

<!-- end_slide -->

# Common Workflow: Hotfix

Current branch:

```text
feature/payment
```

Production issue appears.

Create a worktree:

```bash
git worktree add ../hotfix main
```

<!-- end_slide -->

# Hotfix Workflow

Open a second terminal:

```bash
cd ../hotfix

git checkout -b hotfix/login
```

Fix production.

Meanwhile:

```text
feature/payment
```

remains untouched.

<!-- end_slide -->

# Common Workflow: PR Reviews

Create a review workspace:

```bash
git worktree add ../pr-123 pr-123
```

Open:

```text
feature/payment
```

and

```text
pr-123
```

side-by-side.

<!-- end_slide -->

# Common Workflow: Comparing Branches

Without worktrees:

```bash
git checkout feature-a

git checkout feature-b
```

With worktrees:

```text
feature-a
feature-b
```

open simultaneously.

Much easier to compare behavior and code.

<!-- end_slide -->



# Multi-Root Workspace

You can open multiple worktrees together.

```json
{
  "folders": [
    { "path": "myapp" },
    { "path": "myapp-payment" },
    { "path": "myapp-hotfix" }
  ]
}
```

<!-- end_slide -->

# Recommended Layout

```text
projects/

├── myapp/
├── myapp-feature-auth/
├── myapp-hotfix/
├── myapp-pr-123/
└── myapp-release/
```

Keep all worktrees together.

<!-- end_slide -->

# Useful Commands

Create:

```bash
git worktree add ../feature-x feature-x
```

List:

```bash
git worktree list
```

Remove:

```bash
git worktree remove ../feature-x
```

Prune:

```bash
git worktree prune
```

Repair:

```bash
git worktree repair
```

<!-- end_slide -->

# Live Demo

Create repository:

```bash
git init demo

cd demo

git commit --allow-empty -m "Initial commit"
```

<!-- end_slide -->

# Live Demo

Create a feature worktree:

```bash
git worktree add \
  ../demo-feature \
  -b feature/auth
```

List worktrees:

```bash
git worktree list
```

<!-- end_slide -->

# Live Demo

Open both folders in separate editor windows (one per worktree).

```text
Open both folders in separate editor windows
```

Observe:

* Different branches
* Different editors
* Same repository

<!-- end_slide -->

# Key Takeaways

Git Worktrees:

✅ Eliminate constant branch switching

✅ Remove the need for extra clones

✅ Reduce reliance on stashing

✅ Simplify hotfixes

✅ Improve PR reviews

<!-- end_slide -->

