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

## 11. 임시로 변경사항 숨기기: `git stash`

아직 커밋하기 애매한 수정사항이 있는데, 브랜치를 바꾸거나 `git pull`을 해야 할 때 사용합니다.

대표 상황:

```text
파일을 수정했다.
아직 커밋하고 싶지는 않다.
그런데 다른 브랜치로 이동해야 한다.
```

현재 수정사항을 임시 보관합니다.

```powershell
git stash
```

stash 목록을 확인합니다.

```powershell
git stash list
```

임시 보관한 변경사항을 다시 꺼냅니다.

```powershell
git stash pop
```

stash를 적용하되 목록에는 남겨둡니다.

```powershell
git stash apply
```

stash 하나를 삭제합니다.

```powershell
git stash drop
```

자주 쓰는 흐름:

```powershell
git status
git stash
git checkout main
git pull --ff-only origin main
git stash pop
git status
```

명령어 의미:

- `git stash`: 현재 수정사항을 임시 저장하고 working tree를 깨끗하게 만듭니다.
- `git stash list`: 임시 저장 목록을 봅니다.
- `git stash pop`: 가장 최근 stash를 다시 적용하고 stash 목록에서 제거합니다.
- `git stash apply`: 가장 최근 stash를 다시 적용하지만 stash 목록에는 남겨둡니다.
- `git stash drop`: stash를 삭제합니다.

주의:

- `git stash`는 tracked 파일의 수정사항을 주로 숨깁니다.
- 새 파일까지 함께 숨기려면 `git stash -u`를 사용합니다.
- `git stash pop`을 했을 때 충돌이 날 수 있습니다. 이 경우 `git status`로 충돌 파일을 확인합니다.

새 파일까지 포함해서 숨기기:

```powershell
git stash -u
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
```

## 💡 Git Config와 GitHub 계정의 관계 (중요)

많은 실습자가 `gh auth login`으로 로그인했는데 왜 `git config`를 또 설정해야 하는지 궁금해합니다.

### 1. 로컬 도장(Git) vs 온라인 신분증(GitHub)
*   **Git Config**: 여러분의 컴퓨터에서 작성하는 모든 커밋에 찍히는 **'도장'**입니다. 오프라인에서도 작동하며, 어떤 이름을 써도 제한이 없습니다.
*   **GitHub**: 푸시된 커밋을 받아서 보여주는 **'게시판'**입니다.

### 2. 왜 이메일(`user.email`)이 중요한가? (Contribution Matching)
GitHub은 푸시된 커밋의 **이메일 주소**를 보고 자신의 사용자 DB에서 해당 사용자를 찾습니다.
*   **이메일이 일치하면**: 커밋 옆에 여러분의 **프로필 사진**이 뜨고, 깃허브 메인 페이지의 **잔디(Contribution Graph)**가 심어집니다.
*   **이메일이 다르면**: 깃허브는 이 커밋이 누구의 것인지 알 수 없어 **회색 기본 아이콘**으로 표시하며, 잔디도 심어지지 않습니다.

### 3. 이름(`user.name`) 설정 권장 사항
이름은 기술적으로 잔디 심기에 영향을 주지 않지만, 코드 리뷰 시 **누가 작업했는지 직관적으로 알 수 있게** GitHub ID와 일치시키거나 실명을 사용하는 것이 실무 표준입니다.

> **결론**: 실습 시 `user.name`과 `user.email`을 현재 역할(Manager/Worker)의 GitHub 정보와 일치시키는 것은, GitHub이 여러분의 기여를 정확히 기록하고 프로필을 연결하게 만들기 위함입니다.

```powershell
# 현재 저장소에만 적용
git config user.name "내이름"
git config user.email "내이메일@example.com"

# 모든 저장소에 공통 적용하려면 --global 추가
git config --global user.name "내이름"
```

## 11. GitHub CLI 계정과 Git 작성자 계정의 차이

실무에서 `gh auth status`로 확인한 계정과 `git log`에 찍히는 계정이 달라서 당황하는 경우가 많습니다. 이 둘은 서로 별개의 설정입니다.

### 1) 계정의 종류
- **GitHub CLI (`gh`)**: GitHub 서버와 통신(PR 생성, Issue 관리 등)하기 위한 **인증용 계정**입니다.
- **Git Config (`user.name`, `user.email`)**: 내 컴퓨터에서 생성되는 커밋에 "이 코드는 누가 썼다"라고 표시하는 **작성자 정보**입니다.

### 2) 현재 설정 확인
```powershell
# GitHub 인증 계정 확인
gh auth status

# 로컬 Git 작성자 정보 확인
git config user.name
git config user.email
```

### 3) 작성자 정보 수정 방법
만약 작성자 정보가 잘못되어 있다면 아래 명령어로 수정합니다.

```powershell
# 현재 저장소에만 적용
git config user.name "내이름"
git config user.email "내이메일@example.com"

# 모든 저장소에 공통 적용하려면 --global 추가
git config --global user.name "내이름"
```

### 4) 이미 잘못 올라간 커밋 수정하기
커밋을 이미 했는데 작성자 정보가 틀렸다면, 가장 최근 커밋에 한해 수정이 가능합니다.

```powershell
# 1. 마지막 커밋의 작성자 수정 (로컬)
git commit --amend --author="이름 <이메일>" --no-edit

# 2. GitHub에 이미 푸시했다면 강제 푸시 필요 (주의!)
git push origin main --force
```
