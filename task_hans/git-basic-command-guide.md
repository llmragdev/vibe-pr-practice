# Git Basic Command Guide

Git 기본 명령어를 아직 외우지 않아도 실습을 진행할 수 있도록 만든 짧은 참고 문서입니다.

## 1. 현재 상태 확인

가장 자주 쓰는 명령어입니다.

```powershell
git status
```

의미:

- 현재 브랜치가 무엇인지 확인합니다.
- 수정된 파일이 있는지 확인합니다.
- `git add` 된 파일과 아직 안 된 파일을 구분해서 보여줍니다.

예시:

```text
Changes not staged for commit:
  modified: README.md
```

뜻:

- `README.md`가 수정됐지만 아직 커밋 대상으로 올리지 않았습니다.

## 2. 변경 파일을 커밋 대상으로 올리기

특정 파일만 올릴 때:

```powershell
git add README.md
```

여러 파일을 올릴 때:

```powershell
git add README.md task_hans/pr-branch-quick-guide.md
```

현재 폴더 아래 모든 변경을 올릴 때:

```powershell
git add .
```

의미:

- `git add`는 파일을 바로 커밋하는 명령이 아닙니다.
- 다음 커밋에 포함할 파일을 고르는 명령입니다.
- 이 상태를 staged 상태라고 부릅니다.

## 3. 커밋 만들기

커밋 메시지를 명령어에 바로 적는 방식입니다.

```powershell
git commit -m "docs: clarify PR practice roles"
```

의미:

- staged 된 변경사항을 하나의 커밋으로 저장합니다.
- `-m`은 commit message를 바로 입력하겠다는 뜻입니다.

좋은 커밋 메시지 예시:

```text
docs: add git basic command guide
docs: clarify PR practice roles
feat: add exclamation to frontend greeting
fix: correct health response version
chore: move completed practice tasks
```

자주 쓰는 접두어:

- `docs:` 문서 변경
- `feat:` 기능 추가
- `fix:` 버그 수정
- `chore:` 정리 작업, 설정 변경, 파일 이동

## 4. `git commit -a` 주의

```powershell
git commit -a
```

의미:

- 이미 Git이 추적 중인 수정 파일을 자동으로 staged 처리하고 커밋을 시작합니다.
- 새 파일은 포함하지 않습니다.
- 커밋 메시지를 쓰기 위해 편집기가 열릴 수 있습니다.

주의:

- 편집기에서 메시지를 쓰지 않고 닫으면 커밋이 취소됩니다.
- 이때 다음 메시지가 나올 수 있습니다.

```text
Aborting commit due to empty commit message.
```

초보자는 보통 아래 방식을 추천합니다.

```powershell
git add .
git commit -m "커밋 메시지"
```

## 5. GitHub에 올리기

로컬 커밋을 GitHub에 올립니다.

```powershell
git push
```

처음 만든 브랜치를 GitHub에 올릴 때:

```powershell
git push -u origin BRANCH_NAME
```

예시:

```powershell
git push -u origin vibe/worker/frontend-greet-exclamation-1
```

의미:

- `origin`: GitHub 원격 저장소 이름입니다.
- `BRANCH_NAME`: 올릴 브랜치 이름입니다.
- `-u`: 다음부터 이 브랜치에서는 `git push`만 쳐도 같은 원격 브랜치로 올라가게 연결합니다.

## 6. GitHub 내용 가져오기

`main`을 최신 상태로 맞출 때:

```powershell
git checkout main
git pull --ff-only origin main
```

의미:

- `git checkout main`: `main` 브랜치로 이동합니다.
- `git pull --ff-only origin main`: GitHub의 `main` 변경사항을 로컬로 가져옵니다.
- `--ff-only`: 예상치 못한 merge commit이 생기지 않게 막는 안전 옵션입니다.

성공 예시:

```text
Already up to date.
```

뜻:

- 이미 로컬과 GitHub가 같은 상태입니다.

## 7. 커밋 기록 보기

최근 커밋을 짧게 봅니다.

```powershell
git log --oneline -5
```

예시:

```text
693fc72 docs: add practice task notes
1b5559c chore: init practice project
```

의미:

- 왼쪽 짧은 문자열은 커밋 ID입니다.
- 오른쪽은 커밋 메시지입니다.

## 8. 브랜치 확인과 이동

현재 브랜치 확인:

```powershell
git branch --show-current
```

브랜치 이동:

```powershell
git checkout main
```

새 브랜치 만들고 이동:

```powershell
git checkout -b vibe/worker/frontend-greet-exclamation-1
```

의미:

- `checkout -b`는 새 브랜치를 만들고 바로 그 브랜치로 이동합니다.

## 9. 변경 취소

아직 커밋하지 않은 특정 파일 변경을 버립니다.

```powershell
git restore FILE_PATH
```

예시:

```powershell
git restore frontend/app.js
```

주의:

- 파일 수정 내용이 사라집니다.
- 실행 전에 반드시 `git status`로 확인하세요.

## 10. 자주 쓰는 기본 흐름

문서 수정 후 커밋하고 push:

```powershell
git status
git add README.md task_hans/pr-branch-quick-guide.md
git commit -m "docs: clarify PR practice roles"
git push
git status
```

새 문서까지 포함해서 커밋하고 push:

```powershell
git status
git add README.md task_hans/pr-branch-quick-guide.md task_hans/git-basic-command-guide.md
git commit -m "docs: add git basic command guide"
git push
git status
```

작업 브랜치에서 코드 수정 후 PR 준비:

```powershell
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/frontend-greet-exclamation-1
git status
git add frontend/app.js
git commit -m "feat: add exclamation to frontend greeting"
git push -u origin vibe/worker/frontend-greet-exclamation-1
```
