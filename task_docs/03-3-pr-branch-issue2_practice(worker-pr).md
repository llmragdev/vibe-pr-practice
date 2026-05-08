# 03-3. PR Branch Practice: Issue 2 (Worker-PR)

이 단계에서는 **Worker**가 작업한 내용을 바탕으로 Pull Request(PR)를 생성합니다.

## 1. PR 생성 및 Issue 연결

PR 본문에 `Closes #[이슈번호]`를 포함하면, PR이 머지될 때 해당 이슈가 자동으로 닫힙니다.

```powershell
# 앞서 매니저가 생성한 이슈 번호가 #2라고 가정합니다.
gh pr create `
  --base main `
  --head vibe/worker/backend-health-version-2 `
  --title "feat: add version to health response #2" `
  --body "Closes #2"
```

**기대 결과 (`gh pr create` 시):**
```text
Creating pull request for vibe/worker/backend-health-version-2 into main in llmragdev/vibe-pr-practice

https://github.com/llmragdev/vibe-pr-practice/pull/3
```

> **참고**: GitHub의 다양한 자동 종료 키워드에 대해 더 알아보려면 [Issue 자동 종료 키워드 가이드](./03-ref-github-issue-closing-keywords.md)를 확인하세요.

## 2. 생성 확인

생성된 PR의 URL을 확인하고 Manager에게 검토를 요청합니다.

---
**다음 단계**: [03-4-pr-branch-issue2_practice(manager-merge).md](./03-4-pr-branch-issue2_practice(manager-merge).md)
