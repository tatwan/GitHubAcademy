# Instructor Demo Scripts & Run of Show

Live demos run between the slide segment and the lab for each module. Each demo previews ~80% of what students will do in the lab, so the lab feels familiar rather than new.

**Setup before the day:**

- A clean Codespace (blank template) with terminal font size cranked up (`Cmd/Ctrl +` several times — aim for ~20pt on a Zoom share).
- A throwaway GitHub account or a dedicated demo account, logged in, so you can show repo creation without exposing your real notifications.
- Pre-create nothing — typing from scratch *is* the demo. Keep this file open on a second monitor.
- Prompt tip: a long shell prompt eats space. `export PS1='\W $ '` makes it minimal.
- Zoom: share the **window**, not the whole screen. Pause for questions after every "💬 ASK" cue.

---

## Demo 1 — The Mental Model Made Visible (~10 min, before Lab 1)

**Narrative arc:** "Watch one change travel through all three areas."

```bash
mkdir demo-project && cd demo-project
git status                          # 💬 "Why an error? Git isn't watching. We opt IN per folder."
git init
ls -a                               # 💬 point at .git — "THIS is the repository. Everything lives here."
git status                          # clean slate, on branch main

echo "hello" > notes.txt
git status                          # 🔴 untracked
git add notes.txt
git status                          # 🟢 staged — 💬 "Same file, new AREA. Nothing saved yet!"
git commit -m "Add notes file"
git status                          # clean
git log                             # 💬 decode: hash, author, date, message
```

**The status-color trick:** keep saying "red = working directory, green = staging area." It anchors the model visually.

**💬 ASK:** "If I delete this folder right now and I never pushed anywhere — is my commit safe?" *(No — local only. Plants the seed for Module 3.)*

**Pitfall to perform on purpose:** run `git commit` *before* `git add`-ing a second change — "nothing to commit." Git only commits what's staged.

---

## Demo 2 — Diff Like a Pro (~8 min, before Lab 2)

**Narrative arc:** "Never commit blind."

```bash
echo "line two" >> notes.txt
git diff                            # 💬 read the +/- lines together
git add notes.txt
git diff                            # EMPTY! 💬 dramatic pause — "Where did my change go?"
git diff --staged                   # there it is
git commit -m "Add second line to notes"
```

Then history:

```bash
git log --oneline                   # 💬 "Your daily driver."
git log -p -1                       # full patch
```

Then `.gitignore` with stakes:

```bash
echo "API_KEY=sk-secret-9999" > .env
git status                          # 💬 "If I commit this and push, bots find it in under a minute. Real incidents, real money."
echo ".env" > .gitignore
git status                          # .env vanished from status, .gitignore appeared
git add .gitignore && git commit -m "Ignore env file"
```

**💬 ASK:** "The `.env` file is still on my disk — prove it." (`ls -a`.) "So what does ignore actually do?" *(Hides it from Git, doesn't delete it.)*

---

## Demo 3 — Two Repos, One Truth (~10 min, before Lab 3)

**Narrative arc:** "My laptop and GitHub are two different computers that only sync when I say so."

1. **Browser:** github.com → New repository → `demo-project`, public, **all checkboxes off**. 💬 "Empty on purpose — our history already exists locally."
2. **Terminal:**

```bash
git remote add origin https://github.com/DEMO-USER/demo-project.git
git remote -v                       # 💬 "origin is a NICKNAME for that URL. Nothing more."
git push -u origin main             # 💬 "-u once per branch; afterwards plain push/pull"
```

3. **Browser:** refresh — files appear. Click into commit history; show the hashes match `git log --oneline`.
4. **The drift demo** — edit README on GitHub (pencil icon), commit. Back in terminal:

```bash
cat notes.txt && git log --oneline  # 💬 "My laptop has NO IDEA. There's no magic sync."
git pull
git log --oneline                   # the web commit arrives
```

5. **Clone:**

```bash
cd .. && git clone https://github.com/DEMO-USER/demo-project.git demo-clone
cd demo-clone && git log --oneline  # 💬 "Entire history, remote pre-wired. Day one at any job starts with this command."
```

**💬 ASK:** "When would you `init` and when would you `clone`?" *(init = project born on your machine; clone = project already exists somewhere.)*

**If authentication hiccups live:** stay calm, narrate it — "this is the #1 first-day friction in real jobs too" — and show the browser-popup flow. In Codespaces it just works; say so.

---

## Demo 4 — Branches Without Fear (~10 min, before Lab 4)

**Narrative arc:** "A branch is a sticky note on a commit, and I'll prove it."

```bash
cd ../demo-project && git pull
git branch                          # just main
git switch -c experiment
echo "wild idea" > experiment.txt
git add . && git commit -m "Try a wild idea"

ls                                  # experiment.txt present
git switch main
ls                                  # 💬 GONE. Pause. Let it land. "Deleted? No — stored. Watch."
git switch experiment
ls                                  # back
```

💬 "Switching branches re-paints your folder from Git's database. Your work is *safer* on a branch, not less safe."

**Merge:**

```bash
git switch main
git merge experiment                # fast-forward — 💬 explain the pointer slide with hands on camera
git log --oneline --graph
git branch -d experiment            # 💬 "Deleting the sticky note, not the commits."
```

**Conflict (timebox 3 min — awareness only):**

```bash
git switch -c title-a && sed -i 's/hello/HELLO FROM A/' notes.txt && git commit -am "Title A"
git switch main && sed -i 's/hello/HELLO FROM MAIN/' notes.txt && git commit -am "Title main"
git merge title-a                   # CONFLICT
cat notes.txt                       # 💬 walk the markers slowly: HEAD = mine, below = incoming
# edit file, keep one line, remove markers
git add notes.txt && git commit -m "Resolve title conflict"
```

💬 Closing line: "A conflict is Git asking a question, not throwing an error. You answer it by editing a file. That's all."

---

## Module 5 — No demo, but…

Open a real public repo (e.g. `github.com/microsoft/vscode`) and show: the Pull Requests tab, one open PR with its conversation/review, and the branch dropdown. 2 minutes of "this is where you're headed" makes the preview concrete without teaching workflow.

---

## Timing guardrails

| If you're behind by lunch | Cut |
|---|---|
| 5–10 min | Lab 3 bonus discussion; Demo 4 conflict becomes slides-only |
| 15+ min | Compress Module 5 to 8 min: PR tab tour + "why PRs" slide only |

| If you're ahead | Add |
|---|---|
| 10 min | Live tour of `git log --graph` on a big OSS repo; students love seeing real history |
| 20 min | Show VS Code's Source Control panel doing add/commit/push with buttons — "same Git, different steering wheel" |
