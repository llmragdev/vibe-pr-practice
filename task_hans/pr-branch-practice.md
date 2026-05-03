# PR/브랜치/Vibe Task 3건 연습 가이드

## 목적

이 문서는 `task/*.md` 지시서를 에이전트에게 맡길 때 생겼던 사고를 피하기 위해 만든 연습 절차다.

과거 사고 유형:

- 에이전트가 `issue.md` 안의 실제 작업은 하지 않고 `task_done/`으로 파일만 이동함
- `main` 또는 기준 브랜치에서 직접 작업해서 다른 폴더가 사라진 것처럼 보임
- branch, PR, push 중 어느 단계가 빠졌는지 몰라 PR이 올라가지 않음
- 작업 취소, 브랜치 삭제, 복구 개념이 약해서 대응이 어려움

핵심 원칙:

`task/*.md`는 작업 지시서다. 실제 완료 기준은 `task_done` 이동이 아니라 PR 생성, 리뷰 댓글 등록, merge 또는 close 확인이다.

## 사용할 계정

| 역할 | 계정 | 할 일 |
|------|------|------|
| 관리자/마스터 | `llmragdev@gmail.com` | 저장소 생성, Issue 작성, PR 확인, 리뷰 댓글, merge |
| 작업자 | `namkyuglobal2@gmail.com` | 작업 branch 생성, 코드 수정, commit, push, PR 생성 |

처음에는 이 2개 계정이면 충분하다. 관리자 계정이 리뷰어 역할도 같이 한다.

## 0단계: 로컬에서 새 프로젝트 만들고 GitHub에 올리기

권장 위치:

```text
F:\proj_boot\git_practice\vibe-pr-practice
```

### 0-1. 관리자 계정으로 로그인 확인

GitHub CLI를 쓴다면 관리자 계정 `llmragdev@gmail.com`으로 로그인되어 있어야 한다.

```bash
gh auth status
```

다른 계정으로 로그인되어 있으면 다시 로그인한다.

```bash
gh auth logout
gh auth login
```

웹에서 진행해도 된다. 처음에는 웹으로 저장소를 만들고, 터미널로 push하는 방식이 이해하기 쉽다.

### 0-2. 로컬 프로젝트 폴더 만들기

여기서는 **상위 폴더**인 `F:\proj_boot\git_practice`에서 시작한다고 생각한다.

아직 `git_practice` 폴더가 없다면 먼저 만든다.

```powershell
New-Item -ItemType Directory -Force F:\proj_boot\git_practice
Set-Location F:\proj_boot\git_practice
```

그 다음 연습 프로젝트 root 폴더를 만들고 그 안으로 들어간다.

```powershell
New-Item -ItemType Directory -Force vibe-pr-practice
Set-Location F:\proj_boot\git_practice\vibe-pr-practice
New-Item -ItemType Directory -Force frontend, backend, docs, task\frontend, task\backend, task\docs, task_done
```

이미 `F:\proj_boot\git_practice\vibe-pr-practice` 폴더를 만들어 둔 상태라면 아래 두 줄만 실행하면 된다.

```powershell
Set-Location F:\proj_boot\git_practice\vibe-pr-practice
New-Item -ItemType Directory -Force frontend, backend, docs, task\frontend, task\backend, task\docs, task_done
```

파일을 만든다.

```powershell
@'
# vibe-pr-practice

Git branch, PR, issue, review comment 연습용 저장소.
'@ | Set-Content -Encoding UTF8 README.md

@'
function greet(name) {
  return `Hello, ${name}`;
}

console.log(greet("world"));
'@ | Set-Content -Encoding UTF8 frontend\app.js

@'
function health() {
  return { status: "ok" };
}

module.exports = { health };
'@ | Set-Content -Encoding UTF8 backend\api.js

@'
# Usage

Run the sample files with Node.js.
'@ | Set-Content -Encoding UTF8 docs\usage.md
```

`task` 파일은 작업 지시서 파일이다. 지금 단계에서는 아래 6개 파일을 **빈 파일로 먼저 만들어도 된다**.

```text
task/frontend/issue-1.md
task/frontend/review-1.md
task/backend/issue-2.md
task/backend/review-2.md
task/docs/issue-3.md
task/docs/review-3.md
```

PowerShell로 빈 파일을 만들려면:

```powershell
New-Item -ItemType File -Force task\frontend\issue-1.md
New-Item -ItemType File -Force task\frontend\review-1.md
New-Item -ItemType File -Force task\backend\issue-2.md
New-Item -ItemType File -Force task\backend\review-2.md
New-Item -ItemType File -Force task\docs\issue-3.md
New-Item -ItemType File -Force task\docs\review-3.md
```

그 다음 문서 아래쪽의 `task/frontend/issue-1.md`, `task/backend/issue-2.md`, `task/docs/issue-3.md`, review 파일 예시를 각각 열어서 해당 파일에 붙여 넣는다.

처음 연습에서는 더 단순하게 해도 된다. 우선 빈 파일만 만들고 commit한 뒤, 나중에 VS Code에서 하나씩 내용을 채워도 괜찮다.

### 0-3. Git 저장소로 만들기

```bash
git init
git branch -M main
git config user.name "llmragdev"
git config user.email "llmragdev@gmail.com"
git status
git add .
git commit -m "chore: init practice project"
```

여기까지가 로컬 commit이다. 아직 GitHub에는 올라가지 않았다.

### 0-3-1. `dubious ownership` 에러가 나면

`F:` 드라이브처럼 Git이 파일 소유권 정보를 확인하기 어려운 위치에서는 아래 에러가 날 수 있다.

```text
fatal: detected dubious ownership in repository at 'F:/proj_boot/git_practice/vibe-pr-practice'
```

이 경우 저장소가 망가진 것이 아니다. Git이 안전 확인을 요구하는 것이다.

아래 명령을 한 번 실행한다.

```bash
git config --global --add safe.directory F:/proj_boot/git_practice/vibe-pr-practice
```

그 다음 다시 확인한다.

```bash
git status
```

정상으로 돌아오면 0-3 단계에서 실패했던 명령부터 다시 실행한다.

예를 들어 `git init`은 이미 성공했고 `git branch -M main`에서 실패했다면, 다시 `git init`부터 할 필요 없이 아래부터 이어간다.

```bash
git branch -M main
git config user.name "llmragdev"
git config user.email "llmragdev@gmail.com"
git status
git add .
git commit -m "chore: init practice project"
```

`git remote add origin ...`도 실패했었다면 remote가 아직 등록되지 않았을 가능성이 높다. 아래로 확인한다.

```bash
git remote -v
```

아무것도 안 나오면 다시 등록한다.

```bash
git remote add origin https://github.com/llmragdev/vibe-pr-practice.git
```

### 0-4. GitHub에 빈 저장소 만들기

방법 A: GitHub 웹에서 만들기

1. `llmragdev@gmail.com` 계정으로 GitHub 접속
2. New repository 클릭
3. Repository name: `vibe-pr-practice`
4. Public 또는 Private 선택
5. README, `.gitignore`, license는 체크하지 않는다.
6. Create repository 클릭

방법 B: GitHub CLI로 만들기

```bash
gh repo create vibe-pr-practice --private --source . --remote origin
```

웹에서 만들었다면 remote를 직접 연결한다.

```bash
git remote add origin https://github.com/llmragdev/vibe-pr-practice.git
```

이 명령은 branch를 만드는 명령이 아니다. 로컬 저장소와 GitHub 저장소를 연결하는 명령이다.

```text
로컬 저장소  F:\proj_boot\git_practice\vibe-pr-practice
원격 저장소  https://github.com/llmragdev/vibe-pr-practice
```

처음 GitHub에 올릴 때는 작업 branch를 만들 필요가 없다. 관리자 계정의 초기 업로드는 `main` 브랜치 하나만 사용한다.

초기 업로드 흐름:

```bash
git init
git branch -M main
git add .
git commit -m "chore: init practice project"
git remote add origin https://github.com/llmragdev/vibe-pr-practice.git
git push -u origin main
```

작업 branch는 나중에 작업자 폴더에서 만든다.

```bash
git checkout -b vibe/worker/frontend-greet-exclamation-1
```

정리:

```text
관리자 초기 업로드: main만 사용
작업자 기능 수정: 별도 작업 branch 사용
PR 방향: 작업 branch -> main
```

이미 remote가 있는지 확인:

```bash
git remote -v
```

### 0-5. 첫 push

```bash
git push -u origin main
```

GitHub 웹에서 `vibe-pr-practice` 저장소를 열었을 때 `README.md`, `frontend`, `backend`, `docs`, `task` 폴더가 보이면 성공이다.

### 0-6. 두 번째 계정 초대

관리자 계정에서 GitHub 웹으로 진행한다.

1. 저장소 `vibe-pr-practice` 이동
2. Settings
3. Collaborators
4. Add people
5. `namkyuglobal2@gmail.com` 또는 해당 GitHub username 초대
6. 두 번째 계정에서 초대 수락

초대가 끝나야 작업자 계정이 branch push와 PR 생성을 할 수 있다.

### 0-7. 작업자 계정에서 clone

작업자 계정으로 GitHub CLI 로그인 상태를 바꾼다.

```bash
gh auth logout
gh auth login
gh auth status
```

작업자용 다른 폴더에 clone한다. 관리자 로컬 폴더와 섞지 않는 것이 좋다.

```bash
cd F:\proj_boot\git_practice
git clone https://github.com/llmragdev/vibe-pr-practice.git vibe-pr-practice-worker
cd vibe-pr-practice-worker
git config user.name "namkyuglobal2"
git config user.email "namkyuglobal2@gmail.com"
git status
```

이제 관리자 폴더와 작업자 폴더가 분리된다.

```text
F:\proj_boot\git_practice\vibe-pr-practice          관리자용
F:\proj_boot\git_practice\vibe-pr-practice-worker   작업자용
```

처음에는 이 분리가 매우 중요하다. 같은 폴더에서 계정만 바꿔가며 연습하면 어느 계정이 만든 commit인지 헷갈리기 쉽다.

## VS Code에서 열 폴더

VS Code도 역할별로 여는 폴더를 분리한다.

### 관리자 계정 작업 시 열 위치

`llmragdev@gmail.com` 역할로 작업할 때 VS Code에서 여는 폴더:

```text
F:\proj_boot\git_practice\vibe-pr-practice
```

VS Code 메뉴:

```text
File > Open Folder... > F:\proj_boot\git_practice\vibe-pr-practice
```

또는 터미널:

```powershell
code F:\proj_boot\git_practice\vibe-pr-practice
```

관리자 폴더에서 하는 일:

- `main` 브랜치 최신 상태 확인
- Issue 작성 내용 관리
- `task/*.md`, `review/*.md` 작성 또는 정리
- PR 확인
- 리뷰 댓글 작성
- PR merge 후 `git pull`
- 완료된 task를 `task_done/`으로 이동

관리자 폴더에서 하지 않는 일:

- 작업 branch에서 기능 코드 수정
- 작업자 역할의 PR 생성
- `frontend/app.js`, `backend/api.js`, `docs/usage.md`를 작업 요구사항 해결 목적으로 직접 수정

확인 명령:

```bash
git config user.email
git branch --show-current
git status
```

기대값:

```text
user.email = llmragdev@gmail.com
branch = main
```

### 작업자 계정 작업 시 열 위치

`namkyuglobal2@gmail.com` 역할로 작업할 때 VS Code에서 여는 폴더:

```text
F:\proj_boot\git_practice\vibe-pr-practice-worker
```

VS Code 메뉴:

```text
File > Open Folder... > F:\proj_boot\git_practice\vibe-pr-practice-worker
```

또는 터미널:

```powershell
code F:\proj_boot\git_practice\vibe-pr-practice-worker
```

작업자 폴더에서 하는 일:

- `main`에서 최신 코드 pull
- 작업 branch 생성
- 실제 코드 수정
- commit
- push
- PR 생성

작업자 폴더에서 하지 않는 일:

- `main`에 직접 commit
- 관리자 역할의 task 정리
- merge된 것처럼 `task_done/`으로 파일만 이동
- PR이 생성되기 전 작업 branch 삭제

확인 명령:

```bash
git config user.email
git branch --show-current
git status
```

기대값:

```text
user.email = namkyuglobal2@gmail.com
branch = main 또는 vibe/worker/...
```

작업 시작 전에는 `main`, 실제 수정 중에는 `vibe/worker/...` 브랜치여야 한다.

### VS Code 창 제목으로 구분하기

동시에 두 VS Code 창을 열어도 된다.

```text
[관리자 창] vibe-pr-practice
[작업자 창] vibe-pr-practice-worker
```

작업 전 항상 왼쪽 Explorer 최상단 폴더명을 확인한다.

```text
vibe-pr-practice          관리자
vibe-pr-practice-worker   작업자
```

이름이 비슷해서 헷갈리면 폴더 이름을 더 노골적으로 바꿔도 된다.

```text
vibe-pr-practice-admin
vibe-pr-practice-worker
```

이 경우 GitHub 저장소 이름은 그대로 `vibe-pr-practice`여도 된다. 로컬 폴더 이름만 다를 뿐이다.

## 연습 저장소

관리자 계정에서 새 저장소를 만든다.

```text
vibe-pr-practice
```

권장 초기 구조:

```text
vibe-pr-practice/
  README.md
  frontend/
    app.js
  backend/
    api.js
  docs/
    usage.md
  task/
    frontend/
      issue-1.md
      review-1.md
    backend/
      issue-2.md
      review-2.md
    docs/
      issue-3.md
      review-3.md
  task_done/
```

이 구조는 현재 `wyhil/task/{project}/issue-N.md`, `review-N.md` 구조를 작게 흉내 낸 것이다.

## 초기 파일 예시

### `frontend/app.js`

```js
function greet(name) {
  return `Hello, ${name}`;
}

console.log(greet("world"));
```

### `backend/api.js`

```js
function health() {
  return { status: "ok" };
}

module.exports = { health };
```

### `docs/usage.md`

```markdown
# Usage

Run the sample files with Node.js.
```

## 3개 Issue 연습 목표

| 번호 | 작업 폴더 | Issue 제목 | 수정 파일 | 작업 내용 |
|------|-----------|------------|-----------|-----------|
| #1 | `frontend` | `frontend/greet-exclamation` | `frontend/app.js` | `Hello, world!`처럼 느낌표 추가 |
| #2 | `backend` | `backend/health-version` | `backend/api.js` | health 응답에 `version: "1.0.0"` 추가 |
| #3 | `docs` | `docs/add-run-guide` | `docs/usage.md` | 실행 방법 문서 추가 |

각 작업은 반드시 별도 branch와 별도 PR로 진행한다.

## 절대 규칙

작업 완료 전 금지:

```bash
git mv task/frontend/issue-1.md task_done/frontend/issue-1.md
git mv task/backend/issue-2.md task_done/backend/issue-2.md
git mv task/docs/issue-3.md task_done/docs/issue-3.md
git reset --hard
git clean -fd
git branch -D 작업브랜치
```

`task_done/` 이동은 마지막 정리다.

## 에이전트에게 줄 공통 안전 프롬프트

작업 지시:

```text
이 task md 파일은 작업 지시서다.
task 파일을 먼저 task_done으로 옮기지 마.
main 브랜치에서 직접 수정하지 마.
별도 작업 브랜치를 만들고, md 안의 작업 요구사항을 수행하고, 코드 변경 커밋을 만든 뒤 PR을 올려.
PR URL을 확인하기 전까지 task 파일 정리를 하지 마.
reset, clean, branch 삭제, task_done 이동 전에는 이유를 설명하고 확인을 받아.
```

리뷰 지시:

```text
이 review md 파일은 리뷰 지시서다.
review 파일을 먼저 task_done으로 옮기지 마.
먼저 PR 본문과 diff를 확인하고 리뷰 내용을 작성해.
PR 댓글 등록이 성공한 뒤에만 task 파일 정리를 해.
```

## 관리자 계정 준비

관리자 계정:

```text
llmragdev@gmail.com
```

로컬 설정:

```bash
git config user.name "llmragdev"
git config user.email "llmragdev@gmail.com"
```

GitHub에서 할 일:

1. `vibe-pr-practice` 저장소 생성
2. 위 초기 구조 commit
3. 작업자 계정 `namkyuglobal2@gmail.com`을 collaborator로 초대
4. Issue #1, #2, #3 생성

## 작업자 계정 준비

작업자 계정:

```text
namkyuglobal2@gmail.com
```

로컬 설정:

```bash
git config user.name "namkyuglobal2"
git config user.email "namkyuglobal2@gmail.com"
```

clone:

```bash
git clone https://github.com/llmragdev/vibe-pr-practice.git
cd vibe-pr-practice
```

## Issue #1: frontend 작업

### GitHub Issue 본문

```markdown
## 작업 요구사항

`frontend/app.js`의 `greet(name)` 함수가 `Hello, world!`처럼 끝에 느낌표를 붙이도록 수정한다.

## 완료 조건

- `greet("world")` 결과가 `Hello, world!`가 된다.
- 변경 커밋은 1개만 만든다.
- PR 본문에 `References #1`을 포함한다.
```

### `task/frontend/issue-1.md`

```markdown
# frontend/greet-exclamation 작업 지시 #1

## 금지

- 이 파일을 먼저 `task_done/`으로 옮기지 않는다.
- `main`에서 직접 작업하지 않는다.

## 작업 요구사항

`frontend/app.js`의 `greet(name)` 함수가 `Hello, world!`처럼 끝에 느낌표를 붙이도록 수정한다.

## 명령

git fetch origin
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/frontend-greet-exclamation-1

수정 후:

git status
git add frontend/app.js
git commit -m "feat: add exclamation to frontend greeting"
git push -u origin vibe/worker/frontend-greet-exclamation-1

PR:

gh pr create --base main --head vibe/worker/frontend-greet-exclamation-1 --title "feat: add exclamation to frontend greeting #1" --body "References #1"
```

## Issue #2: backend 작업

### GitHub Issue 본문

```markdown
## 작업 요구사항

`backend/api.js`의 `health()` 응답에 `version: "1.0.0"`을 추가한다.

## 완료 조건

- `health()`가 `{ status: "ok", version: "1.0.0" }` 형태를 반환한다.
- 변경 커밋은 1개만 만든다.
- PR 본문에 `References #2`를 포함한다.
```

### `task/backend/issue-2.md`

```markdown
# backend/health-version 작업 지시 #2

## 금지

- 이 파일을 먼저 `task_done/`으로 옮기지 않는다.
- `main`에서 직접 작업하지 않는다.

## 작업 요구사항

`backend/api.js`의 `health()` 응답에 `version: "1.0.0"`을 추가한다.

## 명령

git fetch origin
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/backend-health-version-2

수정 후:

git status
git add backend/api.js
git commit -m "feat: add version to health response"
git push -u origin vibe/worker/backend-health-version-2

PR:

gh pr create --base main --head vibe/worker/backend-health-version-2 --title "feat: add version to health response #2" --body "References #2"
```

## Issue #3: docs 작업

### GitHub Issue 본문

```markdown
## 작업 요구사항

`docs/usage.md`에 Node.js로 샘플 파일을 실행하는 방법을 추가한다.

## 완료 조건

- `node frontend/app.js` 실행 예시가 문서에 있다.
- 변경 커밋은 1개만 만든다.
- PR 본문에 `References #3`을 포함한다.
```

### `task/docs/issue-3.md`

```markdown
# docs/add-run-guide 작업 지시 #3

## 금지

- 이 파일을 먼저 `task_done/`으로 옮기지 않는다.
- `main`에서 직접 작업하지 않는다.

## 작업 요구사항

`docs/usage.md`에 Node.js로 샘플 파일을 실행하는 방법을 추가한다.

## 명령

git fetch origin
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/docs-add-run-guide-3

수정 후:

git status
git add docs/usage.md
git commit -m "docs: add sample run guide"
git push -u origin vibe/worker/docs-add-run-guide-3

PR:

gh pr create --base main --head vibe/worker/docs-add-run-guide-3 --title "docs: add sample run guide #3" --body "References #3"
```

## 3개 PR을 올리는 순서

작업자 계정에서 하나씩 진행한다.

```bash
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/frontend-greet-exclamation-1
```

Issue #1 수정, commit, push, PR 생성.

그 다음 반드시 다시 `main` 최신 상태에서 시작한다.

```bash
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/backend-health-version-2
```

Issue #2 수정, commit, push, PR 생성.

다시 `main` 최신 상태에서 시작한다.

```bash
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/docs-add-run-guide-3
```

Issue #3 수정, commit, push, PR 생성.

## Review 파일 3개 예시

### `task/frontend/review-1.md`

```markdown
# frontend PR 리뷰 지시 #1

## 금지

- 이 파일을 먼저 `task_done/`으로 옮기지 않는다.
- PR diff를 보기 전에는 리뷰 댓글을 쓰지 않는다.

## 확인

gh pr view PR번호 --comments
gh pr diff PR번호

## 기준

- `frontend/app.js`만 수정했는가
- `Hello, world!`가 반환되는가
- PR 본문에 `References #1`이 있는가

## 댓글 등록

gh pr comment PR번호 --body-file review-1-result.md
```

### `task/backend/review-2.md`

```markdown
# backend PR 리뷰 지시 #2

## 확인

gh pr view PR번호 --comments
gh pr diff PR번호

## 기준

- `backend/api.js`만 수정했는가
- health 응답에 `version: "1.0.0"`이 있는가
- PR 본문에 `References #2`가 있는가

## 댓글 등록

gh pr comment PR번호 --body-file review-2-result.md
```

### `task/docs/review-3.md`

```markdown
# docs PR 리뷰 지시 #3

## 확인

gh pr view PR번호 --comments
gh pr diff PR번호

## 기준

- `docs/usage.md`만 수정했는가
- `node frontend/app.js` 실행 예시가 있는가
- PR 본문에 `References #3`이 있는가

## 댓글 등록

gh pr comment PR번호 --body-file review-3-result.md
```

## Merge 순서

관리자 계정에서 PR을 하나씩 확인하고 merge한다.

```bash
gh pr list --state open
gh pr view PR번호 --comments
gh pr diff PR번호
gh pr merge PR번호 --squash --delete-branch
```

웹에서 할 경우 `Squash and merge`를 선택한다.

## task_done 정리

아래 조건이 모두 만족될 때만 task 파일을 옮긴다.

- Issue 작업 PR이 생성되었다.
- 리뷰 댓글이 등록되었다.
- PR이 merge되었거나 close하기로 결정했다.

정리 예시:

```bash
mkdir -p task_done/frontend task_done/backend task_done/docs
git mv task/frontend/issue-1.md task_done/frontend/issue-1.md
git mv task/frontend/review-1.md task_done/frontend/review-1.md
git mv task/backend/issue-2.md task_done/backend/issue-2.md
git mv task/backend/review-2.md task_done/backend/review-2.md
git mv task/docs/issue-3.md task_done/docs/issue-3.md
git mv task/docs/review-3.md task_done/docs/review-3.md
git commit -m "chore: move completed practice tasks"
git push origin main
```

## 작업 취소 연습

### 커밋 전 취소

```bash
git status
git restore frontend/app.js
git status
```

새 파일 삭제 전 미리 보기:

```bash
git clean -fdn
```

실제 삭제:

```bash
git clean -fd
```

### 커밋 후 push 전 취소

커밋만 취소하고 변경은 남긴다.

```bash
git reset --soft HEAD~1
```

커밋과 변경을 모두 버린다.

```bash
git reset --hard HEAD~1
```

### push 후 PR 전 취소

```bash
git push origin --delete vibe/worker/frontend-greet-exclamation-1
git checkout main
git branch -D vibe/worker/frontend-greet-exclamation-1
```

### PR 후 취소

```bash
gh pr close PR번호 --comment "연습 취소: 다시 작업 예정"
git push origin --delete 브랜치명
```

## 폴더가 사라진 것처럼 보일 때

대부분 실제 삭제가 아니라 다른 브랜치에 있는 상태다. 먼저 확인한다.

```bash
git branch --show-current
git status
```

`main`으로 복구:

```bash
git fetch origin
git checkout main
git reset --hard origin/main
```

원격 작업 브랜치로 복구:

```bash
git fetch origin
git checkout -b vibe/worker/frontend-greet-exclamation-1 origin/vibe/worker/frontend-greet-exclamation-1
```

## PR이 안 올라갔을 때 확인 순서

```bash
git branch --show-current
git status
git log --oneline -5
git branch -vv
gh pr list --state all
```

확인할 것:

- 현재 브랜치가 `main`이 아닌 작업 브랜치인가
- commit이 실제로 있는가
- `git push -u origin 브랜치명`을 했는가
- `gh pr create`가 성공했고 URL을 출력했는가
- PR base가 `main`인가

## 추천 연습 순서

1. Issue #1만 손으로 끝까지 해본다.
2. Issue #2는 일부러 push 후 PR 전 취소를 연습한다.
3. Issue #2를 다시 branch부터 만들어 정상 PR을 올린다.
4. Issue #3은 PR까지 만든 뒤 close와 branch 삭제를 연습한다.
5. Issue #3을 다시 정상 PR로 올리고 merge한다.
6. 마지막에만 `task_done/` 이동을 한다.

이 과정을 한 번 돌리면, 에이전트가 어디서 멈췄는지 `branch`, `commit`, `push`, `PR`, `review comment`, `merge` 단계별로 확인할 수 있다.
