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

You'll publish your `engineering-journal` repo to GitHub, simulate a teammate's change using GitHub's web editor, pull that change down, and finally clone a fresh copy the way you would on day one at a new job.

**⚠️ Important note on Codespaces authentication:** If you started with a **Blank** Codespace (as instructed in Lab 1), you will likely hit a permission error on your first push. This is a very common gotcha caused by GitHub scoping the temporary token. The rest of this lab gives you two clear, explicit paths so you never have to guess which commands to run.

---

## Part 1 — Publish your repo (choose one path)

You must pick **one** of the two paths below and follow its steps from start to finish.

Duplication in the instructions is intentional — it removes mental mapping and guesswork.

### Why we have two options

| Aspect                  | Option A (Recommended)                                      | Option B (Stay & Workaround)                                   |
|-------------------------|-------------------------------------------------------------|----------------------------------------------------------------|
| **Environment**         | Switch to (or start) a new Codespace created from your repo | Stay in the exact same blank Codespace from Labs 1 and 2       |
| **Authentication**      | "Just works" — GitHub gives the Codespace proper permissions | Requires creating a PAT + bypassing the credential helper      |
| **What you learn**      | The normal real-world flow teams use every day              | A very common Codespaces gotcha + how to diagnose and fix it   |
| **Future work**         | Smoother for the rest of the day and beyond                 | You may want to create a proper Codespace later                |
| **Best for**            | Least friction today + good habits                          | Understanding auth issues deeply or not wanting to switch tabs |

**Choose now**, then follow only the steps under your chosen option until you reach the ✅ checkpoint for publishing. After that, Part 2 and Part 3 are the same for everyone.

---

### Option A — Recommended: Create a Codespace from Your GitHub Repo

**Goal:** Use a Codespace that is correctly attached to your repository from the moment it is created. This is the experience you will have in real jobs.

In this path:
- You create the repo on GitHub (you can initialize it with a README).
- You launch a **new** Codespace directly from that repo.
- The remote is usually already set up correctly.
- You bring your work across and push.

**Step A1 — Create the repository on GitHub**

1. In your browser: **github.com → + (top right) → New repository**.
2. Name: `engineering-journal`.
3. Visibility: **Public** (Private is also fine).
4. **For Option A you have flexibility here:**
   - **Easiest start:** Check **"Add a README file"**. GitHub will give the repo an initial commit and a basic README. Your new Codespace will start with this already present.
   - Leave the other boxes unchecked.
5. Click **Create repository**.

**Step A2 — Launch a Codespace from this repo (the key step)**

1. On the repository page you just created, click the green **Code** button.
2. Click the **Codespaces** tab.
3. Click **Create codespace on main**.
4. Wait ~30 seconds for the new Codespace to load in your browser.

This new Codespace has the correct GitHub token with write access to `engineering-journal`.

**Step A3 — Open a terminal and inspect the connection**

In the new Codespace:

```bash
# Codespaces often opens you inside the repo folder already
pwd
ls -a
git remote -v
git status
git log --oneline
```

You should see:
- `origin` already points to your `engineering-journal` repo.
- The repo may have the initial README commit from GitHub (if you chose that).

**Step A4 — Bring your existing work into this Codespace**

Your Lab 1 + Lab 2 work is still in your previous Codespace (usually at `~/engineering-journal`).

Easy ways to move it:

**Using the file explorer (often simplest):**
- In the old Codespace, select the files/folders you want (README.md, any .md files, greet.py, etc.).
- Copy them.
- In the new Codespace, paste them into the main folder (avoid overwriting the `.git` folder).

**Or using the terminal (in the new Codespace):**

```bash
# Copy content from your old location (adjust path if needed)
cp -r ~/engineering-journal/* . 2>/dev/null || true
cp -r ~/engineering-journal/.* . 2>/dev/null || true   # include dotfiles if any

ls
```

**Step A5 — Commit and push**

```bash
git status
git add .
git commit -m "Add engineering journal content from Labs 1-2"
git push -u origin main
```

If you chose to initialize with a README on GitHub, this will add your commits on top.

**Step A6 — Verify on GitHub**

Refresh the repo page. Your files and commits should be visible.

✅ **Checkpoint (Option A):** Your repository is published and you are working in a Codespace that has proper permissions for it.

---

### Option B — Stay in Your Current Codespace (PAT + Credential Bypass)

**Goal:** Publish your existing local repository from the blank Codespace you have been using since Lab 1, while learning how to handle the common permission issue.

In this path:
- You create an **empty** repo on GitHub (no initial commit).
- You stay in your current terminal.
- You use a fine-grained Personal Access Token and tell Git to ignore the scoped Codespaces helper for the push.

**Step B1 — Create an empty repository on GitHub**

1. In your browser: **github.com → + (top right) → New repository**.
2. Name: `engineering-journal`.
3. Visibility: **Public** (Private also works).
4. ⚠️ **Leave every checkbox unchecked** — no README, no .gitignore, no license.  
   We need it *empty* because our local repo already has history.
5. Click **Create repository**.

GitHub now shows setup commands. We will do this by hand.

**Step B2 — Create a fine-grained Personal Access Token**

1. Go to GitHub → click your profile picture → **Settings** → **Developer settings** (left sidebar) → **Personal access tokens** → **Fine-grained tokens**.
2. Click **Generate new token**.
3. Token name: `codespaces-engineering-journal`
4. **Resource owner**: Select your username.
5. **Repository access**: Choose **Only select repositories** → select `engineering-journal`.
6. **Permissions**:
   - Under **Repository permissions**, find **Contents** → set to **Read and write**.
7. Click **Generate token** and **copy the token immediately** (you will not see it again).

**Step B3 — Tell your local repo where the remote is (using the PAT)**

In your **current** Codespace terminal:

```bash
cd ~/engineering-journal
git status   # should still be clean
```

Replace `YOUR-USERNAME` and `YOUR_PAT_HERE` with your actual values:

```bash
git remote remove origin 2>/dev/null || true

git remote add origin https://YOUR-USERNAME:YOUR_PAT_HERE@github.com/YOUR-USERNAME/engineering-journal.git

git remote -v
```

**Step B4 — Push using the bypass**

```bash
git -c credential.helper= push -u origin main
```

The `-c credential.helper=` tells Git "do not use any credential helper for this command" so it uses the username + PAT that is embedded in the URL.

If it still complains, run the same command again.

**Step B5 — Verify on GitHub**

Refresh the repo page in your browser. Your files are there and the commit count should match what you have locally.

```bash
git log --oneline | head -5
```

✅ **Checkpoint (Option B):** Your commits are visible on github.com and you successfully worked around the blank Codespace token limitation.

---

## Continue here (both Option A and Option B)

**Excellent!** Whether you followed the Option A path (new Codespace from the repo) or Option B path (PAT in your current Codespace), your `engineering-journal` repository is now published on GitHub.

### Quick orientation — do this now

Run these commands in the terminal of the environment where you just pushed:

```bash
pwd
git status
git remote -v
git log --oneline -3
```

**Important for Option A students:**  
Codespaces created from a repo often put you in `/workspaces/engineering-journal` (or similar), not `~/engineering-journal`. Use `cd` to go to the correct folder before the next commands.

**Important for Option B students:**  
You are still in your original `~/engineering-journal`.

From this point on the instructions are the same for everyone.

**From now on you should be able to use normal `git push` and `git pull`.** If a push ever fails with 403 again, use:

```bash
git -c credential.helper= push
```

---

## Part 2 — Follow along: pull a "teammate's" change

Your repo now exists in **two places**, and they can drift apart. Let's make that happen deliberately.

### Step 1 — Edit on GitHub (you're playing the teammate)

1. On your repo page, click `README.md` → pencil icon (✏️ **Edit this file**).
2. Add a line at the bottom: `Day 1 (remote): edited directly on GitHub!`
3. Click **Commit changes...**, keep the default message, commit directly to `main`.

GitHub's web editor just made a commit that **your laptop knows nothing about**.

### Step 2 — Prove your local copy is behind

```bash
git log --oneline    # the GitHub edit is NOT here
cat README.md        # no new line
```

### Step 3 — Pull

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

### Working directory reminder (important)

After publishing, use the same folder you were in when you completed your chosen option (Option A or B).

```bash
pwd
git remote -v
```

Option A students: often `/workspaces/engineering-journal` or similar.  
Option B students: usually `~/engineering-journal`.

```bash
cd /path/to/your/engineering-journal
git remote -v
```

### Step 4 — Clone a copy elsewhere

From your home directory (or any convenient location):

```bash
cd ~
git clone https://github.com/YOUR-USERNAME/engineering-journal.git journal-clone
cd journal-clone
git log --oneline
git remote -v
```

(If you prefer SSH and have it set up: `git clone git@github.com:YOUR-USERNAME/engineering-journal.git journal-clone`)

Notice what `clone` did automatically: downloaded the **entire history** (not just latest files), set up the `origin` remote, and checked out `main`. That's why cloning is one command while init-and-connect was three — clone *is* init + remote add + pull, bundled.

✅ **Checkpoint:** `journal-clone` has identical history to `engineering-journal`.

---

## 🏆 Challenge — on your own (~8 min)

You now have **two local copies** of the same remote repo — a perfect simulation of two teammates. Use it:

1. In `journal-clone`, create `clone-note.md` with one sentence in it, commit it, and push. (Will you need `-u`? Find out.)
2. Switch to the original `engineering-journal` folder and get that commit onto it *without* using the browser.
3. From `engineering-journal`, append a line to `clone-note.md`, commit, push.
4. Update `journal-clone` so both copies match. Verify with `git log --oneline` in both.

**If any push in the challenge fails with a 403**, use the bypass:

```bash
git -c credential.helper= push
```

(After the very first successful push of the repo, this is rarely needed again. Option A students almost never need it.)
5. **Bonus 1:** In either copy, run `git fetch` followed by `git status` after the *other* copy has pushed something new. What does `git status` say that it never said before? What does this tell you about the difference between `fetch` and `pull`?
6. **Bonus 2:** What happens if you `git push` when the remote has commits you don't have yet? Engineer the situation and read the error message — it's one of the most common errors in daily Git life, and you'll meet it again.

<details>
<summary>💡 Hint 1 — pushing from the clone</summary>

Clone set up tracking automatically, so plain `git push` works — no `-u` needed. (`-u` is only needed when *you* created the link by hand with `git remote add` — common for students who followed Option B. Many Option A students never had to do this step.)
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

**Note:** Depending on the path you chose, you may have seen `git remote -v` already populated by Codespaces (Option A) instead of typing `git remote add` yourself. The concepts below are the same.

| Command | What it does |
|---|---|
| `git remote add origin <url>` | Connect a local repo to a remote |
| `git remote -v` | List configured remotes |
| `git push -u origin main` | First push of a branch (sets up tracking) |
| `git push` / `git pull` | Send / receive commits after tracking is set |
| `git clone <url>` | Copy an entire remote repo, history and all |
| `git fetch` | Download remote commits *without* merging (look before you leap) |

**Key mental model:** local and remote are separate repos that only sync when *you* say so — `push` sends, `pull` receives, nothing is automatic.
