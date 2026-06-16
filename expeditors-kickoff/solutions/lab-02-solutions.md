# Lab 2 Solutions — The Edit → Stage → Commit Loop

> ⚠️ **Instructor:** share after the lab.

---

## Challenge walkthrough

### 1. Add a `farewell` function

Final `greet.py`:

```python
def greet(name):
    return f"Hello, {name.upper()}!"

def farewell(name):
    return f"Goodbye, {name}!"

print(greet("Expeditors"))
print(farewell("Expeditors"))
```

### 2. View unstaged, then staged

```bash
git diff              # shows the new function as + lines (unstaged)
git add greet.py
git diff              # now EMPTY
git diff --staged     # the change moved here
```

**The concept being tested:** `git diff` compares *working directory ↔ staging area*; `git diff --staged` compares *staging area ↔ last commit*. A change appears in exactly one of the two views, never both. Students who internalize this never get confused about "where" a change is.

### 3. Commit

```bash
git commit -m "Add farewell function"
```

### 4. Ignore `.tmp` files and `tmp/`

```bash
echo "scratch note" > scratch.tmp
mkdir tmp && echo "junk" > tmp/junk.txt

echo "*.tmp" >> .gitignore
echo "tmp/" >> .gitignore

git status                      # scratch.tmp and tmp/ no longer listed
git add .gitignore
git commit -m "Ignore tmp files and tmp directory"
```

**Watch for:** students who write `>` instead of `>>` and wipe their existing `.gitignore` — their `.env` and `debug.log` reappear in `git status`. Great diagnostic moment.

### 5. Bonus 1 — count commits

```bash
git log --oneline | wc -l
```

Expected: 6 commits at this point (README, greet, uppercase, gitignore, farewell, tmp-ignore) — counts vary if they rebuilt the repo; the *method* is what matters. Also acceptable: `git rev-list --count HEAD`.

### 6. Bonus 2 — does .gitignore affect already-committed files?

**No.** `.gitignore` only prevents *untracked* files from being staged. A file that's already committed is **tracked**, and Git keeps tracking it regardless of `.gitignore`.

Demonstration:

```bash
echo "oops" > already-tracked.txt
git add already-tracked.txt && git commit -m "Track a file"
echo "already-tracked.txt" >> .gitignore
echo "edit" >> already-tracked.txt
git status        # still shows as modified! ignore had no effect
```

The fix (preview of a future lesson — don't dwell):

```bash
git rm --cached already-tracked.txt    # stop tracking, keep the file on disk
git commit -m "Stop tracking already-tracked.txt"
```

**The real-world punchline:** if you commit a secret and push it, adding it to `.gitignore` does **nothing** — the secret is in history forever (and must be *rotated*, not just removed). This is why we ignore `.env` from the very first commit.

---

## Common stumbles & what they teach

| Symptom | Cause | Lesson |
|---|---|---|
| `git diff` shows nothing after editing | They already ran `git add` | The change is staged; use `--staged` |
| `.gitignore` "not working" | File was already tracked, or pattern typo (e.g. `tmp` vs `tmp/`) | Ignore rules apply to untracked files only |
| Commit message in past tense / vague | Habit | Re-show the imperative rule; it matches Git's own messages ("Merge branch...") |
