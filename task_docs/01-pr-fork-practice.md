# 01. PR Fork Practice

이 문서는 fork 기반 PR 실습을 위한 가이드입니다.

기존 branch 실습은 같은 repository 안에서 branch를 만들어 PR을 보내는 흐름입니다. 이 실습에서는 worker가 main repository에 직접 push 권한이 없다고 가정하고, worker 계정의 fork repository에서 작업한 뒤 main repository로 Pull Request를 보냅니다.

## 실습 목표

`namkyuglobal2@gmail.com` 계정으로 `llmragdev/vibe-pr-practice` repository를 fork하고, fork에서 branch를 만들어 수정한 뒤 원본 repository의 `main` branch로 PR을 보냅니다.

전체 흐름은 다음과 같습니다.

```text
main repository fork
-> fork clone
-> upstream remote 연결
-> branch 생성
-> 코드 수정
-> commit
-> fork repository로 push
-> fork branch에서 main repository main으로 PR 생성
-> manager review
-> manager merge
```

## 역할

| 역할 | GitHub 계정 | 이메일 | 할 일 |
| --- | --- | --- | --- |
| Manager | `llmragdev` | `llmragdev@gmail.com` | 원본 repository 관리, PR 확인, review, merge |
| Worker | `namkyuglobal2` | `namkyuglobal2@gmail.com` | fork 생성, branch 작업, commit, push, PR 생성 |

## repository 관계

이번 실습에서는 repository가 2개입니다.

```text
원본 repository:
https://github.com/llmragdev/vibe-pr-practice

worker fork repository:
https://github.com/namkyuglobal2/vibe-pr-practice
```

local clone에서는 remote를 다음처럼 구분합니다.

```text
origin   = worker fork repository
upstream = manager 원본 repository
```

PR 방향은 다음과 같습니다.

```text
namkyuglobal2:vibe/worker/fork-frontend-greet-1
-> llmragdev:main
```

## 1. Worker 계정 확인

worker가 GitHub CLI에서 `namkyuglobal2` 계정으로 로그인되어 있는지 확인합니다.

```powershell
gh auth status
```

다른 계정으로 로그인되어 있다면 다시 로그인합니다.

```powershell
gh auth logout
gh auth login
gh auth status
```

git commit 작성자도 worker 계정으로 맞춥니다.

```powershell
git config --global user.name "namkyuglobal2"
git config --global user.email "namkyuglobal2@gmail.com"
```

확인합니다.

```powershell
git config --global user.name
git config --global user.email
```

기대 값:

```text
namkyuglobal2
namkyuglobal2@gmail.com
```

## 2. 원본 repository fork

worker 계정으로 원본 repository를 fork합니다.

```powershell
gh repo fork llmragdev/vibe-pr-practice --clone=false
```

브라우저에서 직접 진행해도 됩니다.

```text
https://github.com/llmragdev/vibe-pr-practice
-> Fork
-> Owner: namkyuglobal2
-> Repository name: vibe-pr-practice
-> Create fork
```

fork가 만들어졌는지 확인합니다.

```powershell
gh repo view namkyuglobal2/vibe-pr-practice --web
```

## 3. fork repository clone

기존 branch 실습 폴더와 섞이지 않도록 worker fork 전용 폴더를 따로 만듭니다.

```powershell
cd F:\proj_boot\git_practice
gh repo clone namkyuglobal2/vibe-pr-practice vibe-pr-practice-fork-worker
cd F:\proj_boot\git_practice\vibe-pr-practice-fork-worker
```

remote를 확인합니다.

```powershell
git remote -v
```

기대 값:

```text
origin  https://github.com/namkyuglobal2/vibe-pr-practice.git (fetch)
origin  https://github.com/namkyuglobal2/vibe-pr-practice.git (push)
```

## 4. upstream remote 연결

원본 repository를 `upstream` remote로 추가합니다.

```powershell
git remote add upstream https://github.com/llmragdev/vibe-pr-practice.git
git remote -v
```

기대 값:

```text
origin    https://github.com/namkyuglobal2/vibe-pr-practice.git (fetch)
origin    https://github.com/namkyuglobal2/vibe-pr-practice.git (push)
upstream  https://github.com/llmragdev/vibe-pr-practice.git (fetch)
upstream  https://github.com/llmragdev/vibe-pr-practice.git (push)
```

fork 작업에서는 보통 `origin`에는 push하고, `upstream`에서는 최신 main을 가져옵니다.

## 5. upstream main 최신화

원본 repository의 최신 상태를 가져옵니다.

```powershell
git checkout main
git fetch upstream
git pull --ff-only upstream main
```

fork repository의 `main`도 최신 상태로 맞춰 둡니다.

```powershell
git push origin main
```

## 6. 작업 branch 생성

fork local clone에서 새 branch를 만듭니다.

```powershell
git checkout -b vibe/worker/fork-frontend-greet-1
```

현재 branch를 확인합니다.

```powershell
git branch --show-current
```

기대 값:

```text
vibe/worker/fork-frontend-greet-1
```

## 7. 코드 수정

`frontend/app.js`를 수정해서 `greet("world")` 실행 결과가 다음처럼 나오게 합니다.

```text
Hello, fork world!
```

예시 코드:

```js
function greet(name) {
  return `Hello, fork ${name}!`;
}

console.log(greet("world"));
```

수정 전/후 예시는 아래 파일을 참고합니다.

```text
task_docs/src_samples/originals/app.js
task_docs/src_samples/revised/app.js
```

실행 결과를 확인합니다.

```powershell
node frontend/app.js
```

기대 값:

```text
Hello, fork world!
```

## 8. commit

변경 파일을 확인하고 commit합니다.

```powershell
git status
git add frontend/app.js
git commit -m "feat: update greeting in fork practice"
```

commit이 만들어졌는지 확인합니다.

```powershell
git log --oneline -3
```

## 9. fork repository로 push

worker는 원본 repository가 아니라 자신의 fork repository로 push합니다.

```powershell
git push -u origin vibe/worker/fork-frontend-greet-1
```

여기서 `origin`은 `namkyuglobal2/vibe-pr-practice`입니다.

## 10. PR 생성

fork branch에서 원본 repository의 `main`으로 PR을 만듭니다.

```powershell
gh pr create `
  --repo llmragdev/vibe-pr-practice `
  --base main `
  --head namkyuglobal2:vibe/worker/fork-frontend-greet-1 `
  --title "feat: update greeting in fork practice" `
  --body "Fork practice PR from namkyuglobal2 to llmragdev."
```

생성된 PR URL을 확인합니다.

```powershell
gh pr view --repo llmragdev/vibe-pr-practice --web
```

브라우저에서 직접 만들 경우 방향을 꼭 확인합니다.

```text
base repository: llmragdev/vibe-pr-practice
base branch: main

head repository: namkyuglobal2/vibe-pr-practice
compare branch: vibe/worker/fork-frontend-greet-1
```

## 11. Manager 계정으로 PR 확인

manager는 원본 repository 폴더에서 확인합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice
gh auth status
git config user.name
git config user.email
```

manager 계정 기대 값:

```text
GitHub CLI account = llmragdev
user.name          = llmragdev
user.email         = llmragdev@gmail.com
```

다른 계정이면 manager 계정으로 다시 로그인합니다.

```powershell
gh auth logout
gh auth login
gh auth status
```

열린 PR을 확인합니다.

```powershell
gh pr list --repo llmragdev/vibe-pr-practice --state open
```

PR 번호를 확인한 뒤 내용을 봅니다.

```powershell
gh pr view PR_NUMBER --repo llmragdev/vibe-pr-practice --comments
gh pr diff PR_NUMBER --repo llmragdev/vibe-pr-practice
```

확인할 내용:

- PR 방향이 `namkyuglobal2:... -> llmragdev:main`인지 확인
- 수정 파일이 `frontend/app.js`인지 확인
- 실행 결과가 `Hello, fork world!`인지 확인
- 불필요한 파일이 포함되지 않았는지 확인

review comment 예시:

```powershell
gh pr comment PR_NUMBER --repo llmragdev/vibe-pr-practice --body "Reviewed by manager: fork PR direction and frontend greeting change look good."
```

## 12. Manager merge

manager가 PR을 squash merge합니다.

```powershell
gh pr merge PR_NUMBER --repo llmragdev/vibe-pr-practice --squash --delete-branch
```

원본 repository의 local main도 최신화합니다.

```powershell
git checkout main
git pull --ff-only origin main
```

## 13. Worker fork 최신화

merge 후 worker는 fork local clone에서 원본 repository의 최신 main을 다시 가져옵니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice-fork-worker
git checkout main
git fetch upstream
git pull --ff-only upstream main
git push origin main
```

작업 branch가 남아 있으면 정리합니다.

```powershell
git branch -d vibe/worker/fork-frontend-greet-1
```

## 14. 완료 확인

manager repository에서 확인합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice
git status
gh pr list --repo llmragdev/vibe-pr-practice --state all --limit 5
```

worker fork repository에서 확인합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice-fork-worker
git status
git remote -v
git log --oneline -5
```

완료 기준:

- fork repository가 `namkyuglobal2/vibe-pr-practice`로 생성됨
- local clone의 `origin`이 worker fork를 가리킴
- local clone의 `upstream`이 `llmragdev/vibe-pr-practice`를 가리킴
- PR 방향이 `namkyuglobal2` fork branch에서 `llmragdev` main으로 설정됨
- PR이 `MERGED` 상태가 됨
- worker fork의 `main`도 upstream 최신 상태를 반영함

## branch 실습과 fork 실습 차이

| 구분 | branch 실습 | fork 실습 |
| --- | --- | --- |
| 작업 위치 | 원본 repository 안의 branch | worker 계정의 fork repository |
| push 대상 | 원본 repository | fork repository |
| 필요한 권한 | 원본 repository write 권한 필요 | 원본 repository write 권한 불필요 |
| PR 방향 | `llmragdev:작업branch -> llmragdev:main` | `namkyuglobal2:작업branch -> llmragdev:main` |
| 주 사용 상황 | 같은 팀 내부 협업 | 외부 기여자, 권한 없는 contributor |

