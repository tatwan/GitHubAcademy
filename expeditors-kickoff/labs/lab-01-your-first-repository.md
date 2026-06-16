# Lab 1 — Your First Repository

**Module 1: The Git Mental Model** · ⏱️ ~20 minutes

---

## 🎯 Goal

By the end of this lab you will be able to:

- Turn an ordinary folder into a Git repository with `git init`
- Move a change through all three areas: **Working Directory → Staging Area → Repository**
- Read the output of `git status` and explain what state your files are in
- View your commit history with `git log`

## 🧠 Why this matters

Every professional engineering team uses version control, and ~93% of developers use Git specifically. The cycle you practice here — *check status, stage, commit* — is the single most-repeated workflow in software engineering. You will run these three commands thousands of times in your career. Today you run them for the first time, slowly and deliberately, so the mental model sticks.

## 🗺️ What to expect

This lab is **entirely local** — nothing touches GitHub yet. You'll create a tiny "engineering journal" project and make your first two commits. If a command's output looks different from the lab, that's a learning moment: run `git status` and read what it tells you. Git's messages are genuinely helpful.

---

## Environment setup

### Option A — GitHub Codespaces (recommended)

1. Go to [github.com/codespaces](https://github.com/codespaces) and click **Use this template → Blank** (or open the blank template directly at [github.com/codespaces/new?template=blank](https://github.com/codespaces/new)).
2. Wait for the editor to load in your browser (~30 seconds).
3. Open the terminal: menu (☰) → **Terminal → New Terminal**, or press `` Ctrl+` ``.

> A Codespace is a complete Linux dev machine running in your browser. Git is pre-installed. Free accounts include 120 core-hours/month — plenty for today.

### Option B — Local machine

Open your terminal (macOS Terminal, Windows **Git Bash**, or WSL) and verify Git is installed:

```bash
git --version
```

You should see `git version 2.x.x`. Any version 2.23+ is fine for today.

### One-time identity setup (both options)

Git stamps every commit with an author. Tell Git who you are (Codespaces sometimes pre-fills this; running it again is harmless):

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

> 💡 `--global` means "for every repo on this machine." The third line makes new repos start on a branch named `main` — the modern standard.

---

## Part 1 — Follow along: create a repository

### Step 1 — Create a project folder

```bash
cd ~
mkdir engineering-journal
cd engineering-journal
```

### Step 2 — Look before you leap

```bash
git status
```

You should see an **error**: `fatal: not a git repository`. Good! This proves Git only works inside folders you've explicitly told it to track. Git never watches your whole computer.

### Step 3 — Initialize the repository

```bash
git init
```

Output: `Initialized empty Git repository in .../engineering-journal/.git/`

That hidden `.git/` folder **is** the repository — the database where Git will store every version of every file. See it for yourself:

```bash
ls -a
```

> ⚠️ Never delete or edit `.git/` by hand. Deleting it deletes your project's entire history.

### Step 4 — Check status again

```bash
git status
```

```
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

✅ **Checkpoint:** You see `On branch main` and `No commits yet`.

### Step 5 — Create your first file

```bash
echo "# Engineering Journal" > README.md
echo "Day 1: Learned the Git mental model." >> README.md
git status
```

Now `README.md` appears in red under **Untracked files**. Untracked = "Git sees this file exists but is not managing it yet."

### Step 6 — Stage the file (Working Directory → Staging Area)

```bash
git add README.md
git status
```

`README.md` is now green under **Changes to be committed**. The staging area is your "shopping cart" — you've placed the item in the cart but haven't checked out.

### Step 7 — Commit (Staging Area → Repository)

```bash
git commit -m "Add README with day 1 journal entry"
```

Output like: `[main (root-commit) a1b2c3d] Add README with day 1 journal entry`

That `a1b2c3d` is the short **commit hash** — a unique ID for this exact snapshot. Yours will differ; every commit hash in the world is unique.

### Step 8 — Verify

```bash
git status
git log
```

`git status` says `nothing to commit, working tree clean` — all three areas are in sync. `git log` shows your commit with hash, author, date, and message. (If `git log` opens a pager, press `q` to quit.)

✅ **Checkpoint:** `git log` shows exactly 1 commit authored by you.

---

## Part 2 — Follow along: the loop, one more time

### Step 9 — Edit and add

```bash
echo "Day 1 (cont.): Made my first commit!" >> README.md
echo "- git status, git add, git commit" > commands-learned.txt
git status
```

Notice Git distinguishes the two situations: `README.md` is **modified** (tracked, but changed since last commit) while `commands-learned.txt` is **untracked** (brand new).

### Step 10 — Stage everything and commit

```bash
git add .
git status
git commit -m "Add commands cheat sheet and update journal"
git log --oneline
```

> 💡 `git add .` stages *all* changes in the current folder. Convenient, but always run `git status` first so you know what you're about to stage.

✅ **Checkpoint:** `git log --oneline` shows **2 commits**, newest on top.

---

## 🏆 Challenge — on your own (~5 min)

No copy-paste this time. Using only what you've practiced:

1. Create a new file called `goals.md` containing three learning goals for this bootcamp (one per line).
2. Stage **only** `goals.md` (not with `git add .`).
3. Commit it with a clear, descriptive message.
4. Prove your history now has **3 commits**, displayed one-per-line.
5. **Bonus:** Find the full (40-character) hash of your *first* commit.

<details>
<summary>💡 Hint 1 — creating a multi-line file</summary>

Either open the file in the editor (in Codespaces just click **New File**), or use `echo` three times with `>>` to append. Remember: `>` overwrites, `>>` appends.
</details>

<details>
<summary>💡 Hint 2 — staging one specific file</summary>

`git add` takes a filename, not just `.`
</details>

<details>
<summary>💡 Hint 3 — compact history</summary>

You used the flag already in Step 10. For the full hash of the first commit, run `git log` *without* that flag and scroll to the bottom (oldest commit).
</details>

---

## 🧾 What you learned

| Command | What it does |
|---|---|
| `git init` | Turns the current folder into a Git repository |
| `git status` | Shows the state of all three areas — **run it constantly** |
| `git add <file>` / `git add .` | Moves changes into the staging area |
| `git commit -m "msg"` | Saves a permanent snapshot to the repository |
| `git log` / `git log --oneline` | Shows commit history |

**Key mental model:** changes flow one way — Working Directory → (`git add`) → Staging Area → (`git commit`) → Repository.
