# Manager 계정에서 PR 확인 하고 merge 작업 수행 

## 11. Manager PR 확인 이 부분은 참고만하세요 

메니저 계정에서 아래의 작업을 수행합니다.  

```powershell
gh auth logout
gh auth login
gh auth status
```

원본 repository 폴더에서 확인합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice
gh pr list --repo llmragdev/vibe-pr-practice --state open
gh pr view PR_NUMBER --repo llmragdev/vibe-pr-practice --comments
gh pr diff PR_NUMBER --repo llmragdev/vibe-pr-practice
```

확인할 것:

- PR 방향이 `namkyuglobal2:...` -> `llmragdev:main`인지
- 수정 파일이 `frontend/app.js`인지
- 실행 결과가 `Hello, fork world!`인지

## 12. Manager merge

```powershell
gh pr merge PR_NUMBER --repo llmragdev/vibe-pr-practice --squash --delete-branch
git checkout main
git pull --ff-only origin main
```

## 13. Worker fork 최신화

fork local 폴더에서 진행합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice-fork-namkyuglobal2
git checkout main
git fetch upstream
git pull --ff-only upstream main
git push origin main
git branch -d vibe/worker/fork-frontend-greet-1
```

## 완료 확인

```powershell
git status
git remote -v
git log --oneline -5
```

완료 기준:

- `origin`이 `namkyuglobal2/vibe-pr-practice`를 가리킴
- `upstream`이 `llmragdev/vibe-pr-practice`를 가리킴
- PR이 `MERGED` 상태임
- Worker fork의 `main`이 원본 `main` 최신 상태를 반영함
