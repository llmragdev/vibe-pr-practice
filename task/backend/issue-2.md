# backend/health-version task #2

## Goal

Update `backend/api.js` so `health()` returns:

```js
{ status: "ok", version: "1.0.0" }
```

## Branch

```bash
git checkout -b vibe/worker/backend-health-version-2
```

## Commit

```bash
git add backend/api.js
git commit -m "feat: add version to health response"
```

## PR

Create a PR from `vibe/worker/backend-health-version-2` to `main`.

```bash
gh pr create --base main --head vibe/worker/backend-health-version-2 --title "feat: add version to health response #2" --body "References #2"
```
