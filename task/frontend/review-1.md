# frontend PR review task #1

## Check

- `frontend/app.js` is the only source file changed.
- `greet("world")` returns `Hello, world!`.
- The PR body includes `References #1`.

## Commands

```bash
gh pr view PR_NUMBER --comments
gh pr diff PR_NUMBER
```

## Review Comment

```bash
gh pr comment PR_NUMBER --body "Reviewed frontend greeting change. Looks good."
```
