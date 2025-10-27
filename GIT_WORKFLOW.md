# Git Workflow Strategy for Independent Fork Development

## Overview
This document describes the git workflow for maintaining your fork of mini-swe-agent independently while keeping the ability to sync with upstream.

## Branch Structure

```
┌─────────────────────────────────────────────────────────────┐
│                  Repository Structure                        │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  origin/main (lelandfy/mini-swe-agent)                       │
│      ├── Your independent development                        │
│      ├── Your custom features                                │
│      └── Always ahead of upstream                            │
│                                                               │
│  origin/upstream-sync                                        │
│      └── Tracks upstream/main exactly (read-only mirror)     │
│                                                               │
│  upstream/main (SWE-agent/mini-swe-agent)                    │
│      └── Original repository (read-only)                     │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

## Current Setup

### Remotes
- **origin**: `https://github.com/lelandfy/mini-swe-agent.git` (your fork)
- **upstream**: `https://github.com/SWE-agent/mini-swe-agent.git` (original)

### Branches
- **main**: Tracks `origin/main` - your independent development
- **upstream-sync**: Tracks `upstream/main` - exact mirror of upstream

## Daily Workflows

### Workflow 1: Regular Development (Most Common)

```bash
# Work on your fork's main branch
git checkout main

# Make changes
# ... edit files ...

# Commit and push
git add .
git commit -m "Your changes"
git push origin main

# Or create feature branches from main
git checkout -b feature/my-feature
# ... work ...
git push origin feature/my-feature
```

### Workflow 2: Sync with Upstream

When you want to pull in updates from the original repository:

```bash
# Step 1: Update your upstream-sync branch
git checkout upstream-sync
git pull upstream main
git push origin upstream-sync

# Step 2A: Cherry-pick specific commits you want
git checkout main
git cherry-pick <commit-hash>
git push origin main

# OR Step 2B: Merge all upstream changes (use carefully)
git checkout main
git merge upstream-sync
# Resolve conflicts if any
git push origin main

# OR Step 2C: Rebase your work on top of upstream (cleanest)
git checkout main
git rebase upstream-sync
# Resolve conflicts if any
git push origin main --force-with-lease
```

### Workflow 3: Contributing Back to Upstream

When you want to contribute a fix back to the original repository:

```bash
# Step 1: Create a clean branch from upstream
git checkout upstream-sync
git pull upstream main
git checkout -b pr/fix-something

# Step 2: Make your targeted changes
# ... edit files ...
git add .
git commit -m "Fix: Description of fix"

# Step 3: Push to your fork
git push origin pr/fix-something

# Step 4: Create PR on GitHub
# Go to: https://github.com/lelandfy/mini-swe-agent/pull/new/pr/fix-something
# Set base repository: SWE-agent/mini-swe-agent
# Set base branch: main
# Set head repository: lelandfy/mini-swe-agent
# Set compare branch: pr/fix-something
```

## Important Commands

### Check Branch Status
```bash
# See which branch you're on and tracking info
git branch -vv

# See all branches including remotes
git branch -a

# Visualize git history
git log --oneline --graph --all --decorate -20
```

### Update Remotes
```bash
# Fetch latest from both remotes
git fetch --all

# Fetch only from upstream
git fetch upstream

# Fetch only from origin
git fetch origin
```

### Compare Branches
```bash
# See what's in main but not in upstream-sync
git log upstream-sync..main

# See what's in upstream-sync but not in main
git log main..upstream-sync

# See files changed between branches
git diff upstream-sync..main --name-only
```

## Best Practices

### DO:
✅ Keep `origin/main` as your primary development branch
✅ Use `origin/upstream-sync` as a read-only mirror of upstream
✅ Create feature branches for experimental work
✅ Regularly fetch updates from upstream
✅ Cherry-pick only the commits you need from upstream
✅ Use `--force-with-lease` instead of `--force` for safety

### DON'T:
❌ Don't commit directly to `upstream-sync` branch
❌ Don't merge upstream blindly (review changes first)
❌ Don't use `git pull` on main if you have local commits
❌ Don't use `--force` without `--with-lease`

## Common Scenarios

### Scenario 1: Upstream has new features you want
```bash
git checkout upstream-sync
git pull upstream main
git push origin upstream-sync

# Review the changes
git log main..upstream-sync

# Cherry-pick specific commits
git checkout main
git cherry-pick <commit-hash>
git push origin main
```

### Scenario 2: You fixed a bug and want to contribute back
```bash
# Create PR branch from clean upstream
git checkout upstream-sync
git pull upstream main
git checkout -b pr/bug-fix

# Apply your fix
git cherry-pick <your-fix-commit>
# or manually recreate the fix

git push origin pr/bug-fix
# Then create PR on GitHub
```

### Scenario 3: Keep your fork updated but separate
```bash
# Just update the mirror, don't merge
git checkout upstream-sync
git pull upstream main
git push origin upstream-sync

# Your main branch stays independent
git checkout main
# Continue your independent work
```

## Current State

As of now:
- ✅ `main` tracks `origin/main` (your fork)
- ✅ `upstream-sync` tracks `upstream/main` (original repo)
- ✅ Your 2 commits are on `origin/main`:
  - `5e60211` - Fix: Resolve Interactive Textual test failures and organize docs
  - `a4282b8` - Fix: Resolve Inspector UI test failures
- ✅ `origin/upstream-sync` mirrors `upstream/main`

## Quick Reference

```bash
# Daily development
git checkout main && git pull origin main
# ... work ...
git push origin main

# Sync upstream mirror
git checkout upstream-sync && git pull upstream main && git push origin upstream-sync

# Cherry-pick from upstream
git checkout main && git cherry-pick <commit-hash> && git push origin main

# Create PR branch
git checkout upstream-sync && git pull upstream main && git checkout -b pr/feature
```

## Resources
- [GitHub Fork Documentation](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks)
- [Git Cherry-pick](https://git-scm.com/docs/git-cherry-pick)
- [Git Rebase](https://git-scm.com/docs/git-rebase)
