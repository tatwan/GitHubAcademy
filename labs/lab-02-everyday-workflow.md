# Lab 2 — The Edit → Stage → Commit Loop

**Module 2: Everyday Git Workflow** · ⏱️ ~25 minutes

---

## 🎯 Goal

By the end of this lab you will be able to:

- Make a series of small, focused commits with good messages
- Use `git diff` to see exactly what changed — and `git diff --staged` to review before committing
- Navigate history with `git log` and its most useful flags
- Use `.gitignore` to keep junk out of your repository

## 🧠 Why this matters

Professional engineers don't commit once a week — they commit several times an hour, in small focused units. Small commits make bugs easy to find ("which commit broke it?"), make code review possible, and create a readable story of the project. `git diff` is your pre-flight check: reviewing your own changes before committing is the habit that separates careful engineers from chaotic ones. And every real project has files that *must not* be committed (passwords, build output, 500MB of `node_modules`) — that's what `.gitignore` is for.

## 🗺️ What to expect

You'll continue in the `engineering-journal` repo from Lab 1 (or quickly recreate it). You'll build a tiny Python script across several commits, inspect diffs at each stage, then deliberately create "junk" files and teach Git to ignore them.

---

## Setup

Continue in your repo from Lab 1:

```bash
cd ~/engineering-journal
git status
```

<details>
<summary>😅 Lost your Lab 1 repo? Quick rebuild</summary>

```bash
cd ~ && mkdir engineering-journal && cd engineering-journal
git init
echo "# Engineering Journal" > README.md
git add . && git commit -m "Add README"
```
</details>

---

## Part 1 — Follow along: diffs tell the truth

### Step 1 — Create a small script

```bash
cat > greet.py << 'EOF'
def greet(name):
    return f"Hello, {name}!"

print(greet("Expeditors"))
EOF
git add greet.py
git commit -m "Add greeting script"
```

### Step 2 — Modify it and ask Git what changed

Edit `greet.py` (click it in the Codespaces editor, or use the heredoc below) so it greets in uppercase:

```bash
cat > greet.py << 'EOF'
def greet(name):
    return f"Hello, {name.upper()}!"

print(greet("Expeditors"))
EOF
```

Now, **before** staging anything:

```bash
git diff
```

Read the output carefully:

- Lines starting with `-` (red) = removed
- Lines starting with `+` (green) = added
- A changed line shows as one `-` and one `+`

> 💡 `git diff` with no arguments answers: *"What have I changed in my working directory that I haven't staged yet?"* (Press `q` if it opens a pager.)

### Step 3 — Stage it, then diff again

```bash
git add greet.py
git diff
```

**Empty output!** Surprised? `git diff` compares working directory ↔ staging area, and they now match. To see what's staged and about to be committed:

```bash
git diff --staged
```

There's your change. This is the command to run as a final review **right before every commit**.

✅ **Checkpoint:** `git diff` shows nothing; `git diff --staged` shows the `.upper()` change.

### Step 4 — Commit with a good message

```bash
git commit -m "Make greeting uppercase for emphasis"
```

**Commit message rules of thumb:**

- Imperative mood: "Add login page", not "Added" or "Adds"
- Say *what and why*, not "fixed stuff" or "changes"
- Keep the first line under ~50 characters

### Step 5 — Explore history

```bash
git log --oneline
git log --oneline --graph --decorate
git log -p -1
```

- `--oneline` — one commit per line (your daily driver)
- `--graph --decorate` — draws the branch structure (more useful after Lab 4)
- `-p -1` — show the full diff (`-p` = patch) of the last 1 commit

✅ **Checkpoint:** You can point at your history and explain what changed in each commit *without opening any files*.

---

## Part 2 — Follow along: .gitignore

### Step 6 — Create some junk

Real projects generate files that don't belong in version control: logs, secrets, build artifacts, editor settings. Simulate that:

```bash
echo "DEBUG: everything is fine" > debug.log
echo "API_KEY=super-secret-123" > .env
mkdir build && echo "compiled output" > build/app.out
git status
```

Git wants to track all of it. **Committing a real API key is a security incident** — bots scan public GitHub for leaked keys within seconds.

### Step 7 — Teach Git to ignore them

```bash
cat > .gitignore << 'EOF'
# Logs
*.log

# Secrets — never commit credentials
.env

# Build output
build/
EOF
git status
```

The junk is gone from `git status`. Notice the patterns: `*.log` matches every log file, `build/` matches a whole folder, and `#` lines are comments.

### Step 8 — Commit the .gitignore itself

```bash
git add .gitignore
git commit -m "Add gitignore for logs, secrets, and build output"
```

> 💡 `.gitignore` **is** committed and shared — that way the whole team ignores the same junk. The ignored files themselves stay on your machine, invisible to Git.

✅ **Checkpoint:** `git status` is clean even though `debug.log`, `.env`, and `build/` still exist (`ls -a` proves it).

---

## 🏆 Challenge — on your own (~8 min)

1. Add a second function `farewell(name)` to `greet.py` that returns `"Goodbye, {name}!"`, and a `print` call for it.
2. Before staging, view your unstaged change. Then stage it and view the staged change.
3. Commit with a properly formatted message.
4. Create a file named `scratch.tmp` and a folder named `tmp/` with any file inside. Update `.gitignore` so **all** `.tmp` files and the `tmp/` folder are ignored, then commit the `.gitignore` change.
5. **Bonus 1:** Run a single command that shows how many commits your repo has, one per line, and count them.
6. **Bonus 2:** You committed `.gitignore` *before* ever committing `.env`. Find out what would have happened if `.env` had been committed **first** — would adding it to `.gitignore` afterward remove it from tracking? (Try it with a dummy file if you're curious, or just reason it out — answer in the solutions.)

<details>
<summary>💡 Hint 1 — the two diff views</summary>

Unstaged: `git diff`. Staged: `git diff --staged`. The moment `git add` runs, the change moves from the first view to the second.
</details>

<details>
<summary>💡 Hint 2 — ignore patterns</summary>

A folder is ignored with a trailing slash (`tmp/`). A file extension is ignored with a wildcard (`*.tmp`). Append to `.gitignore` with `echo "pattern" >> .gitignore` or just edit it.
</details>

<details>
<summary>💡 Hint 3 — bonus 2</summary>

`.gitignore` only affects **untracked** files. What's the implication for a file that's already been committed?
</details>

---

## 🧾 What you learned

| Command | What it does |
|---|---|
| `git diff` | Changes in working directory not yet staged |
| `git diff --staged` | Changes staged and ready to commit (pre-commit review!) |
| `git log --oneline` | Compact history |
| `git log -p -<n>` | Full diffs for the last *n* commits |
| `.gitignore` | Patterns for files Git should never track |

**Key habits:** small focused commits · review with `git diff --staged` before committing · imperative commit messages · never commit secrets.
