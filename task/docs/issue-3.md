# docs/add-run-guide task #3

## Goal

Update `docs/usage.md` with a short Node.js run guide.

## Acceptance Criteria

- The docs mention running `node frontend/app.js`.
- The guide stays short and practical.

## Branch

```bash
git checkout -b vibe/worker/docs-add-run-guide-3
```

## Commit

```bash
git add docs/usage.md
git commit -m "docs: add sample run guide"
```

## PR

Create a PR from `vibe/worker/docs-add-run-guide-3` to `main`.

```bash
gh pr create --base main --head vibe/worker/docs-add-run-guide-3 --title "docs: add sample run guide #3" --body "References #3"
```
