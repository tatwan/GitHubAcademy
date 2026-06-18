# Lab 4 Solutions — Branch, Change, Merge

> ⚠️ **Instructor:** share after the lab.

---

## Setup note (from Lab 3 paths)

Lab 4 starts with:

```bash
cd ~/engineering-journal
git status
git pull
```

- **Option B students** from Lab 3 are usually still in `~/engineering-journal`.
- **Option A students** may be in a different Codespace folder (commonly `/workspaces/engineering-journal`).

Tell students to run `pwd` first and `cd` to the correct location if `git pull` says "not a git repository".

The auth bypass reminder from Lab 3 still applies for any `git push` commands (`git -c credential.helper= push`).

---

## Challenge walkthrough

### 1. Branch with a new file

```bash
git switch -c add-resources
cat > resources.md << 'EOF'
# Git Learning Resources

- Pro Git book (free): https://git-scm.com/book
- GitHub Skills: https://skills.github.com
- Oh Shit, Git!?!: https://ohshitgit.com
EOF
git add resources.md
git commit -m "Add Git learning resources list"
```

### 2. Move `main` forward independently

```bash
git switch main
echo "- Learned about fast-forward vs merge commits" >> standup.md
git commit -am "Add branching learnings to standup notes"
```

Now history has **diverged**: each branch has a commit the other lacks. This is the everyday reality on a team — `main` moves while you work.

### 3. Merge — and it's NOT a fast-forward this time

```bash
git merge add-resources
```

Git opens an editor with a prepared message like `Merge branch 'add-resources'` (save and close — in Codespaces/VS Code, close the tab; in vim, `:wq`), or completes silently with a default message. Output says something like:

```
Merge made by the 'ort' strategy.
```

```bash
git log --oneline --graph
```

```
*   9f8e7d6 Merge branch 'add-resources'
|\
| * 4c5b6a7 Add Git learning resources list
* | 1a2b3c4 Add branching learnings to standup notes
|/
* ...
```

**Why no fast-forward?** A fast-forward is only possible when the receiving branch (`main`) hasn't moved since the feature branch split off — then Git can just slide the pointer forward. Here `main` gained its own commit in step 2, so the histories diverged and Git had to create a **merge commit** — a commit with *two parents* that ties the lines back together.

**Why no conflict?** The branches touched *different files* (`resources.md` vs `standup.md`). Git auto-merges anything that doesn't collide on the same lines. Conflict ≠ divergence; conflict = same-line collision.

### 4. Clean up and push

```bash
cd ~/engineering-journal   # or the folder that contains your repo (check with pwd)
git branch -d add-resources
git push
```

**If this push (or any push) fails with the 403 permission error**, apply the bypass from Lab 3:

```bash
git -c credential.helper= push
```

### 5. Bonus 1 — one-sentence answer

> A **fast-forward** just moves the branch pointer ahead because nothing diverged, while a **merge commit** is a new commit with two parents that Git creates when both branches have moved since they split.

### 6. Bonus 2 — switching with uncommitted changes

```bash
# on main, edit a file that differs between branches, don't commit
git switch some-branch
```

```
error: Your local changes to the following files would be overwritten by checkout:
        standup.md
Please commit your changes or stash them before you switch branches.
Aborting
```

**Why protective:** switching rewrites the working directory to the target branch's snapshot. Your uncommitted edits exist *only* in the working directory — nowhere in Git's database — so overwriting them would destroy work **irrecoverably**. Git refuses rather than risk it.

(If the edited file is *identical* on both branches, Git allows the switch and carries the change along — students may observe this and be confused; it's only a problem when the file differs.)

**Rule of thumb to leave them with:** *committed work is essentially never lost; uncommitted work is the only thing Git can't protect.* Commit early, commit often. (`git stash` is the other answer — name-drop it as a future topic.)

---

## Common stumbles & what they teach

| Symptom | Cause | Lesson |
|---|---|---|
| Merged "backwards" (merged `main` into the feature branch) | Forgot to switch first | The current branch is always the *receiver*: switch to `main`, then merge |
| Vim/editor appears at merge time | Merge commit needs a message | Save-and-close accepts the default; `:wq` in vim |
| `branch -d` refuses: "not fully merged" | Trying to delete unmerged work | That's the safety working as designed; merge first (or `-D` to force — explain the danger) |
| Conflict markers committed into the file | Resolved without removing `<<<<<<<`/`=======`/`>>>>>>>` | Always re-read the file (or run the code) before `git add` |
| "Whoa, my file disappeared!" panic when switching | Expected behavior | Files live in commits; switching swaps snapshots — nothing is lost |
