# Lab 3 Solutions — Local Meets Remote

> ⚠️ **Instructor:** share after the lab.

---

## Challenge walkthrough

The challenge simulates two teammates using the student's two local copies (`engineering-journal` and `journal-clone`) of one GitHub repo.

### 1. Commit and push from the clone

```bash
cd ~/journal-clone
echo "This note was born in the clone." > clone-note.md
git add clone-note.md
git commit -m "Add note from cloned copy"
git push
```

**The embedded question — is `-u` needed? No.** `git clone` automatically configures `origin` *and* sets up branch tracking. `-u` is only required when you wire up a remote by hand (`git remote add`), as in Part 1. This distinction trips up many beginners who cargo-cult `-u origin main` forever.

### 2. Get the commit into the original copy — no browser

```bash
cd ~/engineering-journal
git pull
cat clone-note.md     # it arrived
```

**Concept:** the two folders cannot see each other. GitHub is the hub; all sync is push-up/pull-down through it. This is exactly how two real teammates' laptops relate.

### 3. Edit, commit, push from the original

```bash
echo "And this line was added from the original copy." >> clone-note.md
git commit -am "Extend clone note from original copy"
git push
```

(`-am` works here because the file is already tracked. New files still need explicit `git add`.)

### 4. Sync the clone and verify both match

```bash
cd ~/journal-clone
git pull
git log --oneline      # compare with the other copy — identical hashes
```

Matching **hashes** (not just messages) is the proof — a hash uniquely identifies a commit's content *and* its entire ancestry.

### 5. Bonus 1 — `fetch` vs `pull`

After the *other* copy pushes, in this copy:

```bash
git fetch
git status
```

```
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
```

**Explanation:** `fetch` downloads new remote commits into your local *knowledge of the remote* (`origin/main`) but does **not** touch your working branch or files. `git status` can now compare your `main` against the updated `origin/main` and report you're behind. `pull` = `fetch` + `merge`. Fetch is "check the mailbox"; pull is "check the mailbox and open the mail." Many professionals fetch first to review what's incoming (`git log main..origin/main`) before merging.

### 6. Bonus 2 — pushing when you're behind

Setup: copy A pushes a commit; copy B (without pulling) makes a different commit and pushes:

```bash
git push
```

```
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/...'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. ... You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
```

**Explanation:** Git refuses to push because doing so could discard the remote commits you don't have. This is a *safety feature*, not an error in the scary sense. The fix is exactly what the hint says:

```bash
git pull      # merges remote work into yours (may open an editor for the merge message — save & close)
git push      # now succeeds
```

**Teaching point:** this rejection is possibly the most common Git "error" in professional life. Students who've seen it once, on purpose, won't panic. It also reinforces the habit: **pull before you push, pull before you start work.**

---

## Common stumbles & what they teach

| Symptom | Cause | Lesson |
|---|---|---|
| `remote origin already exists` | Ran `git remote add` twice | `git remote set-url origin <url>` to fix, or `git remote remove origin` and re-add |
| Push rejected with "fetch first" | Remote is ahead | Pull, then push (see Bonus 2) |
| `Permission denied` / auth prompt loop on push | Pushing to a repo they don't own, or token issue | Check the URL has *their* username; in Codespaces, the repo must be theirs |
| "error: src refspec main does not match any" | No commits yet, or branch named `master` | Commit first; check `git branch` |
| Created the GitHub repo *with* a README, push fails with "unrelated histories" | Histories diverge at the root | Easiest fix today: delete the GitHub repo, recreate **empty**. (The `--allow-unrelated-histories` flag exists but is a rabbit hole.) |
