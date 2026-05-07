# 02. PR Branch Practice: Issue 2

이 문서는 GitHub Issue를 실제로 만들고, worker가 PR을 올린 뒤, manager가 review와 merge를 담당하는 실습 가이드입니다.

Issue 1에서는 로컬 task 파일만 지시서로 사용했습니다. Issue 2에서는 GitHub Issues까지 함께 사용합니다.

## 실습 목표

`backend/api.js`의 `health()` 함수가 다음 객체를 반환하도록 수정합니다.

```js
{ status: "ok", version: "1.0.0" }
```

수정 전/후 예시는 아래 파일에서 확인할 수 있습니다.

```text
task_docs/samples/originals/api.js
task_docs/samples/revised/api.js
```

## 역할

| 역할 | 계정 | 할 일 |
| --- | --- | --- |
| Manager | `llmragdev@gmail.com` | GitHub Issue 생성, PR review, merge, 완료 task 이동 |
| Worker | `namkyuglobal2@gmail.com` | branch 생성, 코드 수정, commit, push, PR 생성 |

## 계정 전환 원칙

이 실습은 한 컴퓨터에서 manager와 worker를 번갈아 사용합니다.

- Manager 작업 전에는 `gh auth status`가 `llmragdev` 계정을 보여야 합니다.
- Worker 작업 전에는 `gh auth status`가 `namkyuglobal2` 계정을 보여야 합니다.
- 계정이 다르면 `gh auth logout` 후 `gh auth login`으로 다시 로그인합니다.
- `git config user.name`, `git config user.email`은 각 폴더에서 따로 맞춥니다.

## 1. Manager가 GitHub Issue 생성

관리자 폴더에서 실행합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice
gh auth status
git config user.name
git config user.email
```

기대 값:

```text
GitHub CLI account = llmragdev
user.name          = llmragdev
user.email         = llmragdev@gmail.com
```

GitHub CLI 계정이 manager가 아니면 다시 로그인합니다.

```powershell
gh auth logout
gh auth login
gh auth status
```

git 작성자 정보가 다르면 설정합니다.

```powershell
git config user.name "llmragdev"
git config user.email "llmragdev@gmail.com"
```

GitHub Issue를 생성합니다.

PowerShell에서는 body 전체를 작은따옴표로 감싸면 JSON 모양의 따옴표를 편하게 넣을 수 있습니다.

```powershell
gh issue create `
  --title "backend: add version to health response" `
  --body 'Update backend/api.js so health() returns { status: "ok", version: "1.0.0" }.'
```

Issue 번호를 확인합니다.

```powershell
gh issue list --state open
```

이 문서에서는 생성된 Issue 번호를 `#2`라고 가정합니다. 실제 번호가 다르면 이후 명령의 `#2`를 실제 번호로 바꿔 사용하세요.

# github 사이트에서 Issues 확인 
```
github Issues 확인:  https://github.com/llmragdev/vibe-pr-practice/issues/
```

## 2. Worker 계정으로 전환

여기서부터는 worker가 작업합니다. 먼저 worker 폴더로 이동합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice-worker
```

GitHub CLI 계정도 worker 계정이어야 합니다.

```powershell
gh auth status
```

`llmragdev`로 로그인되어 있으면 worker 계정으로 다시 로그인합니다.

```powershell
gh auth logout
gh auth login
gh auth status
```

로그인 후 `namkyuglobal2` 계정으로 표시되는지 확인합니다.

git 작성자 정보도 worker로 맞춥니다.

```powershell
git config user.name "namkyuglobal2"
git config user.email "namkyuglobal2@gmail.com"
git config user.name
git config user.email
```

기대 값:

```text
GitHub CLI account = namkyuglobal2
user.name          = namkyuglobal2
user.email         = namkyuglobal2@gmail.com
```

## 3. main 최신화

```powershell
git checkout main
git pull --ff-only origin main
```

## 4. 작업 branch 생성

```powershell
git checkout -b vibe/worker/backend-health-version-2
```

현재 branch를 확인합니다.

```powershell
git branch --show-current
```

기대 결과:

```text
vibe/worker/backend-health-version-2
```

## 5. backend/api.js 수정

`backend/api.js`를 아래처럼 수정합니다.

```js
function health() {
  return { status: "ok", version: "1.0.0" };
}

module.exports = { health };
```

## 6. Commit

```powershell
git status
git add backend/api.js
git commit -m "feat: add version to health response"
```

## 7. Push

```powershell
git push -u origin vibe/worker/backend-health-version-2
```

## 8. PR 생성

PR body에는 `Closes #2`를 넣습니다.

`Closes #2`를 넣으면 PR이 merge될 때 GitHub Issue #2가 자동으로 닫힙니다.

```powershell
gh pr create `
  --base main `
  --head vibe/worker/backend-health-version-2 `
  --title "feat: add version to health response #2" `
  --body "Closes #2"
```

worker는 여기까지 진행합니다.

## 9. Manager 계정으로 전환

여기서부터는 manager가 review와 merge를 진행합니다. 관리자 폴더로 돌아옵니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice
```

GitHub CLI 계정도 manager 계정이어야 합니다.

```powershell
gh auth status
```

`namkyuglobal2`로 로그인되어 있으면 manager 계정으로 다시 로그인합니다.

```powershell
gh auth logout
gh auth login
gh auth status
```

로그인 후 `llmragdev` 계정으로 표시되는지 확인합니다.

git 작성자 정보도 manager로 맞춥니다.

```powershell
git config user.name "llmragdev"
git config user.email "llmragdev@gmail.com"
git config user.name
git config user.email
```

기대 값:

```text
GitHub CLI account = llmragdev
user.name          = llmragdev
user.email         = llmragdev@gmail.com
```

## 10. Manager가 PR review

열린 PR을 확인합니다.

```powershell
gh pr list --state open
```

PR 번호를 확인한 뒤 아래 명령에서 `PR_NUMBER`를 실제 번호로 바꿉니다.

```powershell
gh pr view PR_NUMBER --comments
gh pr diff PR_NUMBER
```

확인할 내용:

- `backend/api.js`만 수정되었는지
- `health()`가 `{ status: "ok", version: "1.0.0" }`를 반환하는지
- PR body에 `Closes #2`가 있는지
- 불필요한 파일이 PR에 포함되지 않았는지

review comment를 남깁니다.

```powershell
gh pr comment PR_NUMBER --body "Reviewed by manager: backend health response includes version 1.0.0. Looks good to merge."
```

## 11. Manager가 merge

문제가 없으면 squash merge합니다.
 squash는 branch에서 여러번 커밋했더라도 pr create 생성할때 지정한 title로 merge 코멘트 입력 함 
  
```powershell
gh pr merge PR_NUMBER --squash --delete-branch
```

merge 후 관리자 로컬 `main`을 최신화합니다.

```powershell
git checkout main
git pull --ff-only origin main
```

Issue가 닫혔는지 확인합니다.

```powershell
gh issue view 2
```

## 12. Manager가 task_done으로 이동

Issue 2 작업이 완료되었으므로 task 파일을 이동합니다.

```powershell
mkdir task_done\backend
git mv task\backend\issue-2.md task_done\backend\issue-2.md
git mv task\backend\review-2.md task_done\backend\review-2.md
git status
git commit -m "chore: move completed backend task"
git push origin main
```

## 13. 완료 확인

```powershell
git status
gh pr list --state all --limit 5
gh issue list --state all --limit 5
```

정상 상태:

- PR은 `MERGED`
- GitHub Issue #2는 `CLOSED`
- `backend/api.js`는 version 값을 반환함
- `task/backend/issue-2.md`는 없어짐
- `task_done/backend/issue-2.md`가 존재함
- 관리자 `main`은 `origin/main`과 동기화됨

