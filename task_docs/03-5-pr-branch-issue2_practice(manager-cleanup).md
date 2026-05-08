# 03-5. PR Branch Practice: Issue 2 (Manager-Cleanup)

마지막 단계로 완료된 작업 파일을 정리하고 최종 상태를 확인합니다.

## 1. 완료 파일 이동 (Cleanup)

# 1. 완료된 Task 파일 이동

실습 가이드로 사용된 `task` 폴더의 파일들을 `task_done` 폴더로 옮겨 완료 처리를 합니다.

> **💡 파일이 없는 경우**: 만약 `task` 폴더가 비어있거나 파일이 없다면, 원본 폴더(`main\task`)에서 해당 파일들을 복사해 오세요.

```powershell
# 1. 작업 파일들을 Git 추적 대상으로 추가 (이미 있다면 생략 가능)
git add task

# 2. 완료 폴더 생성 (없을 경우)
mkdir task_done\backend

# 3. 파일 이동
# (※ 실제 파일이 위치한 task/docs 폴더에서 task_done/backend로 옮깁니다)
git mv task\docs\issue-2.md task_done\backend\issue-2.md
git mv task\docs\review-2.md task_done\backend\review-2.md

# 4. 변경사항 커밋 및 푸시
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
    디렉터리: E:\proj_boot\vibe-pr-practice-study-llmragdev\task_done\backend

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----      2026-05-08  오후 7:17           1461 issue-2.md
-a----      2026-05-08  오후 7:17           1566 review-2.md
```

---
**축하합니다!** Issue 2 실습의 모든 과정을 마쳤습니다.
