# Git & GitHub Fundamentals Knowledge Check

**Audience:** New Coding/DevOps bootcamp students  
**Format:** 10-question practice quiz  
**Recommended use:** Final 10 minutes of class or post-class self-check  
**Scoring:** 1 point per question

## Learning Objective Coverage

| Objective | Questions |
|---|---|
| Explain the Git mental model and three areas | Q1, Q2 |
| Use the edit -> stage -> commit workflow | Q3, Q4 |
| Connect local Git work to GitHub | Q5, Q6, Q7 |
| Use branches and recognize merge behavior | Q8, Q9 |
| Describe pull requests at a high level | Q10 |

## Quiz

### Q1. Which command should you run first when you are unsure what state your repository is in?

- A. `git log`
- B. `git status`
- C. `git commit`
- D. `git clone`

**Correct answer:** B  
**Explanation:** `git status` shows the current branch, unstaged changes, staged changes, untracked files, and useful next steps.

### Q2. What does `git add README.md` do?

- A. Saves `README.md` permanently in the repository history
- B. Uploads `README.md` to GitHub
- C. Moves the current `README.md` change into the staging area
- D. Creates a new branch named `README.md`

**Correct answer:** C  
**Explanation:** `git add` stages a change for the next commit. The permanent snapshot happens when you run `git commit`.

### Q3. You changed a file, ran `git add`, and then `git diff` shows no output. What should you run to review the staged change?

- A. `git diff --staged`
- B. `git status --remote`
- C. `git log --graph`
- D. `git branch -d`

**Correct answer:** A  
**Explanation:** `git diff` shows unstaged changes. `git diff --staged` shows what is staged and about to be committed.

### Q4. Which commit message is the best choice for a beginner team project?

- A. `stuff`
- B. `final final`
- C. `changes`
- D. `Add setup steps to README`

**Correct answer:** D  
**Explanation:** Good commit messages are specific, readable in history, and written in the imperative.

### Q5. You already have a local repository with commits. What should you do when creating the matching GitHub repository?

- A. Create it empty, with no README or `.gitignore`
- B. Add every starter file GitHub offers
- C. Create a second unrelated commit history in the browser
- D. Clone it before adding the remote

**Correct answer:** A  
**Explanation:** An empty GitHub repo lets your existing local history push cleanly without unrelated histories.

### Q6. What does `git clone <url>` do?

- A. Creates a new empty local Git repository only
- B. Downloads an existing repository, including history, and configures `origin`
- C. Stages all files in the current folder
- D. Deletes local changes and replaces them with GitHub

**Correct answer:** B  
**Explanation:** `clone` copies the repository history and sets up the remote connection automatically.

### Q7. Your push is rejected because the remote contains work you do not have locally. What is the safest next step?

- A. Delete the remote repository
- B. Run `git pull`, resolve anything Git asks about, then push again
- C. Run `git init` again
- D. Delete the `.git` folder

**Correct answer:** B  
**Explanation:** Git is protecting remote commits you do not have. Pull first, integrate the remote work, then push.

### Q8. You want to merge `add-resources` into `main`. Which command sequence has the correct merge direction?

- A. `git switch add-resources` then `git merge main`
- B. `git branch -d main` then `git merge add-resources`
- C. `git switch main` then `git merge add-resources`
- D. `git clone main` then `git merge add-resources`

**Correct answer:** C  
**Explanation:** The current branch receives the merge. Switch to `main`, then merge the feature branch into it.

### Q9. What is a merge conflict?

- A. A permanent loss of Git history
- B. A GitHub authentication failure
- C. A branch that has not been pushed yet
- D. A pause where Git asks you to resolve competing same-line changes

**Correct answer:** D  
**Explanation:** Conflicts happen when Git cannot safely choose between changes. You edit the file, remove markers, stage, and commit.

### Q10. In this course, what is the best high-level description of a pull request?

- A. A request to merge one branch into another, with review and discussion on GitHub
- B. A Git command that downloads a repository for the first time
- C. A replacement for commits
- D. A way to delete a branch from your local machine

**Correct answer:** A  
**Explanation:** A pull request is a GitHub collaboration feature built around branches, diffs, review, and merge decisions.

## Bias Check

- Correct answer distribution: A = 3, B = 3, C = 2, D = 2.
- Longest-answer pattern: mixed; the correct answer is not consistently the longest.
- Repeated wording cues: no repeated "always" or "never" patterns.
- Alignment: all questions map to stated course objectives and lab activities.
