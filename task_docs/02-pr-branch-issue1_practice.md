# 01. PR Branch Practice: Issue 1

이 문서는 부트캠프 실습자가 `frontend/app.js`를 수정하고 PR을 만든 뒤, 관리자가 merge하고 task 파일을 정리하는 전체 흐름을 따라 하기 위한 가이드입니다.

## 실습 목표

`frontend/app.js`의 `greet("world")` 실행 결과가 다음처럼 나오도록 수정합니다.

```text
Hello, world!
```

수정 전/후 예시는 아래 파일에서 확인할 수 있습니다.

```text
task_docs/samples/originals/app.js
task_docs/samples/revised/app.js
```

## 역할

| 역할 | 계정 | 할 일 |
| --- | --- | --- |
| Manager | `llmragdev@gmail.com` | PR 확인, merge, 완료 task 이동 |
| Worker | `namkyuglobal2@gmail.com` | branch 생성, 코드 수정, commit, push, PR 생성 |

## 1. Worker 폴더로 이동

worker는 worker용 clone 폴더에서 작업합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice-worker
```

계정과 git 설정을 확인합니다.

```powershell
gh auth status
git config user.name
git config user.email
git status
```

worker 계정은 다음 값이어야 합니다.

```text
user.name  = namkyuglobal2
user.email = namkyuglobal2@gmail.com
```

필요하면 설정합니다.

```powershell
git config user.name "namkyuglobal2"
git config user.email "namkyuglobal2@gmail.com"
```

## 2. main 최신화

새 branch를 만들기 전에 `main`을 최신 상태로 맞춥니다.

```powershell
git checkout main
git pull --ff-only origin main
```

## 3. 작업 branch 생성

```powershell
git checkout -b vibe/worker/frontend-greet-exclamation-1
```

현재 branch를 확인합니다.

```powershell
git branch --show-current
```

기대 결과:

```text
vibe/worker/frontend-greet-exclamation-1
```

## 4. frontend/app.js 수정

`frontend/app.js`를 아래처럼 수정합니다.

```js
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet("world"));
```

수정 결과를 실행해서 확인합니다.

```powershell
node frontend/app.js
```

기대 결과:

```text
Hello, world!
```

## 5. Commit

```powershell
git status
git add frontend/app.js
git commit -m "feat: add exclamation to frontend greeting"
```

커밋이 만들어졌는지 확인합니다.

```powershell
git log --oneline -3
```

## 6. Push

```powershell
git push -u origin vibe/worker/frontend-greet-exclamation-1
```

## 7. PR 생성

이 실습에서는 GitHub Issue를 별도로 만들지 않고, 로컬 task 파일 `task/frontend/issue-1.md`를 지시서로 사용합니다.

```powershell
gh pr create `
  --base main `
  --head vibe/worker/frontend-greet-exclamation-1 `
  --title "feat: add exclamation to frontend greeting #1" `
  --body "Practice task: task/frontend/issue-1.md"
```

생성된 PR URL을 확인하고 manager에게 전달합니다.

## 8. Manager가 PR 확인

manager는 관리자 폴더에서 작업합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice
```

관리자 계정인지 확인합니다.

```powershell
gh auth status
git config user.name
git config user.email
```

관리자 계정은 다음 값이어야 합니다.

```text
user.name  = llmragdev
user.email = llmragdev@gmail.com
```

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

- `frontend/app.js`만 수정되었는지
- `Hello, world!`가 출력되도록 수정되었는지
- 불필요한 파일이 PR에 포함되지 않았는지

## 9. Manager가 merge

문제가 없으면 squash merge합니다.

```powershell
gh pr merge PR_NUMBER --squash --delete-branch
```

merge 후 관리자 로컬 `main`을 최신화합니다.

```powershell
git checkout main
git pull --ff-only origin main
```

## 10. Manager가 task_done으로 이동

Issue 1 작업이 완료되었으므로 task 파일을 이동합니다.

```powershell
mkdir task_done\frontend
git mv task\frontend\issue-1.md task_done\frontend\issue-1.md
git mv task\frontend\review-1.md task_done\frontend\review-1.md
git status
git commit -m "chore: move completed frontend task"
git push origin main
```

## 11. 완료 확인

```powershell
git status
gh pr list --state all --limit 5
```

정상 상태:

- PR은 `MERGED`
- `frontend/app.js`는 `Hello, world!`를 출력함
- `task/frontend/issue-1.md`는 없어짐
- `task_done/frontend/issue-1.md`가 존재함
- 관리자 `main`은 `origin/main`과 동기화됨

