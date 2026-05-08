# 03-1. PR Branch Practice: Issue 2 (Manager)

이 단계에서는 **Manager**가 GitHub Issues 기능을 사용하여 새로운 작업을 할당합니다.

## 1. Manager 계정 확인 및 설정

관리자 폴더에서 실행합니다.

```powershell
gh auth status
git config user.name
git config user.email
```

기대 값:
*   GitHub CLI account = `llmragdev`
*   user.name = `llmragdev`
*   user.email = `llmragdev@gmail.com`

> **Tip**: 계정이 다르다면 `gh auth logout` 후 `gh auth login`으로 다시 로그인하세요.

## 2. GitHub Issue 생성

GitHub Issues 기능을 사용하여 `backend/api.js`의 수정을 요청하는 티켓을 생성합니다.

```powershell
gh issue create `
  --title "backend: add version to health response" `
  --body 'Update backend/api.js so health() returns { status: "ok", version: "1.0.0" }.'
```

**기대 결과 (성공 시):**
```text
Creating issue in llmragdev/vibe-pr-practice

https://github.com/llmragdev/vibe-pr-practice/issues/2
```

> **주의**: PowerShell에서 `--body` 내부에 큰따옴표(`"`)를 직접 사용하면 인식 오류가 발생할 수 있습니다. 가이드처럼 바깥쪽을 작은따옴표(`'`)로 감싸주세요. (오류 발생 시: `unknown arguments ["version:" ... ]` 출력됨)

## 3. Issue 번호 확인

이슈가 정상적으로 생성되었는지 목록을 확인합니다.

```powershell
gh issue list --state open
```

**기대 결과:**
```text
Showing 1 of 1 open issue in llmragdev/vibe-pr-practice

ID  TITLE                                    LABELS  UPDATED               
#2  backend: add version to health response          less than a minute ago
```

---
**다음 단계**: [03-2-pr-branch-issue2_practice(worker).md](./03-2-pr-branch-issue2_practice(worker).md)
