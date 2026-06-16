# Lab 3 — Local Meets Remote

**Module 3: GitHub Fundamentals** · ⏱️ ~25 minutes

---

## 🎯 Goal

By the end of this lab you will be able to:

- Create a repository on GitHub and connect your local repo to it
- Push commits to GitHub (`git push`) and pull changes down (`git pull`)
- Clone an existing repository (`git clone`)
- Explain the difference between `git init` + connect vs. `git clone`

## 🧠 Why this matters

Until now your work lived only on one machine — one spilled coffee away from oblivion. Pushing to GitHub gives you a backup, but far more importantly it's *how teams share code*. Every job you'll have follows this loop: pull your teammates' latest work, make changes, push yours back. Push/pull/clone are the bridge between "I can use Git" and "I can work on a team."

## 🗺️ What to expect

You'll publish your `engineering-journal` repo to GitHub, simulate a teammate's change using GitHub's web editor, pull that change down, and finally clone a fresh copy the way you would on day one at a new job. You'll authenticate once — that part has the most "it depends" of the day, so the lab covers both Codespaces (easiest) and local.

---

## Setup & authentication

```bash
cd ~/engineering-journal
git status   # should be clean
```

**Authentication, by environment:**

- **Codespaces:** you're already signed in to GitHub — pushes just work, *if* the repo you push to is one you created. ✨ Nothing to do.
- **Local machine:** the first time you push, Git will prompt you to sign in. On Windows/macOS, **Git Credential Manager** pops up a browser window — sign in and you're done forever. If you have the GitHub CLI, `gh auth login` also works. (GitHub removed password authentication in 2021; the browser flow handles tokens for you.)

---

## Part 1 — Follow along: publish your repo

### Step 1 — Create an empty repo on GitHub

1. In your browser: **github.com → + (top right) → New repository**.
2. Name: `engineering-journal`.
3. Visibility: **Public** (fine for today; Private also works).
4. ⚠️ **Leave every checkbox unchecked** — no README, no .gitignore, no license. We need it *empty* because our local repo already has history; initializing on both ends creates two unrelated histories that conflict.
5. Click **Create repository**.

GitHub now shows a page of setup commands. We'll use the *"push an existing repository"* block — typed by hand, so you understand each line.

### Step 2 — Tell your local repo where the remote is

Replace `YOUR-USERNAME` with your GitHub username:

```bash
git remote add origin https://github.com/YOUR-USERNAME/engineering-journal.git
git remote -v
```

- `remote add` registers a remote repository under a nickname
- `origin` is that nickname — the universal convention for "the main remote." It's not magic, just a default name.
- `git remote -v` verifies it (you'll see `fetch` and `push` URLs)

### Step 3 — Push

```bash
git push -u origin main
```

Breakdown: push the `main` branch to the remote nicknamed `origin`. The `-u` flag links your local `main` to the remote's `main`, so from now on plain `git push` and `git pull` know where to go. You only need `-u` on the *first* push of a branch.

### Step 4 — Verify on GitHub

Refresh the repo page in your browser. Your files are there, your README renders, and the commit count matches `git log --oneline | wc -l`. Click the commit count to see your full history — same hashes as your terminal.

✅ **Checkpoint:** Your commits are visible on github.com.

---

## Part 2 — Follow along: pull a "teammate's" change

Your repo now exists in **two places**, and they can drift apart. Let's make that happen deliberately.

### Step 5 — Edit on GitHub (you're playing the teammate)

1. On your repo page, click `README.md` → pencil icon (✏️ **Edit this file**).
2. Add a line at the bottom: `Day 1 (remote): edited directly on GitHub!`
3. Click **Commit changes...**, keep the default message, commit directly to `main`.

GitHub's web editor just made a commit that **your laptop knows nothing about**.

### Step 6 — Prove your local copy is behind

```bash
git log --oneline    # the GitHub edit is NOT here
cat README.md        # no new line
```

### Step 7 — Pull

```bash
git pull
git log --oneline    # there it is
cat README.md        # new line present
```

> 💡 `git pull` = *fetch* the new commits from the remote + *merge* them into your current branch. **Real-world habit: pull before you start working, every time.** It prevents most conflicts before they happen.

✅ **Checkpoint:** The web-edit commit appears in your local `git log`.

---

## Part 3 — Follow along: clone

`clone` is how you get a repo you *don't* already have — your first day on any team starts with it.

### Step 8 — Clone a copy elsewhere

```bash
cd ~
git clone https://github.com/YOUR-USERNAME/engineering-journal.git journal-clone
cd journal-clone
git log --oneline
git remote -v
```

Notice what `clone` did automatically: downloaded the **entire history** (not just latest files), set up the `origin` remote, and checked out `main`. That's why cloning is one command while init-and-connect was three — clone *is* init + remote add + pull, bundled.

✅ **Checkpoint:** `journal-clone` has identical history to `engineering-journal`.

---

## 🏆 Challenge — on your own (~8 min)

You now have **two local copies** of the same remote repo — a perfect simulation of two teammates. Use it:

1. In `journal-clone`, create `clone-note.md` with one sentence in it, commit it, and push. (Will you need `-u`? Find out.)
2. Switch to the original `engineering-journal` folder and get that commit onto it *without* using the browser.
3. From `engineering-journal`, append a line to `clone-note.md`, commit, push.
4. Update `journal-clone` so both copies match. Verify with `git log --oneline` in both.
5. **Bonus 1:** In either copy, run `git fetch` followed by `git status` after the *other* copy has pushed something new. What does `git status` say that it never said before? What does this tell you about the difference between `fetch` and `pull`?
6. **Bonus 2:** What happens if you `git push` when the remote has commits you don't have yet? Engineer the situation and read the error message — it's one of the most common errors in daily Git life, and you'll meet it again.

<details>
<summary>💡 Hint 1 — pushing from the clone</summary>

Clone set up tracking automatically, so plain `git push` works — no `-u` needed. (`-u` is only needed when *you* created the link by hand, like in Part 1.)
</details>

<details>
<summary>💡 Hint 2 — moving changes between the copies</summary>

The two folders never talk to each other directly. Everything goes *through* GitHub: push from one, pull into the other.
</details>

<details>
<summary>💡 Hint 3 — bonus 2 setup</summary>

Push a commit from copy A. Then in copy B (now behind), make a *different* commit and try to push without pulling first. Read the rejection carefully — Git tells you exactly what to do.
</details>

---

## 🧾 What you learned

| Command | What it does |
|---|---|
| `git remote add origin <url>` | Connect a local repo to a remote |
| `git remote -v` | List configured remotes |
| `git push -u origin main` | First push of a branch (sets up tracking) |
| `git push` / `git pull` | Send / receive commits after tracking is set |
| `git clone <url>` | Copy an entire remote repo, history and all |
| `git fetch` | Download remote commits *without* merging (look before you leap) |

**Key mental model:** local and remote are separate repos that only sync when *you* say so — `push` sends, `pull` receives, nothing is automatic.
