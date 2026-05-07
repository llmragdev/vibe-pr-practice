# 01. PR Fork Practice - Main

fork 기반 PR 실습에서 실제로 따라 할 명령만 모은 메인 가이드입니다.

자세한 설명과 개념 정리는 `task_docs/01_sub-pr-fork-practice.md`를 참고합니다.

## 이번 실습 값

| 항목 | 값 |
| --- | --- |
| 원본 repository | `llmragdev/vibe-pr-practice` |
| Worker GitHub ID | `namkyuglobal2` |
| Worker fork | `namkyuglobal2/vibe-pr-practice` |
| 원본 local 폴더 | `F:\proj_boot\git_practice\vibe-pr-practice` |
| fork local 폴더 | `F:\proj_boot\git_practice\vibe-pr-practice-fork-namkyuglobal2` |
| 작업 branch | `vibe/worker/fork-frontend-greet-1` |
| PR 방향 | `namkyuglobal2:vibe/worker/fork-frontend-greet-1` -> `llmragdev:main` |

## 전체 흐름

```text
fork 생성
-> fork clone
-> upstream 연결
-> branch 생성
-> 코드 수정
-> commit
-> fork로 push
-> 원본 main으로 PR 생성   (실습은 생략)
-> manager review/merge  (실습은 생략)
-> worker fork 최신화  (실습은 생략)
```

## 0. Manager 준비

원본 repository 폴더에서 작업합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice
gh auth status
git remote -v
git status
```

Manager 계정은 `llmragdev`여야 합니다.

## 1. Worker 계정 확인

```powershell
gh auth status
```

Worker 계정이 아니라면 다시 로그인합니다.

```powershell
gh auth logout
gh auth login
gh auth status
```

commit 작성자도 Worker로 맞춥니다.

```powershell
git config --global user.name "namkyuglobal2"
git config --global user.email "namkyuglobal2@gmail.com"
```

## 2. 원본 repository fork

```powershell
gh repo fork llmragdev/vibe-pr-practice --clone=false
```

이미 fork가 있다면 이 단계는 넘어갑니다.

## 3. fork repository clone

상위 폴더에서 clone합니다.

```powershell
cd F:\proj_boot\git_practice
gh repo clone namkyuglobal2/vibe-pr-practice vibe-pr-practice-fork-namkyuglobal2
cd F:\proj_boot\git_practice\vibe-pr-practice-fork-namkyuglobal2
```

VS Code에서는 아래 폴더를 엽니다.

```text
F:\proj_boot\git_practice\vibe-pr-practice-fork-namkyuglobal2
```

remote를 확인합니다.

```powershell
git remote -v
```

기대 값:

```text
origin  https://github.com/namkyuglobal2/vibe-pr-practice.git
```

## 4. upstream remote 연결

```powershell
git remote add upstream https://github.com/llmragdev/vibe-pr-practice.git
git remote -v
```

기대 값:

```text
origin    https://github.com/namkyuglobal2/vibe-pr-practice.git
upstream  https://github.com/llmragdev/vibe-pr-practice.git
```

## 5. main 최신화

```powershell
git checkout main
git fetch upstream
git pull --ff-only upstream main
git push origin main
```

## 6. 작업 branch 생성

```powershell
git checkout -b vibe/worker/fork-frontend-greet-1
git branch --show-current
```

## 7. 코드 수정

`frontend/app.js`를 수정해서 실행 결과가 아래처럼 나오게 합니다.

```text
Hello, fork world!
```

예시:

```js
function greet(name) {
  return `Hello, fork ${name}!`;
}

console.log(greet("world"));
```

확인:

```powershell
node frontend/app.js
```

## 8. commit

```powershell
git status
git add frontend/app.js
git commit -m "feat: update greeting in fork practice"
git log --oneline -3
```

## 9. fork repository로 push

```powershell
git push -u origin vibe/worker/fork-frontend-greet-1
```

여기서 `origin`은 Worker fork인 `namkyuglobal2/vibe-pr-practice`입니다.

## 10. PR 생성

```powershell
gh pr create `
  --repo llmragdev/vibe-pr-practice `
  --base main `
  --head namkyuglobal2:vibe/worker/fork-frontend-greet-1 `
  --title "feat: update greeting in fork practice" `
  --body "Fork practice PR from namkyuglobal2 to llmragdev."
```

브라우저에서 만들 때는 방향을 확인합니다.

```text
base repository: llmragdev/vibe-pr-practice
base branch: main

head repository: namkyuglobal2/vibe-pr-practice
compare branch: vibe/worker/fork-frontend-greet-1
```


 