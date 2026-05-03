# PR Branch Practice Quick Guide

나중에 `vibe-pr-practice` 저장소에서 혼자 다시 연습할 때 따라 하는 짧은 절차입니다.

## 0. 시작 상태 확인

```powershell
git checkout main
git pull --ff-only origin main
git status
git log --oneline -3
git remote -v
```

명령어 설명:

- `git checkout main`: 현재 작업 위치를 `main` 브랜치로 이동합니다.
- `git pull --ff-only origin main`: GitHub의 `origin/main` 내용을 로컬 `main`으로 가져옵니다.
- `--ff-only`: fast-forward 방식으로만 pull 하겠다는 뜻입니다. 내 로컬 `main`에 따로 커밋이 없고 GitHub 쪽만 앞서 있을 때만 업데이트합니다. 꼬인 merge commit을 실수로 만들지 않게 막아주는 안전 옵션입니다.
- `origin`: 원격 저장소 이름입니다. 보통 GitHub repo를 `origin`이라고 부릅니다.
- `main`: 가져올 원격 브랜치 이름입니다.
- `git status`: 현재 브랜치, 변경 파일, 커밋할 파일이 있는지 확인합니다.
- `git log --oneline -3`: 최근 커밋 3개를 한 줄씩 짧게 봅니다.
- `git remote -v`: 연결된 원격 저장소 주소를 확인합니다.

기대 상태:

```text
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

참고:

- `Already up to date.`는 GitHub와 로컬이 이미 같다는 뜻입니다.
- `Untracked files`는 Git이 아직 추적하지 않는 새 파일이 있다는 뜻입니다.
- `task_hans/pr-branch-quick-guide.md`가 untracked로 보이면 아직 `git add`를 하지 않은 상태입니다.
- 문서를 원격에 올리려면 `git add`, `git commit`, `git push`를 하면 됩니다.

## 1. GitHub Issue 만들기

GitHub 저장소의 `Issues` 탭에서 아래 3개를 만듭니다.

### Issue #1

Title:

```text
frontend/greet-exclamation
```

Body:

```markdown
`frontend/app.js`의 `greet(name)` 함수가 `Hello, world!`를 출력하도록 수정한다.

Acceptance Criteria:
- `node frontend/app.js` 실행 결과가 `Hello, world!`이다.
- PR 본문에 `References #1`을 포함한다.
```

### Issue #2

Title:

```text
backend/health-version
```

Body:

```markdown
`backend/api.js`의 `health()` 반환값에 `version: "1.0.0"`을 추가한다.

Acceptance Criteria:
- `health()`가 `{ status: "ok", version: "1.0.0" }`를 반환한다.
- PR 본문에 `References #2`를 포함한다.
```

### Issue #3

Title:

```text
docs/add-run-guide
```

Body:

```markdown
`docs/usage.md`에 Node.js 실행 방법을 추가한다.

Acceptance Criteria:
- `node frontend/app.js` 실행 예시가 문서에 포함된다.
- PR 본문에 `References #3`을 포함한다.
```

## 2. Issue #1 작업

```powershell
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/frontend-greet-exclamation-1
```

명령어 설명:

- `git checkout main`: 작업 브랜치를 만들기 전에 기준 브랜치인 `main`으로 이동합니다.
- `git pull --ff-only origin main`: 최신 `main`을 기준으로 작업하기 위해 GitHub의 변경사항을 가져옵니다.
- `git checkout -b 브랜치명`: 새 브랜치를 만들고 바로 그 브랜치로 이동합니다.
- `vibe/worker/frontend-greet-exclamation-1`: Issue #1 작업용 브랜치 이름입니다. `/`는 폴더가 아니라 브랜치 이름을 보기 좋게 나누는 관례입니다.

`frontend/app.js`를 수정합니다.

```js
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet("world"));
```

검증, 커밋, push:

```powershell
node frontend/app.js
git add frontend/app.js
git commit -m "feat: add exclamation to frontend greeting"
git push -u origin vibe/worker/frontend-greet-exclamation-1
```

명령어 설명:

- `node frontend/app.js`: 수정한 JavaScript 파일을 실행해서 결과를 확인합니다.
- `git add frontend/app.js`: 이 파일 변경분을 다음 커밋에 포함하도록 staged 상태로 올립니다.
- `git commit -m "..."`: staged 된 변경분을 하나의 커밋으로 저장합니다.
- `feat:`: 기능 변경 커밋이라는 의미의 커밋 메시지 접두어입니다.
- `git push -u origin 브랜치명`: 내 로컬 브랜치를 GitHub 원격 저장소에 올립니다.
- `-u`: 다음부터 이 브랜치에서 `git push`만 쳐도 같은 원격 브랜치로 push 되게 연결합니다.

PR 생성:

```powershell
gh pr create --base main --head vibe/worker/frontend-greet-exclamation-1 --title "feat: add exclamation to frontend greeting #1" --body "References #1"
```

명령어 설명:

- `gh pr create`: GitHub CLI로 Pull Request를 만듭니다.
- `--base main`: PR이 합쳐질 대상 브랜치입니다.
- `--head 브랜치명`: PR에 올릴 작업 브랜치입니다.
- `--title`: PR 제목입니다.
- `--body`: PR 본문입니다.
- `References #1`: PR과 Issue #1을 연결합니다. merge만으로 issue를 자동 종료하지는 않고, 연결 참고로 남깁니다.

## 3. Issue #2 작업

```powershell
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/backend-health-version-2
```

명령어 설명:

- Issue #2도 반드시 최신 `main`에서 새 브랜치를 만듭니다.
- 이미 다른 브랜치에 있다면 `git checkout main`으로 돌아온 뒤 시작합니다.
- `vibe/worker/backend-health-version-2`는 backend Issue #2 전용 작업 브랜치입니다.

`backend/api.js`를 수정합니다.

```js
function health() {
  return { status: "ok", version: "1.0.0" };
}

module.exports = { health };
```

커밋, push:

```powershell
git add backend/api.js
git commit -m "feat: add version to health response"
git push -u origin vibe/worker/backend-health-version-2
```

명령어 설명:

- `git add backend/api.js`: backend 파일 변경만 커밋 대상으로 올립니다.
- `git commit -m "..."`
  : health 응답에 version을 추가한 변경을 커밋합니다.
- `git push -u origin vibe/worker/backend-health-version-2`: Issue #2 브랜치를 GitHub에 올립니다.

PR 생성:

```powershell
gh pr create --base main --head vibe/worker/backend-health-version-2 --title "feat: add version to health response #2" --body "References #2"
```

명령어 설명:

- `--base main`: GitHub에서 `main`으로 합치려는 PR입니다.
- `--head vibe/worker/backend-health-version-2`: backend 작업 브랜치의 변경분을 PR로 올립니다.
- `References #2`: Issue #2와 연결합니다.

## 4. Issue #3 작업

```powershell
git checkout main
git pull --ff-only origin main
git checkout -b vibe/worker/docs-add-run-guide-3
```

명령어 설명:

- docs 작업도 최신 `main`에서 시작합니다.
- `vibe/worker/docs-add-run-guide-3`는 docs Issue #3 전용 작업 브랜치입니다.

`docs/usage.md`를 수정합니다.

````markdown
# Usage

Run the sample frontend file with Node.js.

```powershell
node frontend/app.js
```
````

커밋, push:

```powershell
git add docs/usage.md
git commit -m "docs: add sample run guide"
git push -u origin vibe/worker/docs-add-run-guide-3
```

명령어 설명:

- `git add docs/usage.md`: 문서 변경만 커밋 대상으로 올립니다.
- `docs:`: 문서 변경 커밋이라는 의미의 커밋 메시지 접두어입니다.
- `git push -u origin vibe/worker/docs-add-run-guide-3`: docs 작업 브랜치를 GitHub에 올립니다.

PR 생성:

```powershell
gh pr create --base main --head vibe/worker/docs-add-run-guide-3 --title "docs: add sample run guide #3" --body "References #3"
```

명령어 설명:

- `--title "docs: ... #3"`: PR 제목에 Issue 번호를 넣어 어떤 작업인지 바로 보이게 합니다.
- `--body "References #3"`: PR 본문에서 Issue #3을 참조합니다.

## 5. Review 확인

PR 번호를 확인합니다.

```powershell
gh pr list --state open
```

명령어 설명:

- `gh pr list`: PR 목록을 봅니다.
- `--state open`: 아직 열려 있는 PR만 봅니다.

각 PR에서 diff를 봅니다.

```powershell
gh pr view PR_NUMBER --comments
gh pr diff PR_NUMBER
```

명령어 설명:

- `PR_NUMBER`: GitHub PR 번호입니다. 예를 들어 PR #4라면 `4`를 넣습니다.
- `gh pr view PR_NUMBER --comments`: PR 본문과 댓글을 확인합니다.
- `gh pr diff PR_NUMBER`: PR에서 실제로 바뀐 코드를 확인합니다.

리뷰 코멘트를 남깁니다.

```powershell
gh pr comment PR_NUMBER --body "Reviewed. Looks good."
```

명령어 설명:

- `gh pr comment`: PR에 댓글을 남깁니다.
- `--body`: 댓글 내용을 직접 적습니다.

## 6. Merge

각 PR을 확인한 뒤 merge합니다.

```powershell
gh pr merge PR_NUMBER --squash --delete-branch
```

명령어 설명:

- `gh pr merge`: PR을 merge합니다.
- `--squash`: PR 안의 여러 커밋을 하나의 커밋으로 합쳐서 `main`에 넣습니다.
- `--delete-branch`: merge가 끝난 뒤 GitHub의 작업 브랜치를 삭제합니다.

로컬 `main`을 최신화합니다.

```powershell
git checkout main
git pull --ff-only origin main
git status
```

명령어 설명:

- PR을 GitHub에서 merge하면 원격 `main`이 앞서 갑니다.
- 그래서 로컬 `main`에서도 `git pull --ff-only origin main`으로 최신 merge 결과를 가져와야 합니다.

## 7. 완료한 task 파일 이동

PR 생성, 리뷰 코멘트, merge까지 끝난 뒤에만 이동합니다.

```powershell
New-Item -ItemType Directory -Force task_done\frontend, task_done\backend, task_done\docs
git mv task\frontend\issue-1.md task_done\frontend\issue-1.md
git mv task\frontend\review-1.md task_done\frontend\review-1.md
git mv task\backend\issue-2.md task_done\backend\issue-2.md
git mv task\backend\review-2.md task_done\backend\review-2.md
git mv task\docs\issue-3.md task_done\docs\issue-3.md
git mv task\docs\review-3.md task_done\docs\review-3.md
git status
git commit -m "chore: move completed practice tasks"
git push
```

명령어 설명:

- `New-Item -ItemType Directory -Force ...`: 폴더가 없으면 만들고, 이미 있어도 오류 없이 넘어갑니다.
- `git mv 기존경로 새경로`: Git이 파일 이동으로 인식하게 옮깁니다.
- `git status`: 이동된 파일들이 staged 되었는지 확인합니다.
- `chore:`: 기능이나 문서 내용 변경이 아니라 정리 작업이라는 의미의 커밋 메시지 접두어입니다.
- `git push`: 현재 브랜치의 커밋을 GitHub에 올립니다. `-u`로 연결된 브랜치라면 `origin main`을 생략해도 됩니다.

## 8. 자주 확인할 명령어

```powershell
git status
git branch --show-current
git log --oneline -5
git remote -v
gh auth status
gh pr list --state all
```

명령어 설명:

- `git status`: 지금 변경된 파일과 staged 상태를 확인합니다.
- `git branch --show-current`: 현재 내가 어느 브랜치에 있는지 확인합니다.
- `git log --oneline -5`: 최근 커밋 5개를 짧게 확인합니다.
- `git remote -v`: GitHub 원격 주소가 제대로 연결되어 있는지 봅니다.
- `gh auth status`: GitHub CLI가 어떤 계정으로 로그인되어 있는지 확인합니다.
- `gh pr list --state all`: 열린 PR, 닫힌 PR, merge된 PR을 모두 봅니다.

## 9. 실수했을 때

아직 commit 전이면 파일 변경만 되돌립니다.

```powershell
git restore FILE_PATH
```

설명:

- `FILE_PATH`에는 되돌릴 파일 경로를 넣습니다.
- 예: `git restore frontend/app.js`
- commit 전 변경이 사라지므로 실행 전에 `git status`로 꼭 확인합니다.

방금 commit만 취소하고 수정 내용은 남깁니다.

```powershell
git reset --soft HEAD~1
```

설명:

- `HEAD~1`: 현재 커밋 바로 이전을 뜻합니다.
- `--soft`: 커밋만 취소하고 파일 수정 내용은 staged 상태로 남깁니다.

원격 branch를 지웁니다.

```powershell
git push origin --delete BRANCH_NAME
```

설명:

- GitHub에 올라간 작업 브랜치를 삭제합니다.
- 예: `git push origin --delete vibe/worker/frontend-greet-exclamation-1`

로컬 branch를 지웁니다.

```powershell
git checkout main
git branch -D BRANCH_NAME
```

설명:

- 브랜치를 삭제하기 전에는 다른 브랜치, 보통 `main`으로 이동해야 합니다.
- `-D`는 강제 삭제입니다. 아직 필요한 커밋이 있는지 `git branch --show-current`, `git log --oneline -5`로 확인한 뒤 사용합니다.
