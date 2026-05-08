# 03-5. PR Branch Practice: Issue 2 (Manager - Cleanup)

마지막 단계로 완료된 작업 파일을 정리하고 최종 상태를 확인합니다.

## 1. 완료 파일 이동 (Cleanup)

실습 가이드로 사용된 `task` 폴더의 파일들을 `task_done` 폴더로 옮겨 완료 처리를 합니다.

```powershell
# 완료 폴더 생성 (없을 경우)
mkdir task_done\backend

# 파일 이동
git mv task\backend\issue-2.md task_done\backend\issue-2.md
git mv task\backend\review-2.md task_done\backend\review-2.md

# 변경사항 커밋 및 푸시
git commit -m "chore: move completed backend task"
git push origin main
```

**기대 결과 (`git commit` 시):**
```text
[main d68509c] chore: move completed backend task
 2 files changed, 0 insertions(+), 0 deletions(-)
 rename {task => task_done}/backend/issue-2.md (100%)
 rename {task => task_done}/backend/review-2.md (100%)
```

## 2. 최종 상태 확인

모든 작업이 정상적으로 종료되었는지 확인합니다.

```powershell
# PR 상태 확인 (MERGED 상태여야 함)
gh pr list --state all --limit 5

# 이슈 상태 확인 (CLOSED 상태여야 함 - 키워드로 인해 자동 종료)
gh issue list --state all --limit 5

# 로컬 파일 확인
ls task_done\backend
```

**기대 결과 (`ls` 시):**
```text
    디렉터리: E:\proj_boot\vibe-pr-practice-branch-llmragdev\task_done\backend

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----      2026-05-08  오후 7:17           1461 issue-2.md
-a----      2026-05-08  오후 7:17           1566 review-2.md
```

---
**축하합니다!** Issue 2 실습의 모든 과정을 마쳤습니다.
