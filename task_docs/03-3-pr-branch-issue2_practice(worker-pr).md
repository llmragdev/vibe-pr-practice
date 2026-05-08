# 03-3. PR Branch Practice: Issue 2 (Worker-PR)

이 단계에서는 **Worker**가 작업한 내용을 바탕으로 Pull Request(PR)를 생성합니다.

## 1. PR 생성 및 Issue 연결

PR 본문에 `Closes #[이슈번호]`를 포함하면, PR이 머지될 때 해당 이슈가 자동으로 닫힙니다.

```powershell
# 작업 내용에 맞춰 이슈 번호를 연결하여 PR을 생성합니다.
# (주의: 아래 예시의 #1은 예시일 뿐, 본인이 생성한 '이슈 번호'를 적어야 합니다!)
gh pr create `
  --base main `
  --head vibe/worker/backend-health-version-2 `
  --title "feat: add version to health response #1" `
  --body "Closes #1"
```

> **⚠️ 매우 중요: 이슈 번호 vs PR 번호**
> GitHub은 한 저장소 내에서 이슈와 PR의 번호를 **통합해서 1번부터 순서대로** 매깁니다. 
> *   예: 관리자가 이슈를 먼저 만들면 **#1**
> *   예: 작업자가 그다음 PR을 만들면 **#2**
> 이 경우 PR 본문에는 **`Closes #1`**이라고 적어야 합니다. 만약 본인의 PR 번호인 `#2`를 적으면 아무 일도 일어나지 않습니다!

**기대 결과:**
```text
Creating pull request for vibe/worker/backend-health-version-2 into main in llmragdev/vibe-pr-practice-study-llmragdev

https://github.com/llmragdev/vibe-pr-practice-study-llmragdev/pull/2
```

> **참고**: GitHub의 다양한 자동 종료 키워드에 대해 더 알아보려면 [Issue 자동 종료 키워드 가이드](./03-ref-github-issue-closing-keywords.md)를 확인하세요.

## 2. 생성 확인 및 검토 요청

PR이 정상적으로 생성되었는지 확인하고 Manager에게 검토를 요청합니다.

*   **웹에서 확인**: [Pull Requests 목록 페이지](https://github.com/llmragdev/vibe-pr-practice-study-llmragdev/pulls)에서 생성된 PR과 연동된 이슈 정보를 확인합니다.
*   **터미널에서 확인**: 아래 명령어로 현재 열려 있는 PR 목록을 조회할 수 있습니다.
    ```powershell
    gh pr list
    ```

**확인할 사항**:
1. PR 제목에 이슈 번호가 포함되어 있는가?
2. 본문(`body`)에 `Closes #2`가 정확히 적혀 있는가?
3. (웹 사이트 우측 하단) `Development` 섹션에 해당 이슈가 연결되어 보이는가?

---
**다음 단계**: [03-4-pr-branch-issue2_practice(manager-merge).md](./03-4-pr-branch-issue2_practice(manager-merge).md)
