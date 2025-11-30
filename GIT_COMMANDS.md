# 📚 Git Commands Reference Guide

A comprehensive reference for all essential Git commands you need for open source contribution and development.

## 📋 Table of Contents

- [Quick Start](#quick-start)
- [Basic Commands](#basic-commands)
- [Branching Commands](#branching-commands)
- [Commit Commands](#commit-commands)
- [Remote Commands](#remote-commands)
- [Syncing Commands](#syncing-commands)
- [Advanced Commands](#advanced-commands)
- [Workflow Cheat Sheet](#workflow-cheat-sheet)
- [Common Scenarios](#common-scenarios)

---

## Quick Start

```bash
# First time setup (configure your identity)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Verify configuration
git config --list
```

---

## Basic Commands

### Initialize a Repository

```bash
# Create a new Git repository in current directory
git init

# Create a new Git repository in a specific directory
git init my-project
```

### Clone a Repository

```bash
# Clone a repository
git clone https://github.com/username/repository.git

# Clone with a different folder name
git clone https://github.com/username/repository.git my-folder

# Clone a specific branch
git clone -b branch-name https://github.com/username/repository.git
```

### Check Status

```bash
# Show the working tree status
git status

# Show status in short format
git status -s

# Show branch and tracking info
git status -sb
```

**Output explanation:**
- 🟢 **Untracked files** - New files not yet tracked by Git
- 🟡 **Modified files** - Changed files not staged for commit
- 🔵 **Staged files** - Files ready to be committed

### View Changes

```bash
# Show changes in working directory (not staged)
git diff

# Show changes that are staged for commit
git diff --staged

# Show changes for a specific file
git diff filename.txt

# Show changes between branches
git diff branch1..branch2
```

---

## Branching Commands

Branches are essential for parallel development and open source contributions.

### Create Branches

```bash
# Create a new branch
git branch feature-name

# Create and switch to new branch
git checkout -b feature-name

# Create and switch to new branch (modern syntax)
git switch -c feature-name

# Create a branch from a specific commit
git branch feature-name commit-hash
```

### Switch Branches

```bash
# Switch to an existing branch
git checkout branch-name

# Switch to an existing branch (modern syntax)
git switch branch-name

# Switch to previous branch
git checkout -

# Switch and create if doesn't exist
git checkout -B branch-name
```

### List Branches

```bash
# List all local branches
git branch

# List all branches (local + remote)
git branch -a

# List remote branches only
git branch -r

# List branches with last commit info
git branch -v

# List merged branches
git branch --merged

# List unmerged branches
git branch --no-merged
```

### Rename Branches

```bash
# Rename current branch
git branch -m new-branch-name

# Rename a specific branch
git branch -m old-name new-name
```

### Delete Branches

```bash
# Delete a local branch (safe - won't delete if unmerged)
git branch -d branch-name

# Force delete a local branch
git branch -D branch-name

# Delete a remote branch
git push origin --delete branch-name

# Delete multiple local branches
git branch -d branch1 branch2 branch3
```

---

## Commit Commands

Commits are snapshots of your code at specific points in time.

### Stage Files

```bash
# Stage a specific file
git add filename.txt

# Stage all changes (new, modified, deleted)
git add .

# Stage all files in current directory
git add *

# Stage multiple specific files
git add file1.txt file2.js file3.css

# Stage all files of a specific type
git add *.js

# Stage only modified and deleted files (not new files)
git add -u

# Interactive staging
git add -p
```

### Unstage Files

```bash
# Unstage a specific file
git restore --staged filename.txt

# Unstage all files
git restore --staged .

# Unstage using reset (alternative)
git reset filename.txt

# Unstage all files using reset
git reset
```

### Commit Changes

```bash
# Commit staged changes with a message
git commit -m "Your commit message"

# Commit with multi-line message
git commit -m "Title" -m "Description line 1" -m "Description line 2"

# Stage all tracked files and commit
git commit -am "Your commit message"

# Commit with detailed message in editor
git commit
```

**Good commit message examples:**
```bash
git commit -m "Add user authentication feature"
git commit -m "Fix login redirect bug for new users"
git commit -m "Update README with installation instructions"
git commit -m "Refactor database connection logic"
```

### Amend Commits

```bash
# Amend the last commit (change message)
git commit --amend -m "New commit message"

# Amend the last commit (add more files)
git add forgotten-file.txt
git commit --amend --no-edit

# Amend the last commit (open editor)
git commit --amend
```

> [!WARNING]
> Never amend commits that have been pushed to a shared branch! This rewrites history.

### View Commit History

```bash
# Show commit history
git log

# Show last N commits
git log -n 5

# Show commits in one line each
git log --oneline

# Show commits with diff
git log -p

# Show commits with stats
git log --stat

# Show commits graphically
git log --graph --oneline --all

# Show commits by author
git log --author="Your Name"

# Show commits in a date range
git log --since="2024-01-01" --until="2024-12-31"

# Show commits that modified a specific file
git log -- filename.txt
```

### View Specific Commit

```bash
# Show details of a specific commit
git show commit-hash

# Show specific commit's changes
git show commit-hash:filename.txt
```

---

## Remote Commands

Remotes are connections to repositories hosted on GitHub, GitLab, etc.

### View Remotes

```bash
# List all remotes
git remote

# List remotes with URLs
git remote -v

# Show detailed info about a remote
git remote show origin
```

### Add Remotes

```bash
# Add a new remote
git remote add remote-name https://github.com/user/repo.git

# Add upstream (for forks)
git remote add upstream https://github.com/original-owner/repo.git

# Common setup for open source (fork workflow)
git remote add origin https://github.com/YourUsername/repo.git
git remote add upstream https://github.com/OriginalOwner/repo.git
```

### Remove/Rename Remotes

```bash
# Remove a remote
git remote remove remote-name

# Rename a remote
git remote rename old-name new-name
```

### Fetch from Remotes

```bash
# Fetch all changes from origin (doesn't merge)
git fetch origin

# Fetch from a specific remote
git fetch upstream

# Fetch all remotes
git fetch --all

# Fetch and prune deleted branches
git fetch -p
```

### Pull from Remotes

```bash
# Fetch and merge changes from origin
git pull origin main

# Pull from current branch's tracking branch
git pull

# Pull with rebase instead of merge
git pull --rebase

# Pull from upstream
git pull upstream main
```

### Push to Remotes

```bash
# Push current branch to origin
git push origin branch-name

# Push and set upstream tracking
git push -u origin branch-name

# Push all branches
git push --all

# Push tags
git push --tags

# Force push (dangerous!)
git push --force

# Force push (safer - won't overwrite others' work)
git push --force-with-lease
```

> [!CAUTION]
> Use `--force` with extreme caution! It rewrites history and can cause data loss.

---

## Syncing Commands

Keep your fork updated with the original repository.

### Setup Upstream (One-Time Setup)

```bash
# Add the original repository as upstream
git remote add upstream https://github.com/original-owner/repo.git

# Verify remotes
git remote -v
```

You should see:
```
origin    https://github.com/YourUsername/repo.git (fetch)
origin    https://github.com/YourUsername/repo.git (push)
upstream  https://github.com/original-owner/repo.git (fetch)
upstream  https://github.com/original-owner/repo.git (push)
```

### Sync Fork with Upstream

```bash
# Fetch changes from upstream
git fetch upstream

# Switch to your main branch
git checkout main

# Merge upstream changes
git merge upstream/main

# Push updates to your fork
git push origin main
```

**One-liner alternative:**
```bash
git checkout main && git fetch upstream && git merge upstream/main && git push origin main
```

### Update Feature Branch with Main

```bash
# Switch to your feature branch
git checkout feature-branch

# Merge latest main into your feature branch
git merge main

# Or rebase your feature branch onto main
git rebase main
```

---

## Advanced Commands

### Stash Changes

Stashing temporarily saves your uncommitted changes.

```bash
# Stash current changes
git stash

# Stash with a message
git stash save "Work in progress on feature X"

# List all stashes
git stash list

# Apply most recent stash
git stash apply

# Apply a specific stash
git stash apply stash@{2}

# Apply and remove stash
git stash pop

# Remove a specific stash
git stash drop stash@{0}

# Remove all stashes
git stash clear

# Show stash contents
git stash show -p stash@{0}
```

**Use case:** Need to switch branches but have uncommitted changes.

### Reset Changes

```bash
# Discard changes in working directory
git restore filename.txt

# Discard all changes in working directory
git restore .

# Move HEAD to a previous commit (keep changes)
git reset --soft HEAD~1

# Move HEAD to a previous commit (unstage changes)
git reset HEAD~1

# Move HEAD to a previous commit (discard all changes)
git reset --hard HEAD~1

# Reset to a specific commit
git reset --hard commit-hash
```

> [!WARNING]
> `git reset --hard` permanently deletes uncommitted changes!

### Cherry-Pick Commits

Apply a specific commit from one branch to another.

```bash
# Apply a specific commit to current branch
git cherry-pick commit-hash

# Apply multiple commits
git cherry-pick commit-hash1 commit-hash2

# Cherry-pick without committing
git cherry-pick -n commit-hash
```

### Rebase

Reapply commits on top of another base.

```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase (edit last 3 commits)
git rebase -i HEAD~3

# Continue rebase after resolving conflicts
git rebase --continue

# Abort rebase
git rebase --abort

# Skip current commit during rebase
git rebase --skip
```

**Interactive rebase commands:**
- `pick` - use the commit
- `reword` - use commit, but edit message
- `edit` - use commit, but stop for amending
- `squash` - meld into previous commit
- `drop` - remove commit

### Resolve Merge Conflicts

```bash
# Check which files have conflicts
git status

# After manually resolving conflicts in files
git add resolved-file.txt

# Continue the merge
git commit

# Or abort the merge
git merge --abort
```

**Conflict markers in files:**
```
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> branch-name
```

Remove the markers and keep the correct code.

### View Reflog

Reflog shows a history of all HEAD movements (useful for recovering lost commits).

```bash
# Show reflog
git reflog

# Recover a "lost" commit
git checkout commit-hash

# Create a branch from a reflog entry
git branch recovery-branch HEAD@{5}
```

### Clean Untracked Files

```bash
# Show what would be deleted (dry run)
git clean -n

# Delete untracked files
git clean -f

# Delete untracked files and directories
git clean -fd

# Delete ignored files too
git clean -fx
```

---

## Workflow Cheat Sheet

### Open Source Contribution Workflow

```bash
# 1. Fork repository on GitHub, then:

# 2. Clone your fork
git clone https://github.com/YourUsername/repo.git
cd repo

# 3. Add upstream remote
git remote add upstream https://github.com/OriginalOwner/repo.git

# 4. Sync your fork
git checkout main
git pull upstream main
git push origin main

# 5. Create feature branch
git checkout -b fix-bug

# 6. Make changes, then stage and commit
git add .
git commit -m "Fix: resolve authentication bug"

# 7. Push to your fork
git push -u origin fix-bug

# 8. Create Pull Request on GitHub

# 9. After PR is merged, cleanup
git checkout main
git pull upstream main
git push origin main
git branch -d fix-bug
git push origin --delete fix-bug
```

### Daily Development Workflow

```bash
# Start of day - update main
git checkout main
git pull origin main

# Create feature branch
git checkout -b feature/new-dashboard

# Make changes periodically
git add .
git commit -m "Add dashboard layout"

# More changes
git add .
git commit -m "Add dashboard widgets"

# Push to remote
git push -u origin feature/new-dashboard

# End of day - sync with main
git fetch origin
git merge origin/main

# Push final changes
git push origin feature/new-dashboard
```

### Fix a Bug Workflow

```bash
# Create hotfix branch
git checkout -b hotfix/login-error

# Fix the bug
# ... edit files ...

# Commit fix
git add .
git commit -m "Fix: resolve login redirect error"

# Push and create PR
git push -u origin hotfix/login-error
```

---

## Common Scenarios

### Scenario 1: I committed to the wrong branch

```bash
# Copy the commit to correct branch
git checkout correct-branch
git cherry-pick wrong-branch

# Remove commit from wrong branch
git checkout wrong-branch
git reset --hard HEAD~1
```

### Scenario 2: I want to undo my last commit

```bash
# Keep changes, just undo commit
git reset --soft HEAD~1

# Undo commit and unstage changes
git reset HEAD~1

# Completely remove commit and changes
git reset --hard HEAD~1
```

### Scenario 3: I need to change my last commit message

```bash
# Change the last commit message
git commit --amend -m "New message"

# Push the change (if already pushed)
git push --force-with-lease
```

### Scenario 4: I have merge conflicts

```bash
# Check conflicted files
git status

# Open conflicted files and resolve manually
# Remove conflict markers (<<<<<<, =======, >>>>>>>)

# After resolving
git add resolved-file.txt
git commit
```

### Scenario 5: I want to discard all local changes

```bash
# Discard all uncommitted changes
git restore .

# Discard changes and remove untracked files
git restore .
git clean -fd

# Reset to match remote exactly
git fetch origin
git reset --hard origin/main
```

### Scenario 6: I pushed sensitive data (API key, password)

```bash
# Remove file from Git history
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/file" \
  --prune-empty --tag-name-filter cat -- --all

# Force push
git push --force --all

# Better: Use BFG Repo-Cleaner
# https://rtyley.github.io/bfg-repo-cleaner/
```

> [!CAUTION]
> Consider the data compromised. Rotate all credentials immediately!

### Scenario 7: I want to work on someone else's PR

```bash
# Fetch their PR
git fetch origin pull/123/head:pr-123

# Switch to it
git checkout pr-123

# Make changes and push to their branch (if you have permission)
git push origin pr-123
```

### Scenario 8: My fork is behind by many commits

```bash
# Sync fork
git checkout main
git fetch upstream
git reset --hard upstream/main
git push origin main --force

# Update all your feature branches
git checkout feature-branch
git rebase main
```

---

## Quick Command Reference Table

| Task | Command |
|------|---------|
| **Check status** | `git status` |
| **Stage all changes** | `git add .` |
| **Commit** | `git commit -m "message"` |
| **Push** | `git push origin branch-name` |
| **Pull** | `git pull origin branch-name` |
| **Create branch** | `git checkout -b branch-name` |
| **Switch branch** | `git checkout branch-name` |
| **Delete branch** | `git branch -d branch-name` |
| **View branches** | `git branch -a` |
| **View commits** | `git log --oneline` |
| **Undo last commit** | `git reset --soft HEAD~1` |
| **Discard changes** | `git restore .` |
| **Stash changes** | `git stash` |
| **Apply stash** | `git stash pop` |
| **Sync fork** | `git pull upstream main` |

---

## Git Configuration Tips

### Useful Aliases

Add these to your `~/.gitconfig` file:

```ini
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    cm = commit -m
    aa = add --all
    unstage = restore --staged
    last = log -1 HEAD
    visual = log --graph --oneline --all
    amend = commit --amend --no-edit
```

Usage:
```bash
git st              # Instead of git status
git co main         # Instead of git checkout main
git cm "message"    # Instead of git commit -m "message"
```

### Set Default Branch Name

```bash
# Set default branch name to 'main' for new repositories
git config --global init.defaultBranch main
```

### Enable Colored Output

```bash
git config --global color.ui auto
```

### Set Default Editor

```bash
# Use VS Code
git config --global core.editor "code --wait"

# Use Vim
git config --global core.editor "vim"

# Use Nano
git config --global core.editor "nano"
```

---

## Additional Resources

- 📖 [Official Git Documentation](https://git-scm.com/doc)
- 🎓 [Git Handbook by GitHub](https://guides.github.com/introduction/git-handbook/)
- 🆘 [Oh Shit, Git!?!](https://ohshitgit.com/) - Fix common Git mistakes
- 🎮 [Learn Git Branching](https://learngitbranching.js.org/) - Interactive tutorial
- 📚 [Pro Git Book](https://git-scm.com/book/en/v2) - Free online book

---

**Happy Coding! 🚀**

Keep this reference handy and remember: Git is powerful but forgiving. Almost everything can be undone!
