---
title: "Git & GitHub Fundamentals Handout"
course: "Expeditors Engineering Kickoff Day"
audience: "New Coding/DevOps bootcamp students"
type: "learner handout"
---

# Git & GitHub Fundamentals Handout

## What You Should Remember

Git saves snapshots of a project over time. GitHub hosts Git repositories online so people can back up, share, review, and collaborate on code.

Git and GitHub are related, but they are not the same thing:

| Tool | What it is | Where it runs |
|---|---|---|
| Git | Version control system | Your machine or Codespace |
| GitHub | Repository hosting and collaboration platform | The web |

Your first habit should be simple: when you are unsure, run `git status` and read what Git tells you.

## The Three Areas

Every Git change moves through three areas.

| Area | Meaning | Common command |
|---|---|---|
| Working directory | Files as you are editing them | `git status`, `git diff` |
| Staging area | Changes selected for the next commit | `git add <file>` |
| Repository | Permanent committed history | `git commit -m "message"` |

The flow is:

```text
edit files -> git add -> git commit
working directory -> staging area -> repository
```

The staging area matters because it lets you choose exactly what belongs in the next snapshot. You can edit five files and commit only the two that belong together.

## Daily Workflow Checklist

Use this loop for normal work:

1. Edit a file.
2. Run `git status`.
3. Review unstaged changes with `git diff`.
4. Stage the right changes with `git add <file>` or `git add .`.
5. Review staged changes with `git diff --staged`.
6. Commit with `git commit -m "Clear message"`.
7. Run `git status` again.

Good commit messages are short, specific, and written in the imperative:

```text
Add README setup steps
Fix rounding error in invoice total
Ignore generated log files
```

Avoid messages like:

```text
stuff
changes
fixed it
final commit
```

## Local and Remote Repositories

Your local repository and GitHub repository are separate copies. They do not sync automatically.

| Command | Meaning |
|---|---|
| `git remote add origin <url>` | Connect a local repo to a GitHub repo |
| `git remote -v` | Show configured remotes |
| `git push -u origin main` | First push of local `main` to GitHub |
| `git push` | Send local commits to GitHub |
| `git pull` | Bring remote commits down and merge them |
| `git clone <url>` | Copy an existing GitHub repo locally |
| `git fetch` | Download remote updates without merging them |

Use `git init` when the project starts on your machine. Use `git clone` when the project already exists somewhere else.

## Branches and Merges

A branch is a movable pointer to a commit. It is not a copy of the whole project.

| Command | Meaning |
|---|---|
| `git branch` | List branches |
| `git switch -c <branch>` | Create and switch to a new branch |
| `git switch <branch>` | Switch to an existing branch |
| `git merge <branch>` | Merge that branch into your current branch |
| `git branch -d <branch>` | Delete a merged branch |

Merge rule:

```text
Switch to the branch that should receive the changes, then merge.
```

Example:

```bash
git switch main
git merge add-standup-notes
```

## Merge Conflicts

A merge conflict happens when two branches changed the same lines and Git cannot safely choose for you.

Conflict markers look like this:

```text
<<<<<<< HEAD
# Team Engineering Journal
=======
# My Engineering Journal
>>>>>>> update-title
```

Resolve the conflict by:

1. Opening the file.
2. Keeping the correct version, or writing a better combined version.
3. Deleting the marker lines.
4. Running `git add <file>`.
5. Committing the resolution.

A conflict is not a disaster. It is Git asking you to make a decision.

## Files You Should Not Commit

Use `.gitignore` for files that should stay on your machine and out of Git history.

Common examples:

```gitignore
*.log
.env
build/
node_modules/
tmp/
*.tmp
```

Important: `.gitignore` only affects untracked files. If a secret was already committed and pushed, adding it to `.gitignore` does not remove it from history. The secret must be rotated.

## When You Are Stuck

Run:

```bash
git status
```

Then ask:

| What Git says | What it usually means |
|---|---|
| `not a git repository` | You are in the wrong folder, or you have not run `git init` |
| `Changes not staged for commit` | Edit exists, but is not staged |
| `Changes to be committed` | Change is staged and ready to commit |
| `Your branch is behind` | GitHub has commits you do not have locally |
| `fetch first` | Pull remote work before pushing |
| `both modified` | You are in a merge conflict |

## After-Class Practice

1. Create a new folder and initialize it with Git.
2. Make three small commits to a README.
3. Add a `.gitignore` file.
4. Create a branch, add a file, merge it into `main`, and delete the branch.
5. Push the repository to GitHub.

The goal is not speed. The goal is to know where every change lives.
