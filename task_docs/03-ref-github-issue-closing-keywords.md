# 02-2. GitHub Issue Closing Keywords

이 문서는 `02-pr-branch-issue2_practice.md`의 보충 설명입니다.

PR body에 `Closes #2`를 넣으면 왜 PR merge 시 GitHub Issue #2가 자동으로 닫히는지 설명합니다.

## 핵심 규칙

GitHub는 PR 설명이나 commit message에서 다음 패턴을 인식합니다.

```text
KEYWORD #ISSUE_NUMBER
```

예:

```text
Closes #2
Fixes #2
Resolves #2
```

이렇게 작성하면 GitHub는 해당 PR이 Issue를 해결하는 작업이라고 판단합니다.

PR이 저장소의 기본 브랜치, 보통 `main`, 로 merge되면 연결된 Issue가 자동으로 닫힙니다.

## 자동 close keyword

GitHub가 자동 close로 인식하는 keyword는 다음과 같습니다.

```text
close
closes
closed
fix
fixes
fixed
resolve
resolves
resolved
```

대소문자는 중요하지 않습니다.

아래 표현은 모두 동작합니다.

```text
Closes #2
CLOSES #2
Closes: #2
fixes #2
Resolves #2
```

## 같은 저장소의 Issue를 닫는 경우

PR과 Issue가 같은 저장소에 있으면 Issue 번호만 쓰면 됩니다.

```text
Closes #2
```

이 뜻은 다음과 같습니다.

```text
이 PR이 현재 저장소의 Issue #2를 해결합니다.
PR이 main에 merge되면 Issue #2를 닫습니다.
```

## 다른 저장소의 Issue를 닫는 경우

다른 저장소의 Issue를 닫으려면 저장소 이름까지 씁니다.

```text
Fixes owner/repository#2
```

예:

```text
Fixes llmragdev/vibe-pr-practice#2
```

## 여러 Issue를 닫는 경우

여러 Issue를 닫으려면 각 Issue마다 keyword를 붙입니다.

```text
Closes #2, fixes #3, resolves #4
```

아래처럼 첫 번째에만 keyword를 붙이면 모든 Issue가 닫힌다고 기대하면 안 됩니다.

```text
Closes #2, #3, #4
```

실습에서는 명확하게 아래 형식을 사용합니다.

```text
Closes #2, closes #3, closes #4
```

## References와의 차이

`References #2`는 Issue를 언급하거나 연결하는 용도입니다.

```text
References #2
```

이 표현은 GitHub Issue #2에 관련 PR이 있다는 힌트는 줄 수 있지만, PR merge 시 Issue를 자동으로 닫지는 않습니다.

자동으로 닫고 싶으면 closing keyword를 사용해야 합니다.

```text
Closes #2
```

## 자동 close 조건

자동 close가 되려면 다음 조건을 만족해야 합니다.

1. `#2`가 실제 GitHub Issue 번호여야 합니다.
2. PR의 base branch가 저장소의 기본 브랜치여야 합니다.
3. PR body나 commit message에 closing keyword가 있어야 합니다.
4. PR이 merge되어야 합니다.

예를 들어 로컬 파일 `task/backend/issue-2.md`만 있고 GitHub Issues에 Issue #2가 없다면 `Closes #2`는 닫을 Issue를 찾지 못합니다.

## 실습에서 사용하는 PR 생성 예시

```powershell
gh pr create `
  --base main `
  --head vibe/worker/backend-health-version-2 `
  --title "feat: add version to health response #2" `
  --body "Closes #2"
```

이 명령에서 중요한 부분은 다음 두 줄입니다.

```powershell
--base main
--body "Closes #2"
```

`--base main`은 PR이 기본 브랜치로 들어간다는 뜻입니다.

`--body "Closes #2"`는 이 PR이 GitHub Issue #2를 해결한다는 뜻입니다.

## 확인 방법

PR 생성 후 GitHub 웹 화면에서 PR 오른쪽 영역이나 Issue 화면의 Development 영역을 보면 연결된 PR을 확인할 수 있습니다.

CLI에서는 PR과 Issue 상태를 확인합니다.

```powershell
gh pr view PR_NUMBER
gh issue view 2
```

merge 후에는 아래처럼 확인합니다.

```powershell
gh pr list --state all --limit 5
gh issue list --state all --limit 5
```

기대 상태:

```text
PR    MERGED
Issue CLOSED
```

## 공식 문서

GitHub 공식 문서:

```text
https://docs.github.com/articles/closing-issues-via-commit-messages
```

