# 03-2. PR Branch Practice: Issue 2 (Worker-Work)

이 단계에서는 **Worker**가 할당된 이슈를 확인하고, 브랜치를 생성하여 코드를 수정합니다.

> **💡 역할 전환 팁**: Manager에서 Worker로 역할을 바꿀 때는 **기존 VS Code 창을 닫고 새로 실행**하여 폴더를 다시 여는 것을 추천합니다. 터미널 기록이나 열려 있는 파일들로 인해 역할이 헷갈리는 것을 방지할 수 있습니다.

## 1. Worker 계정 전환 및 폴더 이동

> **💡 역할 전환 팁**: Manager에서 Worker로 역할을 바꿀 때는 기존 VS Code 창을 닫고 **새 창에서 폴더를 다시 여는 것**을 추천합니다. 터미널 기록이나 열려 있는 파일들로 인해 역할이 헷갈리는 것을 방지할 수 있습니다.

```powershell
# Worker 작업 폴더로 이동 (이미 클론된 상태여야 함)
cd E:\proj_boot\vibe-pr-practice-study-llmragdev

# 1. 기존 계정 로그아웃 후 Worker 계정으로 로그인
gh auth logout
gh auth login

# (참고: 로그인 과정에서 브라우저 인증을 완료하고 hanschoi20 계정이 나타나야 합니다)

# 2. 현재 로그인된 계정 상태 확인
gh auth status
```

**기대 결과 (`gh auth status` 시):**
```text
github.com
  ✓ Logged in to github.com account hanschoi20 (keyring)
  - Active account: true
  ...
```

```powershell
# 3. Git 작성자 정보 설정 (실습자의 실제 ID와 이메일 입력)
git config user.name "hanschoi20"
git config user.email "hanschoi20@example.com"

# 설정 확인
git config user.name
git config user.email
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
# (Tip: 브랜치를 만든다고 해서 새로운 폴더가 생기지는 않습니다. 
# 01-1 실습(Fork) 때와 달리, 현재 폴더 내에서 '작업 세계관'만 분리하는 것입니다.)
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
**다음 단계**: [03-3-pr-branch-issue2_practice(worker-pr).md](./03-3-pr-branch-issue2_practice(worker-pr).md)
