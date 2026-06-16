# Lab 4 — Branch, Change, Merge

**Module 4: Branching Basics** · ⏱️ ~25 minutes

---

## 🎯 Goal

By the end of this lab you will be able to:

- Create a branch and switch between branches with `git switch`
- Make commits on a branch without touching `main`
- Merge a finished branch back into `main`
- Recognize (not yet master) what a merge conflict looks like

## 🧠 Why this matters

Branches are *the* feature that makes team software development possible. On a real team you will **never commit directly to `main`** — every feature, fix, and experiment happens on its own branch, gets reviewed, and only then merges in. `main` stays stable and shippable at all times. Branches in Git are nearly free (a branch is just a 41-byte pointer to a commit — not a copy of your code), which is why Git users branch constantly and fearlessly. Today's goal is exactly that: lose the fear.

## 🗺️ What to expect

You'll grow your `engineering-journal` with a feature branch, watch files appear and disappear as you switch branches (this is the "whoa" moment — let it sink in), merge your work back, and finish with a *guided, intentional* merge conflict so the first one you meet in real life isn't a surprise. The conflict part is follow-along and gentle — awareness, not mastery.

---

## Setup

```bash
cd ~/engineering-journal
git status        # clean working tree
git pull          # always start in sync
git branch        # shows: * main
```

> 💡 We use `git switch` (introduced 2019) rather than the older `git checkout`. Both work — `checkout` does many unrelated things, while `switch` does exactly one. You'll see `checkout` in older tutorials; mentally translate it.

---

## Part 1 — Follow along: your first branch

### Step 1 — Create and switch in one move

```bash
git switch -c add-standup-notes
```

`-c` = create. Output: `Switched to a new branch 'add-standup-notes'`

```bash
git branch
```

The `*` marks where you are. Both branches currently point at the **same commit** — creating a branch copies nothing.

> **Naming:** short, lowercase, hyphenated, descriptive: `add-standup-notes`, `fix-login-bug`. Many teams prefix with a ticket number: `eng-142-fix-login-bug`.

### Step 2 — Commit on the branch

```bash
cat > standup.md << 'EOF'
# Daily Standup Notes

## What I did yesterday
- Learned the Git mental model

## What I'm doing today
- Branching and merging

## Blockers
- None
EOF
git add standup.md
git commit -m "Add daily standup notes template"
```

### Step 3 — The "whoa" moment

```bash
ls            # standup.md is here
git switch main
ls            # standup.md is GONE
```

Not deleted — it lives safely in a commit on `add-standup-notes`. Switching branches rewrites your working directory to match the branch's snapshot. Switch back and watch it reappear:

```bash
git switch add-standup-notes
ls            # it's back
```

✅ **Checkpoint:** You can make `standup.md` appear and disappear by switching branches, and you can explain why.

### Step 4 — One more commit, then compare histories

```bash
echo "- Ask about merge conflicts" >> standup.md
git add standup.md
git commit -m "Add question to standup blockers"

git log --oneline           # branch history: 2 new commits
git switch main
git log --oneline           # main: doesn't have them
```

---

## Part 2 — Follow along: merge it back

### Step 5 — Merge

Rule: **switch to the branch that should receive the changes**, then merge the other one in. We want the notes *in main*:

```bash
git switch main
git merge add-standup-notes
```

Output mentions **`Fast-forward`**. Because `main` had no new commits of its own, Git simply slid the `main` pointer forward to your branch's latest commit. No new commit needed; cleanest possible merge.

```bash
git log --oneline --graph
ls    # standup.md is on main now
```

### Step 6 — Clean up and push

```bash
git branch -d add-standup-notes
git branch
git push
```

> 💡 Deleting a merged branch deletes **nothing** — the commits are in `main`'s history. Branches are sticky notes, not folders. `-d` even refuses to delete unmerged work, so it's safe.

✅ **Checkpoint:** `git branch` shows only `main`, and `standup.md` is on GitHub after the push.

---

## Part 3 — Follow along: meet a merge conflict (gently)

A conflict happens when **two branches change the same lines** and Git can't pick for you. Let's cause one on purpose, in a safe sandbox, with the instructor.

### Step 7 — Set up the collision

```bash
git switch -c update-title
sed -i 's/# Engineering Journal/# My Engineering Journal/' README.md
git commit -am "Change title to My Engineering Journal"

git switch main
sed -i 's/# Engineering Journal/# Team Engineering Journal/' README.md
git commit -am "Change title to Team Engineering Journal"
```

(On macOS locally, use `sed -i ''` — or just edit the title line in your editor by hand, which is the honest way anyway. `-am` stages all *tracked* modified files and commits in one step.)

Both branches changed **line 1 of README.md** to different things.

### Step 8 — Trigger it

```bash
git merge update-title
```

```
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

**Don't panic — nothing is broken.** Run `git status`: it says `both modified: README.md` and tells you exactly what to do.

### Step 9 — Look at the conflict markers

```bash
cat README.md
```

```
<<<<<<< HEAD
# Team Engineering Journal
=======
# My Engineering Journal
>>>>>>> update-title
```

Read it like this: between `<<<<<<< HEAD` and `=======` is *your current branch's* version; between `=======` and `>>>>>>>` is the *incoming branch's* version. Git is asking a question, not reporting an error.

### Step 10 — Resolve

Edit `README.md`: delete the three marker lines and keep the title you want (or write a third option — you're in charge):

```bash
# final line 1 should read, e.g.:
# Team Engineering Journal
```

Then tell Git you've decided:

```bash
git add README.md
git commit -m "Merge update-title, keep Team title"
git branch -d update-title
git push
```

✅ **Checkpoint:** `git log --oneline --graph` shows a merge commit joining two lines of history.

> 🧘 That's the whole conflict workflow: **open the file → pick what's right → remove markers → add → commit.** We'll go deeper in a future session; today you just needed to see that it's survivable.

---

## 🏆 Challenge — on your own (~8 min)

1. Create a branch `add-resources`, and on it create `resources.md` listing 2–3 Git learning links. Commit.
2. While that branch exists, switch back to `main` and add one line to `standup.md`. Commit. *(Now both branches have new commits — `main` has moved on.)*
3. Merge `add-resources` into `main`. Look at the output: is it a fast-forward this time? Why or why not? Check `git log --oneline --graph`.
4. Delete the merged branch and push everything.
5. **Bonus 1:** You changed *different files* on the two branches in steps 1–2, yet step 3's merge behaved differently from Step 5 in Part 2. Write one sentence explaining the difference between a fast-forward merge and a merge commit.
6. **Bonus 2:** Try `git switch` to a branch while you have **uncommitted** changes to a file that differs between branches. What does Git do? Why is this protective?

<details>
<summary>💡 Hint 1 — step 3</summary>

A fast-forward is only possible when the receiving branch hasn't moved since the other branch was created. Did `main` move?
</details>

<details>
<summary>💡 Hint 2 — no conflict expected</summary>

Different files (or different lines) = Git merges automatically with a *merge commit*. Conflicts only happen on the *same lines*.
</details>

<details>
<summary>💡 Hint 3 — bonus 2</summary>

Make an edit to `standup.md`, don't commit, then try to switch to a branch where that file differs. Read Git's message — when in doubt, commit first.
</details>

---

## 🧾 What you learned

| Command | What it does |
|---|---|
| `git branch` | List branches (`*` = current) |
| `git switch -c <name>` | Create a branch and switch to it |
| `git switch <name>` | Switch to an existing branch |
| `git merge <name>` | Merge `<name>` into your *current* branch |
| `git branch -d <name>` | Delete a merged branch (safe) |

**Key mental model:** a branch is a movable pointer to a commit. Switching branches swaps your working directory between snapshots. Merging brings one line of history into another — and a conflict is just Git asking you to make a call it can't make for you.
