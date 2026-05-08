# 03-2. PR Branch Practice: Issue 2 (Worker - Work)

이 단계에서는 **Worker**가 할당된 이슈를 확인하고, 브랜치를 생성하여 코드를 수정합니다.

## 1. Worker 계정 전환 및 폴더 이동

```powershell
# Worker 작업 폴더로 이동 (이미 클론된 상태여야 함)
cd E:\proj_boot\vibe-pr-practice-branch-llmragdev

# GitHub CLI 계정 전환
gh auth login

# (namkyuglobal2 계정으로 로그인 확인)
gh auth status
```

**기대 결과 (`gh auth status` 시):**
```text
github.com
  ✓ Logged in to github.com account namkyuglobal2 (keyring)
  - Active account: true
  - Git operations protocol: https
  ...
```

# Git 작성자 정보 설정
git config user.name "namkyuglobal2"
git config user.email "namkyuglobal2@gmail.com"
```

## 2. main 최신화 및 브랜치 생성

작업을 시작하기 전 `main`을 최신화하고 새 브랜치를 만듭니다.

```powershell
git checkout main
git pull origin main
```

**기대 결과 (`git pull` 시):**
```text
Updating ce5c1a9..1c896dc
Fast-forward
 frontend/app.js                          | 2 +-
 {task => task_done}/frontend/issue-1.md  | 0
 3 files changed, 1 insertion(+), 1 deletion(-)
```

# 작업 브랜치 생성
git checkout -b vibe/worker/backend-health-version-2
```

## 3. 코드 수정 (backend/api.js)

`backend/api.js`를 열어 `health()` 함수가 버전을 포함하도록 수정합니다.

```js
function health() {
  return { status: "ok", version: "1.0.0" };
}

module.exports = { health };
```

## 4. 커밋 및 푸시 (Commit & Push)

수정사항을 저장하고 원격 저장소에 올립니다.

```powershell
git add backend/api.js
git commit -m "feat: add version to health response"
git push -u origin vibe/worker/backend-health-version-2
```

**기대 결과 (`git push` 시):**
```text
[vibe/worker/backend-health-version-2 319c3a7] feat: add version to health response
 1 file changed, 1 insertion(+), 1 deletion(-)
Enumerating objects: 7, done.
...
To https://github.com/llmragdev/vibe-pr-practice.git
 * [new branch]      vibe/worker/backend-health-version-2 -> vibe/worker/backend-health-version-2
```

---
**다음 단계**: [03-3-pr-branch-issue2_practice(worker).md](./03-3-pr-branch-issue2_practice(worker).md)
