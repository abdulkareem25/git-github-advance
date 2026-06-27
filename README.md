# Git & GitHub Advanced Cheat Sheet

> A hands-on learning repository and **complete reference guide** for developers who want to use Git like a senior engineer — not just memorize commands, but understand *why* each one exists.

---

## Table of Contents

- [Setup](#-setup)
- [Basic Commands](#-basic-commands)
- [Undoing & Fixing](#-undoing--fixing-senior-dev-level)
- [Branching & Switching](#-branching--switching-modern-git)
- [GitHub & Remotes](#-github--remotes)
- [Advanced Optimization](#-advanced-optimization)
- [Typical Senior Workflow](#-typical-senior-workflow)
- [Repository Contents](#-repository-contents)

---

## ⚙️ Setup

Before you write a single commit, Git needs to know who you are. These settings are embedded in every commit you make.

| Command | Description |
|---|---|
| `git config --global user.name "Your Name"` | Set your display name globally across all repositories |
| `git config --global user.email "you@example.com"` | Set your email globally — must match your GitHub account for contribution graphs |
| `git config --list` | View all active configurations (global + local) |

> **💡 Pro Tip:** Use `git config --local` (without `--global`) inside a specific repo to override your global identity — useful when juggling work and personal accounts on the same machine.

---

## 📦 Basic Commands

The foundation of every Git workflow. Master these before anything else.

| Command | Description |
|---|---|
| `git init` | Initialize a new local Git repository in the current directory |
| `git status` | Show the state of your working directory and staging area — your most-used command |
| `git add [filename]` | Stage a specific file, giving you precise control over what goes into the next commit |
| `git add .` | Stage **all** changes in the current directory (new files, modifications, deletions) |
| `git commit -m "message"` | Create a snapshot of staged changes with a descriptive message |
| `git log --oneline` | View a compact, one-line-per-commit history — great for a quick orientation |
| `git diff` | Show line-by-line unstaged changes in your working directory |

> **💡 Pro Tip:** `git diff --staged` shows what's already staged and about to be committed. Use it as a final review before `git commit`.

---

## 🔧 Undoing & Fixing *(Senior Dev Level)*

This is where most developers get lost. Understanding these commands separates juniors from seniors. Always know **what state your changes are in** before running any of these.

```
Working Directory  →  Staging Area  →  Commit History
     (unstaged)          (staged)         (committed)
```

| Command | Description |
|---|---|
| `git restore --staged [filename]` | **Unstage** a file — moves it back to the working directory without losing your changes |
| `git restore [filename]` | **Discard** all unsaved changes to a file, reverting it to the state of the last commit ⚠️ *Irreversible* |
| `git commit --amend --no-edit` | Fold staged changes into your **last commit** without touching the commit message — perfect for fixing a typo you just noticed |
| `git reflog` | Your safety net. Shows a full local history of every `HEAD` movement — use this to recover commits lost after a bad reset or accidental branch deletion |
| `git reset --soft HEAD~1` | Undo the last commit, but keep all changes **staged** and ready to recommit |
| `git reset --hard HEAD~1` | Undo the last commit and **permanently wipe** all associated changes ⚠️ *Use with extreme caution* |
| `git revert [commit-id]` | The **safe, collaborative** way to undo. Creates a new commit that inverts a specific commit — does not rewrite history, ideal for shared branches |

> **💡 When to use `reset` vs `revert`:**
> - Use `git reset` only on **local, unpushed commits** — it rewrites history.
> - Use `git revert` on **pushed commits** — it's safe for team environments because it adds a new commit instead of erasing one.

---

## 🌿 Branching & Switching *(Modern Git)*

Branches are cheap and fast in Git. A senior developer creates a new branch for *every* feature or fix — no exceptions.

| Command | Description |
|---|---|
| `git branch` | List all local branches; the active branch is highlighted with `*` |
| `git switch [branch-name]` | Switch to an existing branch — the modern, explicit replacement for `git checkout` |
| `git switch -c [branch-name]` | **Create and switch** to a new branch in one step — the modern replacement for `git checkout -b` |
| `git merge [branch-name]` | Merge the specified branch into your **current** branch |
| `git branch -d [branch-name]` | Delete a branch **safely** — Git will refuse if the branch hasn't been fully merged |
| `git cherry-pick [commit-id]` | Pluck a single specific commit from any branch and apply it to your current branch — surgical and precise |

> **💡 Pro Tip:** Prefer `git switch` over `git checkout` for all branch operations. `checkout` is overloaded with too many responsibilities (switching branches AND restoring files), making it error-prone. `switch` is clear and unambiguous.

---

## ☁️ GitHub & Remotes

Connecting your local repository to the outside world.

| Command | Description |
|---|---|
| `git clone [url]` | Download a full copy of a remote repository, including its entire history |
| `git remote add origin [url]` | Link your local repository to a remote (usually called `origin`) for the first time |
| `git remote -v` | Verify your remote connections — always check this when something seems off with push/pull |
| `git push -u origin [branch-name]` | Push a branch to the remote **and** set up upstream tracking so future `git push`/`git pull` work without arguments |
| `git push --force-with-lease` | Force push **safely** — it checks if the remote has commits you haven't seen yet, preventing you from silently overwriting a teammate's work |
| `git pull` | Fetch latest changes from the remote and immediately merge them into your current branch |
| `git fetch --prune` | Fetch remote state and **clean up** stale local references to branches that were deleted on the remote |

> **💡 `--force-with-lease` vs `--force`:**
> Never use `git push --force` on a shared branch. `--force-with-lease` is the professional alternative — it acts as a force push but aborts if the remote has been updated since your last fetch, protecting your teammates' work.

---

## ⚡ Advanced Optimization

Tools that dramatically improve your workflow once you're comfortable with the basics.

| Command | Description |
|---|---|
| `git stash` | Temporarily shelve your current uncommitted changes so you can switch context without losing work |
| `git stash pop` | Re-apply your most recently stashed changes and remove them from the stash list |
| `git stash list` | View all stashed changesets — each entry is labelled with its branch and last commit |
| `git rebase -i HEAD~[n]` | Interactively rebase the last `n` commits — squash noisy WIP commits, reword messages, or reorder history before opening a PR |

> **💡 Interactive Rebase in Practice:**
> Running `git rebase -i HEAD~3` opens an editor where you can `pick`, `squash`, `reword`, or `drop` the last 3 commits. This is how senior devs deliver a clean, readable commit history on their pull requests — making the reviewer's job much easier.

---

## 🏆 Typical Senior Workflow

A repeatable, professional workflow for every feature or bug fix. Follow this pattern consistently and your Git history will always be clean.

```bash
# 1. Always start from the latest version of main
git pull

# 2. Create an isolated branch for your work — never commit directly to main
git switch -c feature/your-feature-name

# 3. ... make your changes, test them thoroughly ...

# 4. Stage your clean, intentional changes
git add .

# 5. Commit with a clear, conventional message
#    Format: <type>: <short description>
#    Types: feat | fix | refactor | docs | chore | test
git commit -m "feat: add user authentication via OAuth2"

# 6. Push your branch and set up upstream tracking
git push -u origin feature/your-feature-name

# 7. ... open a Pull Request on GitHub, get it reviewed, then merge it ...

# 8. Return to main
git switch main

# 9. Pull the freshly merged code
git pull

# 10. Delete your local feature branch — the work is done
git branch -d feature/your-feature-name

# 11. Prune any stale remote-tracking references for a spotless local state
git fetch --prune
```

> **💡 Conventional Commits** (`feat:`, `fix:`, `docs:`, etc.) are a widely adopted standard that makes `git log` readable, enables automated changelogs, and signals professionalism in open-source and team settings. Adopt them early.

---

## 📁 Repository Contents

This repository is intentionally kept simple so the focus stays on Git and GitHub mastery, not application code. Use it as a sandbox for practising everything above.

| File | Purpose |
|---|---|
| `a.txt`, `b.txt`, `c.txt` | Sample files for basic change tracking and staging practice |
| `advanced.txt` | Notes and experiments for advanced Git/GitHub workflows |
| `addText.txt` | Demonstrates staged content additions |
| `learning.txt` | Reference material and notes on Git/GitHub concepts |
| `navbar.txt`, `sidebar.txt`, `footer.txt` | Example content fragments for branching and merge practice |

---

<div align="center">

**The best way to learn Git is to use it. Break things. Fix them. Repeat.**

*Built for developers who want to go beyond `add`, `commit`, `push`.*

</div>
