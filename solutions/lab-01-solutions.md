# Lab 1 Solutions — Your First Repository

> ⚠️ **Instructor:** share this *after* the lab. Exact commands may vary slightly — what matters is the concepts, noted under each step.

---

## Challenge walkthrough

### 1. Create `goals.md` with three goals

```bash
echo "Master the Git basics" > goals.md
echo "Get comfortable on the command line" >> goals.md
echo "Push my first project to GitHub" >> goals.md
```

Or simply create the file in the editor — both are equally correct. **Concept check:** `>` creates/overwrites, `>>` appends. Students who used `>` three times will have only the last line — a useful mistake to discuss.

### 2. Stage only `goals.md`

```bash
git add goals.md
```

**Why this matters:** `git add .` is convenient but indiscriminate. Naming files explicitly is how you build *focused* commits when you've touched several files but only want to commit some. Verify with `git status` — only `goals.md` should be green.

### 3. Commit with a descriptive message

```bash
git commit -m "Add bootcamp learning goals"
```

Good messages are imperative ("Add", not "Added") and describe the change. "stuff", "asdf", and "commit 3" fail the test: could a teammate understand the change from the message alone?

### 4. Prove there are 3 commits, one per line

```bash
git log --oneline
```

```
f3e9a21 Add bootcamp learning goals
b7c4d10 Add commands cheat sheet and update journal
a1b2c3d Add README with day 1 journal entry
```

Newest first — Git history reads top-down from most recent.

### 5. Bonus — full hash of the first commit

```bash
git log
```

Scroll to the bottom (press space; `q` to quit) — the oldest commit's `commit` line shows the full 40-character SHA-1 hash.

Elegant alternatives worth showing:

```bash
git log --reverse --oneline | head -1     # oldest first, take the first
git rev-list --max-parents=0 HEAD          # "the commit with no parent" = root commit
```

**Teaching point:** the short hash (`a1b2c3d`) is just the first 7 characters of the full hash — Git accepts either anywhere a hash is needed, as long as it's unambiguous.

---

## Common stumbles & what they teach

| Symptom | Cause | Lesson |
|---|---|---|
| `fatal: not a git repository` | Ran a git command outside the project folder | Git is per-folder; `cd` first, and `git status` to orient |
| `Please tell me who you are` error on commit | Skipped `git config` setup | Every commit is signed with an identity |
| Committed with `git commit` (no `-m`) and got "stuck" in an editor | Vim opened for the message | `Esc` then `:q!` to abort, or `i`, type, `Esc`, `:wq` — or just always use `-m` for now |
| Nothing happens after `git add` | Expected | `add` is silent on success; `git status` is how you confirm |
