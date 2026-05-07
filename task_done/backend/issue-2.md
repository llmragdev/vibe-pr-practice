# backend/health-version task #2

This task follows:

```text
task_docs/02-0-pr-branch-issue2_practice.md
```

## Owner

Worker:

```text
namkyuglobal2@gmail.com
```

The manager creates the GitHub Issue first. The worker starts after receiving the GitHub Issue number.

## Goal

Update `backend/api.js` so `health()` returns:

```js
{ status: "ok", version: "1.0.0" }
```

Sample files:

```text
task_docs/src_samples/originals/api.js
task_docs/src_samples/revised/api.js
```

## Branch

Start from the latest `main`.

```bash
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/backend-health-version-2
```

## Change

Edit `backend/api.js`.

```js
function health() {
  return { status: "ok", version: "1.0.0" };
}

module.exports = { health };
```

## Commit

```bash
git status
git add backend/api.js
git commit -m "feat: add version to health response"
```

## Push

```bash
git push -u origin vibe/worker/backend-health-version-2
```

## PR

Create a PR from `vibe/worker/backend-health-version-2` to `main`.

Use `Closes #2` so GitHub Issue #2 is closed automatically when the PR is merged.

If the actual GitHub Issue number is not `#2`, replace `#2` with the real number.

```bash
gh pr create --base main --head vibe/worker/backend-health-version-2 --title "feat: add version to health response #2" --body "Closes #2"
```

After creating the PR, the worker stops. The manager handles review, merge, and `task_done` cleanup.

