# PR/브랜치 연습 중 에러 대응 절차

이 문서는 `vibe-pr-practice` 연습 중 자주 만나는 Git/GitHub 에러를 보고, 어디서 멈췄는지 판단하는 용도다.

기본 원칙:

- 에러가 나면 다음 명령으로 넘어가지 않는다.
- 먼저 `git status`, `git branch --show-current`, `git remote -v`를 확인한다.
- `reset --hard`, `clean -fd`, branch 삭제는 원인을 이해한 뒤 실행한다.
- `task_done/` 이동은 PR/리뷰/merge가 끝난 뒤 마지막에 한다.

## 1. 현재 위치부터 확인

먼저 내가 어느 폴더에서 작업 중인지 확인한다.

```powershell
Get-Location
```

관리자 폴더:

```text
F:\proj_boot\git_practice\vibe-pr-practice
```

작업자 폴더:

```text
F:\proj_boot\git_practice\vibe-pr-practice-worker
```

Git 상태 확인:

```bash
git status
git branch --show-current
git config user.email
git remote -v
```

## 2. `detected dubious ownership` 에러

에러 예시:

```text
fatal: detected dubious ownership in repository at 'F:/proj_boot/git_practice/vibe-pr-practice'
'F:/proj_boot/git_practice/vibe-pr-practice' is on a file system that does not record ownership
```

뜻:

Git이 이 폴더를 안전한 저장소로 아직 신뢰하지 않는다는 뜻이다. `F:` 드라이브 같은 위치에서 자주 난다. 저장소가 망가진 것은 아니다.

해결:

```bash
git config --global --add safe.directory F:/proj_boot/git_practice/vibe-pr-practice
```

작업자 폴더에서 같은 에러가 나면:

```bash
git config --global --add safe.directory F:/proj_boot/git_practice/vibe-pr-practice-worker
```

그 다음:

```bash
git status
```

정상으로 나오면 실패했던 명령부터 다시 실행한다.

예를 들어 `git branch -M main`에서 실패했다면:

```bash
git branch -M main
git config user.name "llmragdev"
git config user.email "llmragdev@gmail.com"
git status
```

## 3. `fatal: not in a git directory`

에러 예시:

```text
fatal: not in a git directory
```

주요 원인:

- 현재 폴더가 Git 저장소가 아니다.
- `dubious ownership` 에러 때문에 Git이 저장소로 취급하지 못하고 있다.
- 잘못된 폴더에서 명령을 실행했다.

확인:

```powershell
Get-Location
Get-ChildItem -Force
```

`.git` 폴더가 있는지 확인한다.

해결 순서:

```bash
git status
```

`dubious ownership`가 같이 나오면 먼저 safe.directory를 등록한다.

```bash
git config --global --add safe.directory F:/proj_boot/git_practice/vibe-pr-practice
```

`.git` 폴더가 정말 없다면 아직 `git init`을 안 한 것이다.

```bash
git init
```

## 4. `No commits yet`

상태 예시:

```text
On branch main

No commits yet

Changes to be committed:
  new file: README.md
```

또는:

```text
fatal: your current branch 'main' does not have any commits yet
```

뜻:

`git add .`까지는 했지만 아직 첫 commit을 만들지 않았다는 뜻이다. 이 상태에서 `git log`를 보면 에러가 나는 것이 정상이다.

해결:

```bash
git commit -m "chore: init practice project"
```

성공 확인:

```bash
git status
git log --oneline -3
```

정상 상태:

```text
nothing to commit, working tree clean
```

## 5. `git remote -v`가 아무것도 안 나옴

상태:

```powershell
git remote -v
```

결과가 아무것도 없으면 remote가 아직 등록되지 않은 것이다.

해결:

```bash
git remote add origin https://github.com/llmragdev/vibe-pr-practice.git
```

확인:

```bash
git remote -v
```

정상 예시:

```text
origin  https://github.com/llmragdev/vibe-pr-practice.git (fetch)
origin  https://github.com/llmragdev/vibe-pr-practice.git (push)
```

## 6. `remote origin already exists`

에러 예시:

```text
error: remote origin already exists.
```

뜻:

이미 `origin`이 등록되어 있다.

확인:

```bash
git remote -v
```

주소가 맞으면 그대로 다음 단계로 간다.

```bash
git push -u origin main
```

주소가 틀리면 수정한다.

```bash
git remote set-url origin https://github.com/llmragdev/vibe-pr-practice.git
git remote -v
```

## 7. 첫 push가 실패할 때

실행:

```bash
git push -u origin main
```

### 7-1. 원격 저장소가 없다는 에러

예시:

```text
Repository not found.
```

확인:

- GitHub 웹에서 `https://github.com/llmragdev/vibe-pr-practice`가 실제로 있는가
- 저장소 이름 철자가 맞는가
- `gh auth status` 계정이 `llmragdev`인가

확인 명령:

```bash
gh auth status
git remote -v
```

### 7-2. 권한 에러

예시:

```text
Permission denied
```

확인:

```bash
gh auth status
```

관리자 초기 push는 `llmragdev` 계정이어야 한다.

계정이 다르면:

```bash
gh auth logout
gh auth login
```

## 8. 줄바꿈 warning

warning 예시:

```text
warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
```

뜻:

Windows 줄바꿈 설정 때문에 나오는 경고다. 연습 중에는 대부분 무시해도 된다.

계속 신경 쓰이면 저장소에 `.gitattributes`를 추가할 수 있다.

```text
* text=auto
```

하지만 지금 연습에서는 필수는 아니다.

## 9. PR이 안 올라갔을 때

확인 순서:

```bash
git branch --show-current
git status
git log --oneline -5
git remote -v
git branch -vv
gh pr list --state all
```

체크할 것:

- 현재 브랜치가 `main`이 아니라 `vibe/worker/...`인가
- commit이 있는가
- push를 했는가
- `gh pr create`를 실행했는가
- PR URL이 출력되었는가

작업 branch push:

```bash
git push -u origin vibe/worker/frontend-greet-exclamation-1
```

PR 생성:

```bash
gh pr create --base main --head vibe/worker/frontend-greet-exclamation-1 --title "feat: add exclamation to frontend greeting #1" --body "References #1"
```

## 10. 다른 폴더가 사라진 것처럼 보일 때

대부분 실제 삭제가 아니라 다른 branch를 보고 있는 상태다.

확인:

```bash
git branch --show-current
git status
Get-ChildItem
```

관리자 main으로 복귀:

```bash
git checkout main
git pull --ff-only origin main
```

원격 main 기준으로 강제 복구가 필요할 때:

```bash
git fetch origin
git reset --hard origin/main
```

주의:

`git reset --hard`는 현재 로컬 변경을 버린다. 연습 중 정말 버려도 되는 변경인지 확인하고 실행한다.

## 11. 작업 취소

### commit 전 취소

```bash
git status
git restore frontend/app.js
```

새 파일 삭제 미리 보기:

```bash
git clean -fdn
```

실제 삭제:

```bash
git clean -fd
```

### commit 후 push 전 취소

commit만 취소하고 변경은 남긴다.

```bash
git reset --soft HEAD~1
```

commit과 변경을 모두 버린다.

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

## 12. 에러가 났을 때 보고할 정보

누군가에게 도움을 요청할 때 아래 5개 결과를 같이 보여주면 빠르다.

```bash
git status
git branch --show-current
git log --oneline -5
git remote -v
gh auth status
```

아직 commit이 하나도 없으면 `git log`는 실패할 수 있다. 그 경우 `No commits yet`이라고 말하면 된다.
