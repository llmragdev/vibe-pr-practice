# 01. PR Fork Practice - Main

fork 기반 PR 실습에서 실제로 따라 할 명령만 모은 메인 가이드입니다.

자세한 설명과 개념 정리는 `task_docs/01_sub-pr-fork-practice.md`를 참고합니다.

## 이번 실습 값

| 항목 | 값 |
| --- | --- |
| 원본 repository | `llmragdev/vibe-pr-practice` |
| Worker GitHub ID | `hanschoi20` |
| Worker fork | `hanschoi20/vibe-pr-practice` |
| 원본 local 폴더 | `F:\proj_boot\git_practice\vibe-pr-practice` |
| fork local 폴더 | `F:\proj_boot\git_practice\vibe-pr-practice-fork-hanschoi20` |
| 작업 branch | `vibe/worker/fork-frontend-greet-1` |
| PR 방향 | `hanschoi20:vibe/worker/fork-frontend-greet-1` -> `llmragdev:main` |

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

## 0. Manager 준비   (참고만 하세요)

원본 repository 폴더에서 작업합니다.

```powershell
cd F:\proj_boot\git_practice\vibe-pr-practice
gh auth status
git remote -v
git status
```

Manager 계정은 `llmragdev`여야 합니다.   (작업 수행 생략합니다.)
(이번 fork 실습은 manager 계정을 만들지 않고 기존 소스를 clone하고 fork, branch 만들고 커밋하고 pull request까지만 합니다 ) 
(manger 계정에서 소스 merge 작업은 생략합니다 )

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

auth 상태 결과 예시:

```text
github.com
  ✓ Logged in to github.com account hanschoi20 (keyring)
  - Active account: true
  - Git operations protocol: https
  - Token: gho_************************************
  - Token scopes: 'gist', 'read:org', 'repo', 'workflow'
```

commit 작성자도 Worker로 맞춥니다.
GitHub에 등록된 공개 이메일이 없다면 noreply 이메일을 사용할 수 있습니다.
아래 명령으로 현재 로그인한 GitHub 계정의 noreply 이메일을 확인합니다.

```powershell
gh api user --jq '{login: .login, id: .id, email: .email}'
```
    {
      "email": null,
      "id": 107975999,
      "login": "hanschoi20"
    }

`107975999`는 GitHub 내부 사용자 ID이고, `hanschoi20`은 GitHub 사용자 이름(login ID)입니다.
즉 `107975999+hanschoi20@users.noreply.github.com`는 `hanschoi20` 계정의 GitHub noreply 이메일입니다.

```powershell
git config --global user.name "hanschoi20"
git config --global user.email "107975999+hanschoi20@users.noreply.github.com"
```

## 2. 원본 repository fork

`https://github.com/llmragdev/vibe-pr-practice` 원본 repository를 Worker 계정으로 fork합니다.
`gh` 명령에서는 GitHub URL 대신 `llmragdev/vibe-pr-practice`처럼 `OWNER/REPO` 형식으로 적을 수 있습니다.

```powershell
gh repo fork llmragdev/vibe-pr-practice --clone=false
```

위 명령을 실행하면 Worker 계정의 GitHub에 `https://github.com/hanschoi20/vibe-pr-practice` fork repository가 생성됩니다.
`--clone=false` 옵션 때문에 GitHub에는 fork가 생기지만, 아직 로컬 PC에는 clone하지 않습니다.

이미 fork가 있다면 이 단계는 넘어갑니다.

## 3. fork repository clone

2번에서 만든 Worker fork repository를 clone합니다.
원본 `https://github.com/llmragdev/vibe-pr-practice`를 clone하는 것이 아니라, Worker 계정의 fork인 `https://github.com/hanschoi20/vibe-pr-practice`를 clone합니다.
여기서 `hanschoi20`은 GitHub의 사용자 이름(login ID)이며, fork repository 주소는 `https://github.com/[GitHub 사용자 이름]/vibe-pr-practice` 형식입니다.
`gh repo clone hanschoi20/vibe-pr-practice ...`에서도 앞의 `hanschoi20`은 같은 GitHub 사용자 이름입니다.
이렇게 해야 로컬 repository의 `origin`이 Worker fork를 가리키고, 이후 push도 Worker fork로 올라갑니다.

상위 폴더에서 clone합니다.

```powershell
cd F:\proj_boot\git_practice
gh repo clone hanschoi20/vibe-pr-practice vibe-pr-practice-fork-hanschoi20
cd F:\proj_boot\git_practice\vibe-pr-practice-fork-hanschoi20
```

VS Code에서는 아래 폴더를 엽니다.

```text
F:\proj_boot\git_practice\vibe-pr-practice-fork-hanschoi20
```

remote를 확인합니다.

```powershell
git remote -v
```

기대 값:

```text
origin  https://github.com/hanschoi20/vibe-pr-practice.git
```

## 4. upstream remote 연결
(upstream 작업은 fork repo가 원본 repo를 참조할 수 있게 연결하는 작업)

```powershell
git remote add upstream https://github.com/llmragdev/vibe-pr-practice.git
git remote -v
```

기대 값:

```text
origin    https://github.com/hanschoi20/vibe-pr-practice.git
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

여기서 `origin`은 Worker fork인 `hanschoi20/vibe-pr-practice`입니다.

## 10. PR 생성

```powershell
gh pr create `
  --repo llmragdev/vibe-pr-practice `
  --base main `
  --head hanschoi20:vibe/worker/fork-frontend-greet-1 `
  --title "feat: update greeting in fork practice" `
  --body "Fork practice PR from hanschoi20 to llmragdev."
```

브라우저에서 만들 때는 방향을 확인합니다.

```text
base repository: llmragdev/vibe-pr-practice
base branch: main

head repository: hanschoi20/vibe-pr-practice
compare branch: vibe/worker/fork-frontend-greet-1
```


 
