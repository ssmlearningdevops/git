# Git Commands Reference

A quick reference guide for the most commonly used Git commands.

---

## Table of Contents

1. [git add](#1-git-add)
2. [git commit](#2-git-commit)
3. [git push](#3-git-push)
4. [git fetch](#4-git-fetch)
5. [git merge](#5-git-merge)
6. [git pull](#6-git-pull)
7. [git diff](#7-git-diff)
8. [git diff HEAD](#8-git-diff-head)
9. [git status](#9-git-status)
10. [git branch](#10-git-branch)
11. [git checkout](#11-git-checkout)
12. [git log](#12-git-log)
13. [git stash](#13-git-stash)
14. [git rebase](#14-git-rebase)
15. [git reset](#15-git-reset)
16. [git revert](#16-git-revert)
17. [git cherry-pick](#17-git-cherry-pick)
18. [git bisect](#18-git-bisect)
19. [git init](#19-git-init)

---

## 1. `git add`

Adds changes from the working directory into the staging area.

```bash
git add <file>        # Stage a specific file
git add .             # Stage all changes in the current directory
git add -p            # Interactively stage chunks of changes
```

---

## 2. `git commit`

Saves a snapshot of currently staged changes in the local repository, with a message.

```bash
git commit -m "your message"       # Commit with an inline message
git commit --amend                 # Modify the most recent commit
```

---

## 3. `git push`

Uploads committed changes from the local repository to a remote repository.

```bash
git push                           # Push to the tracked remote branch
git push origin <branch>           # Push a specific branch to origin
git push -u origin <branch>        # Push and set upstream tracking
```

---

## 4. `git fetch`

Downloads changes from a remote repository without applying them locally.

```bash
git fetch                          # Fetch from the default remote
git fetch origin                   # Fetch from origin
git fetch --all                    # Fetch from all remotes
```

---

## 5. `git merge`

Combines changes from one branch into another.

```bash
git merge <branch>                 # Merge a branch into the current branch
git merge --no-ff <branch>         # Merge with a merge commit (no fast-forward)
git merge --abort                  # Abort an in-progress merge
```

---

## 6. `git pull`

Fetches and then merges changes from a remote repository into the local branch.

```bash
git pull                           # Pull from the tracked remote branch
git pull origin <branch>           # Pull a specific remote branch
git pull --rebase                  # Pull using rebase instead of merge
```

---

## 7. `git diff`

Shows changes that are not staged or committed yet.

```bash
git diff                           # Show unstaged changes
git diff --staged                  # Show staged changes
git diff <branch1> <branch2>       # Compare two branches
```

---

## 8. `git diff HEAD`

Shows changes between the current working directory and the latest commit.

```bash
git diff HEAD                      # Diff working directory vs last commit
git diff HEAD~1                    # Diff working directory vs second-to-last commit
```

---

## 9. `git status`

Shows the current state of the working directory and staging area.

```bash
git status                         # Full status output
git status -s                      # Short/compact status output
```

---

## 10. `git branch`

Lists, creates, or deletes branches.

```bash
git branch                         # List all local branches
git branch -a                      # List all local and remote branches
git branch <name>                  # Create a new branch
git branch -d <name>               # Delete a branch (safe)
git branch -D <name>               # Force delete a branch
```

---

## 11. `git checkout`

Switches between branches or creates a new one.

```bash
git checkout <branch>              # Switch to an existing branch
git checkout -b <branch>           # Create and switch to a new branch
git checkout <commit>              # Detach HEAD to a specific commit
```

> **Note:** For newer versions of Git, `git switch` is preferred for switching branches and `git restore` for discarding changes.

---

## 12. `git log`

Shows commits on the current branch with extra details.

```bash
git log                            # Full log
git log --oneline                  # Compact one-line-per-commit log
git log --oneline --graph          # Visual branch/merge graph
git log --author="name"            # Filter by author
```

---

## 13. `git stash`

Temporarily saves uncommitted changes so they can be applied later.

```bash
git stash                          # Stash current changes
git stash list                     # List all stashes
git stash pop                      # Apply the latest stash and remove it
git stash apply stash@{n}          # Apply a specific stash without removing it
git stash drop stash@{n}           # Delete a specific stash
```

---

## 14. `git rebase`

Applies commits from one branch onto another, replaying them in sequence.

```bash
git rebase <branch>                # Rebase current branch onto another
git rebase -i HEAD~n               # Interactive rebase for the last n commits
git rebase --abort                 # Abort an in-progress rebase
git rebase --continue              # Continue after resolving conflicts
```

---

## 15. `git reset`

Undoes changes in the working directory and moves back to a specific commit.

```bash
git reset --soft HEAD~1            # Undo last commit, keep changes staged
git reset --mixed HEAD~1           # Undo last commit, keep changes unstaged (default)
git reset --hard HEAD~1            # Undo last commit, discard all changes
```

> **Warning:** `--hard` permanently discards uncommitted changes.

---

## 16. `git revert`

Undoes changes by creating a new commit, preserving history.

```bash
git revert <commit>                # Revert a specific commit
git revert HEAD                    # Revert the latest commit
git revert --no-commit <commit>    # Stage the revert without committing
```

> **Note:** Prefer `git revert` over `git reset` when working on shared/public branches.

---

## 17. `git cherry-pick`

Applies one or more specific commits from one branch onto another.

```bash
git cherry-pick <commit>           # Apply a specific commit
git cherry-pick <c1>..<c2>         # Apply a range of commits
git cherry-pick --abort            # Abort an in-progress cherry-pick
```

---

## 18. `git bisect`

Uses binary search to narrow down which commit introduced a bug.

```bash
git bisect start                   # Start a bisect session
git bisect bad                     # Mark the current commit as bad
git bisect good <commit>           # Mark a known good commit
git bisect reset                   # End the bisect session
```

---

## 19. `git init`

Creates a new empty Git repository in the current directory.

```bash
git init                           # Initialize a repo in the current folder
git init <directory>               # Initialize a repo in a new folder
```

---

## Quick Summary Table

| Command | Purpose |
|---|---|
| `git add` | Stage changes |
| `git commit` | Save staged snapshot |
| `git push` | Upload to remote |
| `git fetch` | Download from remote (no merge) |
| `git merge` | Combine branches |
| `git pull` | Fetch + merge |
| `git diff` | Show unstaged changes |
| `git diff HEAD` | Show all uncommitted changes |
| `git status` | Show working directory state |
| `git branch` | List/manage branches |
| `git checkout` | Switch/create branches |
| `git log` | View commit history |
| `git stash` | Temporarily shelve changes |
| `git rebase` | Replay commits onto another branch |
| `git reset` | Undo commits (rewrites history) |
| `git revert` | Undo commits (safe, new commit) |
| `git cherry-pick` | Apply specific commits |
| `git bisect` | Binary search for bugs |
| `git init` | Create a new repository |
