# Git Scenario-Based Questions & Answers

A practical guide covering beginner to advanced Git scenarios with step-by-step answers.

---

## Table of Contents

### Beginner
1. [Initialize a repository and connect to GitHub](#1-initialize-a-repository-and-connect-to-github)
2. [Stage specific files and commit](#2-stage-specific-files-and-commit)
3. [Undo changes to a file](#3-undo-changes-to-a-file)
4. [Clone a repository](#4-clone-a-repository)

### Intermediate
5. [Resolve a merge conflict](#5-resolve-a-merge-conflict)
6. [Pull vs fetch](#6-pull-vs-fetch)
7. [Stash uncommitted changes](#7-stash-uncommitted-changes)
8. [Rebase a feature branch](#8-rebase-a-feature-branch)

### Advanced
9. [Cherry-pick a specific commit](#9-cherry-pick-a-specific-commit)
10. [Recover from a detached HEAD](#10-recover-from-a-detached-head)
11. [Reset vs revert on a shared branch](#11-reset-vs-revert-on-a-shared-branch)
12. [Recover lost commits after a reset](#12-recover-lost-commits-after-a-reset)
13. [Enforce commit message standards with Git hooks](#13-enforce-commit-message-standards-with-git-hooks)

---

## Beginner

---

### 1. Initialize a repository and connect to GitHub

**Scenario:**
You just created a new project folder called `my-app` on your machine. You want to track it with Git and push it to a new GitHub repository.

**Question:**
How do you set up Git locally and connect it to GitHub?

**Answer:**

**Step 1 — Initialize the local repository**
```bash
cd my-app
git init
```
This creates a hidden `.git` folder that starts tracking the project.

**Step 2 — Stage and commit your initial files**
```bash
git add .
git commit -m "Initial commit"
```

**Step 3 — Create the remote repository**
Go to [github.com](https://github.com), click **New repository**, name it `my-app`, and create it (without initializing with a README, since you already have one locally).

**Step 4 — Link local repo to GitHub and push**
```bash
git remote add origin https://github.com/your-username/my-app.git
git branch -M main
git push -u origin main
```

- `git remote add origin` — registers the GitHub URL as a remote named `origin`
- `-M main` — renames the default branch to `main`
- `-u` — sets upstream tracking so future `git push` / `git pull` work without specifying the remote

**Result:** Your project is now on GitHub and future pushes only require `git push`.

---

### 2. Stage specific files and commit

**Scenario:**
You modified three files: `index.html`, `style.css`, and `app.js`. The change in `app.js` is incomplete and not ready to commit. You only want to commit the changes in `index.html` and `style.css`.

**Question:**
How do you stage only two of the three modified files and commit them?

**Answer:**

**Step 1 — Stage only the files you want**
```bash
git add index.html style.css
```

**Step 2 — Verify what is staged**
```bash
git status
```
You will see:
- `index.html` and `style.css` listed under **Changes to be committed**
- `app.js` listed under **Changes not staged for commit**

**Step 3 — Commit only the staged files**
```bash
git commit -m "Update HTML structure and styles"
```

`app.js` remains modified in your working directory and is not included in this commit. You can continue editing it and commit it separately when ready.

> **Key concept:** `git add` controls what goes into a commit. The working directory and staging area are separate from the local repository.

---

### 3. Undo changes to a file

**Scenario:**
You were editing `config.js` and made a mess of it. You have not staged or committed the changes. You want to restore it to the last committed state.

**Question:**
What command do you use to discard the uncommitted changes to a single file?

**Answer:**

```bash
git restore config.js
```

This discards all modifications in `config.js` and restores it to the version in the last commit.

**If the file was already staged**, you need two steps:
```bash
git restore --staged config.js   # Unstage the file
git restore config.js            # Discard the working directory changes
```

**Older Git alternative (pre-2.23):**
```bash
git checkout -- config.js
```

> **Warning:** `git restore` is destructive for the working directory. The discarded changes cannot be recovered.

---

### 4. Clone a repository

**Scenario:**
Your teammate shares this GitHub link with you: `https://github.com/team/project.git`. You need a local copy of the project to start contributing.

**Question:**
How do you get the project onto your local machine?

**Answer:**

```bash
git clone https://github.com/team/project.git
```

This will:
1. Create a new folder called `project` in your current directory
2. Download all branches, commits, and files
3. Automatically set `origin` as the remote pointing to the GitHub URL

**Clone into a specific folder name:**
```bash
git clone https://github.com/team/project.git my-folder-name
```

**After cloning:**
```bash
cd project
git status       # Confirm you are on the main branch
git log          # Review commit history
```

> You are now ready to create a branch and start contributing.

---

## Intermediate

---

### 5. Resolve a merge conflict

**Scenario:**
You have been working on `feature/login`. Your teammate merged changes into `main` that touch the same lines in `auth.js` that you also modified. When you run `git merge main` into your branch, Git reports a conflict.

**Question:**
How do you resolve the merge conflict step by step?

**Answer:**

**Step 1 — Attempt the merge**
```bash
git checkout feature/login
git merge main
```
Git outputs something like:
```
CONFLICT (content): Merge conflict in auth.js
Automatic merge failed; fix conflicts and then commit the result.
```

**Step 2 — Identify conflicting files**
```bash
git status
```
Files listed under `both modified` have conflicts.

**Step 3 — Open and resolve the conflict**
Open `auth.js`. Git marks the conflict like this:
```
<<<<<<< HEAD
// your changes
const timeout = 3000;
=======
// incoming changes from main
const timeout = 5000;
>>>>>>> main
```

Edit the file to keep the correct version (or combine both):
```js
const timeout = 5000; // updated per team standard
```

Remove all `<<<<<<<`, `=======`, and `>>>>>>>` markers.

**Step 4 — Stage the resolved file**
```bash
git add auth.js
```

**Step 5 — Complete the merge**
```bash
git commit -m "Merge main into feature/login, resolve auth.js conflict"
```

**Step 6 — Verify**
```bash
git log --oneline --graph
```

> **Tip:** Tools like VS Code's built-in merge editor or `git mergetool` make conflict resolution easier visually.

---

### 6. Pull vs fetch

**Scenario:**
Your remote branch has new commits from teammates. Before integrating them, you want to inspect what changed without affecting your local branch.

**Question:**
Which command do you use and why? What is the difference between `git pull` and `git fetch`?

**Answer:**

Use `git fetch`:
```bash
git fetch origin
```

This downloads all new commits, branches, and tags from the remote into your local remote-tracking references (e.g. `origin/main`), but **does not touch your working directory or current branch**.

**Inspect the changes before merging:**
```bash
git log origin/main --oneline          # See incoming commits
git diff main origin/main              # See what will change
```

**Once satisfied, merge manually:**
```bash
git merge origin/main
```

**Comparison:**

| | `git fetch` | `git pull` |
|---|---|---|
| Downloads remote changes | Yes | Yes |
| Merges into local branch | No | Yes (automatically) |
| Safe to inspect first | Yes | No |
| Equivalent to | `fetch` only | `fetch` + `merge` |

> **Rule of thumb:** Use `git fetch` when you want control. Use `git pull` when you trust the remote and want a fast update.

---

### 7. Stash uncommitted changes

**Scenario:**
You are halfway through editing `dashboard.js` when your team lead asks you to urgently fix a bug on the `hotfix/navbar` branch. Your changes are not ready to commit, and switching branches would either block you or carry over unfinished work.

**Question:**
How do you save your work temporarily and switch branches cleanly?

**Answer:**

**Step 1 — Stash your current changes**
```bash
git stash
```
This saves all tracked, modified files to a stash stack and reverts your working directory to the last commit.

**Step 2 — Switch to the hotfix branch**
```bash
git checkout hotfix/navbar
```

**Step 3 — Fix the bug, commit, and push**
```bash
# ... make the fix ...
git add .
git commit -m "Fix navbar collapse on mobile"
git push origin hotfix/navbar
```

**Step 4 — Return to your original branch**
```bash
git checkout feature/dashboard
```

**Step 5 — Restore your stashed work**
```bash
git stash pop
```
This re-applies the most recent stash and removes it from the stash list.

**Useful stash commands:**
```bash
git stash list                    # View all stashes
git stash show -p stash@{0}       # Inspect a stash's changes
git stash apply stash@{1}         # Apply a specific stash without dropping it
git stash drop stash@{0}          # Delete a specific stash
git stash clear                   # Delete all stashes
```

---

### 8. Rebase a feature branch

**Scenario:**
Your team enforces a linear commit history. You have been working on `feature/payments` for two days. Meanwhile, three commits were merged into `main`. Before opening a pull request, you need your branch to sit cleanly on top of the latest `main`.

**Question:**
How do you rebase your feature branch onto the updated `main`?

**Answer:**

**Step 1 — Update your local main**
```bash
git checkout main
git pull origin main
```

**Step 2 — Rebase your feature branch**
```bash
git checkout feature/payments
git rebase main
```
Git replays your commits on top of the latest `main` one by one.

**Step 3 — Resolve any conflicts that arise**
If a conflict occurs during replay:
```bash
# Edit the conflicting file(s), then:
git add <file>
git rebase --continue
```
To cancel and go back to the original state:
```bash
git rebase --abort
```

**Step 4 — Force-push the rebased branch**
Because rebase rewrites commit history, you must force-push:
```bash
git push --force-with-lease origin feature/payments
```
`--force-with-lease` is safer than `--force` — it refuses to push if someone else pushed to the branch since your last fetch.

**Result:** Your branch history is now linear — `main` commits, then your commits, with no merge commit.

> **Important:** Never rebase commits that have already been pushed to a shared branch others are working on.

---

## Advanced

---

### 9. Cherry-pick a specific commit

**Scenario:**
A critical bug fix was committed to `hotfix/auth` with commit hash `a3f8c21`. You need that fix in `develop` immediately, but you cannot merge the entire `hotfix/auth` branch because it contains unrelated, unfinished work.

**Question:**
How do you apply only that specific commit to `develop`?

**Answer:**

**Step 1 — Find the commit hash**
```bash
git log hotfix/auth --oneline
```
Identify the commit: `a3f8c21 Fix null pointer in auth token validation`

**Step 2 — Switch to the target branch**
```bash
git checkout develop
```

**Step 3 — Cherry-pick the commit**
```bash
git cherry-pick a3f8c21
```
Git applies the changes from that commit and creates a **new commit** on `develop` with a new hash.

**If a conflict occurs:**
```bash
# Resolve the conflict in the file, then:
git add <file>
git cherry-pick --continue
```

To cancel:
```bash
git cherry-pick --abort
```

**Cherry-pick a range of commits:**
```bash
git cherry-pick a3f8c21..d9e1b04    # Apply all commits between the two hashes (exclusive of the first)
git cherry-pick a3f8c21^..d9e1b04   # Inclusive of the first commit
```

> **Note:** Cherry-picking duplicates commits. Prefer merging when possible to avoid diverging histories.

---

### 10. Recover from a detached HEAD

**Scenario:**
You ran `git checkout a3f8c21` to inspect an old commit. You then started making changes and even added a new commit. Now Git warns you are in a **detached HEAD** state and your new commit will be lost if you switch branches.

**Question:**
How do you preserve your work and continue development from this point?

**Answer:**

**Understanding the problem:**
In detached HEAD state, `HEAD` points directly to a commit rather than a branch. Any new commits you make are not attached to any branch and will eventually be garbage collected by Git.

**Solution — Create a new branch from your current position:**
```bash
git checkout -b feature/from-old-commit
```
or using the newer syntax:
```bash
git switch -c feature/from-old-commit
```

This creates a new branch pointing to your current commit (including any new commits you made in detached HEAD state), and attaches `HEAD` to it.

**Verify:**
```bash
git log --oneline        # Confirm your commits are on the new branch
git status               # Confirm HEAD is attached to a branch
```

**If you already switched away and lost the commit:**
```bash
git reflog               # Find the lost commit hash
git checkout -b recovery-branch <lost-commit-hash>
```

> **Key concept:** `git reflog` records every position HEAD has pointed to, even commits no longer reachable from any branch.

---

### 11. Reset vs revert on a shared branch

**Scenario:**
You pushed a faulty commit (`b7d3e91`) to the shared `main` branch. Teammates have already pulled. You need to undo it without rewriting history and without breaking their local repos.

**Question:**
Should you use `git reset` or `git revert`? Why? How do you do it safely?

**Answer:**

**Use `git revert` — never `git reset` on shared branches.**

**Why not `git reset`:**
`git reset` moves the branch pointer backwards, rewriting history. If teammates have already pulled `b7d3e91`, their histories will diverge from yours, causing major conflicts on their next push or pull.

**Safe solution with `git revert`:**

**Step 1 — Revert the faulty commit**
```bash
git revert b7d3e91
```
Git creates a **new commit** that undoes the changes introduced by `b7d3e91`. History is preserved — the faulty commit still exists, but its effects are neutralized.

**Step 2 — Push the revert commit**
```bash
git push origin main
```

**Step 3 — Notify teammates**
They simply run `git pull` and receive the revert commit cleanly.

**Comparison:**

| | `git reset` | `git revert` |
|---|---|---|
| Rewrites history | Yes | No |
| Safe for shared branches | No | Yes |
| Creates a new commit | No | Yes |
| Teammates affected | Yes (force-push needed) | No |

> **Rule:** On any branch others have pulled from, always use `git revert`.

---

### 12. Recover lost commits after a reset

**Scenario:**
You ran `git reset --hard HEAD~3` intending to undo the last commit, but you accidentally removed three important commits. The branch no longer shows those commits in `git log`.

**Question:**
How do you recover the lost commits?

**Answer:**

Git does not immediately delete commits — they remain in the object store until garbage collection runs. The key tool here is `git reflog`.

**Step 1 — View the reflog**
```bash
git reflog
```
Output example:
```
a1b2c3d HEAD@{0}: reset: moving to HEAD~3
d4e5f6g HEAD@{1}: commit: Add payment validation
h7i8j9k HEAD@{2}: commit: Add checkout page
l0m1n2o HEAD@{3}: commit: Add cart feature
```

The commits you lost are `d4e5f6g`, `h7i8j9k`, `l0m1n2o`.

**Step 2 — Reset back to the commit before the accidental reset**
```bash
git reset --hard d4e5f6g
```
This moves your branch pointer back to the last good commit, restoring all three.

**Alternative — Create a new branch from the lost commit:**
```bash
git checkout -b recovery-branch d4e5f6g
```

**Step 3 — Verify recovery**
```bash
git log --oneline
```

> **Key concept:** `git reflog` is your safety net. It records every move of HEAD, even across resets, rebases, and detached HEAD states. Entries expire after 90 days by default.

---

### 13. Enforce commit message standards with Git hooks

**Scenario:**
Your team has agreed that every commit message must start with a ticket number in the format `[PROJ-123]`. Developers keep forgetting this standard. You want to automatically reject commits that do not follow the format.

**Question:**
How do you set up a Git hook to enforce this rule?

**Answer:**

Git hooks are scripts that run automatically at specific points in the Git workflow. The `commit-msg` hook runs after the developer types a message but before the commit is finalized.

**Step 1 — Navigate to the hooks directory**
```bash
cd .git/hooks
```

**Step 2 — Create the `commit-msg` hook**
```bash
touch commit-msg
chmod +x commit-msg
```

**Step 3 — Write the validation script**
```bash
#!/bin/sh

COMMIT_MSG_FILE=$1
COMMIT_MSG=$(cat "$COMMIT_MSG_FILE")

PATTERN='^\[PROJ-[0-9]+\]'

if ! echo "$COMMIT_MSG" | grep -qE "$PATTERN"; then
  echo ""
  echo "ERROR: Commit message must start with a ticket number."
  echo "Expected format: [PROJ-123] Your message here"
  echo "Your message was: $COMMIT_MSG"
  echo ""
  exit 1
fi

exit 0
```

**Step 4 — Test it**

Failing commit:
```bash
git commit -m "Fix the login bug"
# ERROR: Commit message must start with a ticket number.
```

Passing commit:
```bash
git commit -m "[PROJ-456] Fix null pointer in login flow"
# [main a1b2c3d] [PROJ-456] Fix null pointer in login flow
```

**Sharing hooks with the team:**

The `.git/hooks` directory is not tracked by Git. To share hooks:

1. Create a `hooks/` folder in the repo root and add scripts there
2. Configure Git to use it:
```bash
git config core.hooksPath hooks
```
3. Commit the `hooks/` folder — everyone who clones the repo sets the same config

**Common Git hooks:**

| Hook | When it runs |
|---|---|
| `pre-commit` | Before the commit is created (e.g. run linter) |
| `commit-msg` | After message is written (e.g. validate format) |
| `pre-push` | Before pushing (e.g. run tests) |
| `post-merge` | After a merge (e.g. install dependencies) |

> **Tip:** Tools like [Husky](https://typicode.github.io/husky/) make managing Git hooks in Node.js projects much easier.

---

## Quick Reference

| Scenario | Key Command(s) |
|---|---|
| Initialize + connect to GitHub | `git init` → `git remote add origin` → `git push -u` |
| Stage specific files | `git add file1 file2` |
| Undo uncommitted file changes | `git restore <file>` |
| Clone a repo | `git clone <url>` |
| Resolve merge conflict | edit file → `git add` → `git commit` |
| Inspect remote without merging | `git fetch` + `git diff main origin/main` |
| Save work temporarily | `git stash` / `git stash pop` |
| Linear history before PR | `git rebase main` + `git push --force-with-lease` |
| Apply one commit to another branch | `git cherry-pick <hash>` |
| Save work from detached HEAD | `git checkout -b <new-branch>` |
| Safely undo a pushed commit | `git revert <hash>` + `git push` |
| Recover lost commits | `git reflog` → `git reset --hard <hash>` |
| Enforce commit message format | `.git/hooks/commit-msg` script |
