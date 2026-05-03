# PR Branch Practice Quick Guide

This is a short operating guide for practicing GitHub Issue, branch, PR, review, and merge flow.

The actual coding instructions live in the task files:

- `task/frontend/issue-1.md`
- `task/backend/issue-2.md`
- `task/docs/issue-3.md`
- `task/frontend/review-1.md`
- `task/backend/review-2.md`
- `task/docs/review-3.md`

Use this guide to know **who does what** and **in what order**.

## 0. Program Overview

This repository uses a very small JavaScript example program that runs with Node.js.

Files:

- `frontend/app.js`: Defines `greet(name)` and prints a greeting.
- `backend/api.js`: Defines `health()` and returns a simple status object.
- `docs/usage.md`: Documents how to run the sample.

Run:

```powershell
node frontend/app.js
```

Initial output:

```text
Hello, world
```

## 1. Roles

### You: repository owner / reviewer

You create GitHub Issues, ask an AI coding agent to work from the task files, review PRs, merge PRs, and move completed task files to `task_done/`.

### AI coding agent: worker

The agent reads one `task/*/issue-*.md` file, creates a branch, edits code, commits, pushes, and creates a PR.

### Task files: source of truth

Do not duplicate the full task requirements in this quick guide. The actual requirements are inside the task files.

## 2. Start From A Clean Main

Run this before starting or assigning work.

```powershell
git checkout main
git pull --ff-only origin main
git status
git log --oneline -3
git remote -v
```

Command notes:

- `git checkout main`: Move to the main branch.
- `git pull --ff-only origin main`: Update local `main` from GitHub only if Git can fast-forward cleanly.
- `--ff-only`: Safety option. It prevents Git from creating an unexpected merge commit during pull.
- `origin`: The remote GitHub repository name.
- `main`: The branch to update from.
- `git status`: Shows changed, staged, and untracked files.
- `git log --oneline -3`: Shows the latest 3 commits in short form.
- `git remote -v`: Shows the connected GitHub remote URL.

Expected clean state:

```text
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

If you see `Untracked files`, Git sees a new file that has not been added yet. Add and commit it if it should be kept.

## 3. Create GitHub Issues

Owner action.

Create three GitHub Issues in the repository. Keep the GitHub Issue short, because the detailed task instruction already exists in `task/`.

Suggested Issue titles:

```text
frontend/greet-exclamation
backend/health-version
docs/add-run-guide
```

Suggested Issue body template:

```markdown
See the matching task file in this repository.

Task file:
- `task/frontend/issue-1.md`

Acceptance:
- Worker opens a PR that references this issue.
- Reviewer verifies the PR diff before merge.
```

Use the matching task file for each issue:

- Issue #1: `task/frontend/issue-1.md`
- Issue #2: `task/backend/issue-2.md`
- Issue #3: `task/docs/issue-3.md`

## 4. Ask The Agent To Work On Issue #1

Owner action.

Give the AI coding agent this instruction:

```text
Read `task/frontend/issue-1.md` and execute it.

Do not work directly on `main`.
Create the branch requested in the task file.
Modify only the required files.
Run the verification command.
Commit, push, and create a PR to `main`.
Include `References #1` in the PR body.
```

Agent action.

The agent should do the branch, edit, test, commit, push, and PR creation described in `task/frontend/issue-1.md`.

## 5. Review Issue #1 PR

Owner action.

Find the open PR:

```powershell
gh pr list --state open
```

Inspect the PR:

```powershell
gh pr view PR_NUMBER --comments
gh pr diff PR_NUMBER
```

Command notes:

- `PR_NUMBER`: The GitHub PR number. For PR #4, use `4`.
- `gh pr view PR_NUMBER --comments`: Shows the PR body and comments.
- `gh pr diff PR_NUMBER`: Shows the code changes in the PR.

Review checklist:

- The PR changed the file requested by `task/frontend/issue-1.md`.
- The behavior matches the task file.
- The PR body includes `References #1`.

Leave a review comment:

```powershell
gh pr comment PR_NUMBER --body "Reviewed. Looks good."
```

Merge:

```powershell
gh pr merge PR_NUMBER --squash --delete-branch
```

Command notes:

- `--squash`: Combine the PR commits into one commit on `main`.
- `--delete-branch`: Delete the remote worker branch after merge.

Update local `main`:

```powershell
git checkout main
git pull --ff-only origin main
git status
```

## 6. Repeat For Issue #2

Owner action.

Give the agent this instruction:

```text
Read `task/backend/issue-2.md` and execute it.

Do not work directly on `main`.
Create the branch requested in the task file.
Modify only the required files.
Run any reasonable verification available.
Commit, push, and create a PR to `main`.
Include `References #2` in the PR body.
```

Then review and merge the PR using the same commands from the previous section.

Use the review checklist from `task/backend/review-2.md`.

## 7. Repeat For Issue #3

Owner action.

Give the agent this instruction:

```text
Read `task/docs/issue-3.md` and execute it.

Do not work directly on `main`.
Create the branch requested in the task file.
Modify only the required files.
Commit, push, and create a PR to `main`.
Include `References #3` in the PR body.
```

Then review and merge the PR using the same commands from the previous section.

Use the review checklist from `task/docs/review-3.md`.

## 8. Move Completed Task Files

Owner action.

Only do this after all three PRs have been created, reviewed, and merged.

```powershell
New-Item -ItemType Directory -Force task_done\frontend, task_done\backend, task_done\docs
git mv task\frontend\issue-1.md task_done\frontend\issue-1.md
git mv task\frontend\review-1.md task_done\frontend\review-1.md
git mv task\backend\issue-2.md task_done\backend\issue-2.md
git mv task\backend\review-2.md task_done\backend\review-2.md
git mv task\docs\issue-3.md task_done\docs\issue-3.md
git mv task\docs\review-3.md task_done\docs\review-3.md
git status
git commit -m "chore: move completed practice tasks"
git push
```

Command notes:

- `New-Item -ItemType Directory -Force ...`: Create folders if missing. Do nothing harmful if they already exist.
- `git mv`: Move files in a way Git tracks as a rename.
- `chore:`: Commit message prefix for repository maintenance work.

## 9. Useful Check Commands

```powershell
git status
git branch --show-current
git log --oneline -5
git remote -v
gh auth status
gh pr list --state all
```

Command notes:

- `git status`: Shows current file state.
- `git branch --show-current`: Shows the current branch.
- `git log --oneline -5`: Shows the latest 5 commits.
- `git remote -v`: Shows the GitHub remote URL.
- `gh auth status`: Shows which GitHub account is logged in.
- `gh pr list --state all`: Shows open, closed, and merged PRs.

## 10. Recovery Commands

Before using recovery commands, always run:

```powershell
git status
git branch --show-current
```

Discard a file change before commit:

```powershell
git restore FILE_PATH
```

Undo the latest commit but keep the file changes staged:

```powershell
git reset --soft HEAD~1
```

Delete a remote branch:

```powershell
git push origin --delete BRANCH_NAME
```

Delete a local branch:

```powershell
git checkout main
git branch -D BRANCH_NAME
```

Notes:

- `HEAD~1`: The commit before the current one.
- `--soft`: Remove the commit but keep the changes.
- `-D`: Force delete a local branch. Use it only after confirming the branch is no longer needed.
