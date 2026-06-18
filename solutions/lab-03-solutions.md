# Lab 3 Solutions — Local Meets Remote

> ⚠️ **Instructor:** share after the lab.

---

## Publishing paths (Part 1)

Students now choose one explicit path. Both are valid.

### Option A (Recommended) — New Codespace from the repo

- Student created the GitHub repo (often with README checked for convenience).
- Created a fresh Codespace using the **Code → Codespaces → Create codespace on main** button.
- In the new Codespace they copied their files (or recreated content) and did `git push`.
- In this environment `git remote -v` usually already showed the correct origin.
- Normal `git push` and `git pull` should work from here on.

**Common student questions in Option A:**
- "Where are my old files?" → They need to copy from the previous Codespace (file explorer or `cp`).
- "Do I still need `git remote add`?" → Usually not — Codespaces sets it when created from the repo.
- "I'm in /workspaces/... not ~/engineering-journal" → This is normal. Use `pwd` and `cd` to the right folder before `git pull` / later commands.
- "Can I delete the old blank Codespace?" → Yes, after confirming everything is pushed.

### Option B — Stay in blank Codespace + PAT

- Student created an *empty* GitHub repo (all boxes unchecked).
- Created a fine-grained PAT (Contents: Read + Write, scoped only to this repo).
- Ran:
  ```bash
  git remote remove origin 2>/dev/null || true
  git remote add origin https://USERNAME:PAT@github.com/USERNAME/engineering-journal.git
  git -c credential.helper= push -u origin main
  ```
- After success, later commands are normal (with the bypass as a safety net).

**Common student questions in Option B:**
- "Why did my PAT not work at first?" → The Codespaces credential helper overrode it until `-c credential.helper=` was used.
- "Do I have to do this every time?" → Usually only for the very first push. After that the helper is less aggressive.

**Instructor tip:** Encourage most students to try Option A for the rest of the day. Students who chose Option B have seen a valuable real-world friction.

---

## Challenge walkthrough

The challenge simulates two teammates using the student's two local copies (`engineering-journal` and `journal-clone`) of one GitHub repo.

**Directory note:** After publishing, students were told to run the "Quick orientation" commands. 
- Option A students are often in `/workspaces/engineering-journal` (or similar).
- Option B students are usually in `~/engineering-journal`.
- Tell them to `cd` to the correct folder before these steps if needed.

**Note:** If a push in the challenge fails with the 403 permission error, students should use:

```bash
git -c credential.helper= push
```

This is the same bypass taught in the lab body. After the very first successful push of the repo, this is rarely needed again (especially for students who used Option A).

### 1. Commit and push from the clone

```bash
cd ~/journal-clone
echo "This note was born in the clone." > clone-note.md
git add clone-note.md
git commit -m "Add note from cloned copy"
git push
```

**The embedded question — is `-u` needed? No.** `git clone` automatically configures `origin` *and* sets up branch tracking. `-u` is only required when *you* manually connected the remote with `git remote add`. In Option B this was done by hand. In Option A, Codespaces often set the remote automatically when the Codespace was created from the repo, so many students never run that command.

### 2. Get the commit into the original copy — no browser

```bash
cd ~/engineering-journal   # or the correct folder for your chosen path (see orientation above)
git pull
cat clone-note.md     # it arrived
```

**Concept:** the two folders cannot see each other. GitHub is the hub; all sync is push-up/pull-down through it. This is exactly how two real teammates' laptops relate.

### 3. Edit, commit, push from the original

```bash
cd ~/engineering-journal   # or the correct folder for your chosen path
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

(Again: use the correct location of your `journal-clone` folder.)

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
| `remote origin already exists` | Ran `git remote add` twice (mostly Option B students) | `git remote set-url origin <url>` to fix, or `git remote remove origin` and re-add. Option A students usually don't run this command at all. |
| Push rejected with "fetch first" | Remote is ahead | Pull, then push (see Bonus 2) |
| `Permission denied to YOUR-USERNAME` (403) on push **in Codespaces** | Blank template Codespace has a scoped `GITHUB_TOKEN` that only trusts the template repo | Use the path you chose: **Option A** (new Codespace from the repo — recommended) or **Option B** (fine-grained PAT + `git -c credential.helper= push`). |
| `Permission denied` / auth prompt on local machine | Wrong username in URL, classic PAT expired, or Git Credential Manager not installed | Verify URL has *your* username; re-run `gh auth login` or let Git Credential Manager open the browser flow. Never use your GitHub password. |
| "error: src refspec main does not match any" | No commits yet, or branch named `master` | Commit first; check `git branch` |
| Created the GitHub repo *with* a README, push fails with "unrelated histories" | **Only a problem in Option B.** In Option B you must create the repo empty (no initial commit). In Option A it is fine (and often recommended) to let GitHub add a README because you start fresh inside the new Codespace. | For Option B: delete the GitHub repo and recreate it empty. |

---

## Codespaces "Permission denied to yourself" (403) — Full Troubleshooting

**This is now fully documented in the student lab** under **Part 1 — Publish your repo (choose one path)**.

Key points for instructors:

- The root cause is the scoped `GITHUB_TOKEN` on blank-template Codespaces.
- Students must follow **all** steps inside either the Option A block **or** the Option B block — mixing them causes confusion.
- Option A (create Codespace from the repo) is the experience we want them to have long-term.
- Option B teaches the bypass `git -c credential.helper= push` after wiring a fine-grained PAT into the remote URL.
- After the first successful push, students are told to run the "Quick orientation" commands (`pwd`, `git remote -v`, etc.). Remind them of this before they start Part 2.
- After the first successful push, normal commands usually work for both paths.
- The detailed student commands (including PAT creation steps and directory reminders) live in the lab — point students back to their chosen option if they get lost.
