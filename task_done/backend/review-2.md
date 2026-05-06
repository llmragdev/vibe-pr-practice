# backend PR review task #2

## Check

- `backend/api.js` is the only source file changed.
- `health()` returns `{ status: "ok", version: "1.0.0" }`.
- The PR body includes `References #2`.

## Commands

```bash
gh pr view PR_NUMBER --comments
gh pr diff PR_NUMBER
```

## Review Comment

```bash
gh pr comment PR_NUMBER --body "Reviewed backend health response change. Looks good."
```
