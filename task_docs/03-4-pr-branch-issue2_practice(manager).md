# 03-4. PR Branch Practice: Issue 2 (Manager - Merge)

이 단계에서는 **Manager**가 Worker의 PR을 검토하고 최종 머지(Merge)를 수행합니다.

## 1. Manager 계정 전환

```powershell
# GitHub CLI 계정 전환 (llmragdev)
gh auth login

# Git 작성자 정보 설정
git config user.name "llmragdev"
git config user.email "llmragdev@gmail.com"
```

## 2. PR 검토 (Review)

열린 PR 목록을 확인하고 변경 내용을 검토합니다.

```powershell
# PR 목록 확인
gh pr list --state open

**기대 결과 (`gh pr list` 시):**
```text
Showing 1 of 1 open pull request in llmragdev/vibe-pr-practice

ID  TITLE                             BRANCH                            CREATED AT
#3  feat: add version to health r...  vibe/worker/backend-health-ve...  about 43 minutes ago
```

# PR 상세 내용 및 Diff 확인 (PR_NUMBER를 실제 번호로 교체)
gh pr view [PR_NUMBER] --comments
```

**기대 결과 (`gh pr view` 시):**
```text
feat: add version to health response #2 llmragdev/vibe-pr-practice#3
Open • hanschoi20 wants to merge 1 commit into main from vibe/worker/backend-health-version-2
...
  Closes #2
```

```powershell
gh pr diff [PR_NUMBER]
```

**기대 결과 (`gh pr diff` 시):**
```diff
--- a/backend/api.js
+++ b/backend/api.js
@@ -1,5 +1,5 @@
 function health() {
-  return { status: "ok" };
+  return { status: "ok", version: "1.0.0" };
 }
```

문제가 없다면 승인 댓글을 남깁니다.

```powershell
gh pr comment [PR_NUMBER] --body "Reviewed by manager: Looks good to merge."
```

## 3. 머지 (Merge)

`squash` 머지를 사용하여 브랜치의 이력을 하나로 합치며 머지합니다.

```powershell
gh pr merge [PR_NUMBER] --squash --delete-branch
```

**기대 결과 (`gh pr merge` 시):**
```text
✓ Squashed and merged pull request llmragdev/vibe-pr-practice#3 (feat: add version to health response #2)
✓ Deleted remote branch vibe/worker/backend-health-version-2
```

## 4. 로컬 main 최신화

머지된 최신 코드를 로컬 `main` 브랜치로 가져옵니다.

```powershell
git checkout main
git pull origin main
```

**기대 결과 (`git pull` 시):**
```text
Updating ce5c1a9..1c896dc
Fast-forward
 backend/api.js | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

## 5. 이슈 자동 종료 확인

이슈가 키워드에 의해 자동으로 닫혔는지 확인합니다.

```powershell
gh issue view 2
```

**기대 결과 (`gh issue view` 시):**
```text
backend: add version to health response llmragdev/vibe-pr-practice#2
Closed • llmragdev opened about 7 hours ago • 0 comments
```

---
**다음 단계**: [03-5-pr-branch-issue2_practice(manager).md](./03-5-pr-branch-issue2_practice(manager).md)
