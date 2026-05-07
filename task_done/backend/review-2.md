# backend PR review task #2

This review task follows:

```text
task_docs/02-0-pr-branch-issue2_practice.md
```

## Owner

Manager:

```text
llmragdev@gmail.com
```

The worker creates the PR. The manager reviews, merges, and moves the completed task files to `task_done`.

## Review Target

Review the PR for GitHub Issue #2.

If the actual GitHub Issue number is not `#2`, use the real number from GitHub.

## Check

- `backend/api.js` is the only source file changed.
- `health()` returns `{ status: "ok", version: "1.0.0" }`.
- The PR body includes `Closes #2`.
- The PR base branch is `main`.
- The PR head branch is `vibe/worker/backend-health-version-2`.
- No unrelated files are included.

## Commands

Replace `PR_NUMBER` with the actual PR number.

```bash
gh pr view PR_NUMBER --comments
gh pr diff PR_NUMBER
```

## Review Comment

```bash
gh pr comment PR_NUMBER --body "Reviewed by manager: backend health response includes version 1.0.0. Looks good to merge."
```

## Merge

```bash
gh pr merge PR_NUMBER --squash --delete-branch
```

After merge, update local `main`.

```bash
git checkout main
git pull --ff-only origin main
```

Confirm that GitHub Issue #2 is closed.

```bash
gh issue view 2
```

## Move Completed Task Files

After the PR is merged and the GitHub Issue is closed, move the task files.

```bash
mkdir task_done\backend
git mv task\backend\issue-2.md task_done\backend\issue-2.md
git mv task\backend\review-2.md task_done\backend\review-2.md
git status
git commit -m "chore: move completed backend task"
git push origin main
```

