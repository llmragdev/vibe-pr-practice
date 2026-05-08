# 03-1. PR Branch Practice: Issue 2 (Manager-Issue)

이 단계에서는 **Manager**가 GitHub Issues 기능을 사용하여 새로운 작업을 할당합니다.

## 1. Manager 계정 확인 및 설정

> **💡 실습 팁**: 가이드를 보기 위한 창과 별개로, **새로운 VS Code 창(Ctrl+Shift+N)**을 열어 `vibe-pr-practice-study-llmragdev` 폴더를 독립적으로 여는 것을 권장합니다.

관리자(Manager) 역할로 실습하기 위해 아래 명령어를 실행합니다.

```powershell
# 1. GitHub CLI 로그인 상태 확인
gh auth status
```

기대 값: 
  github.com
    ✓ Logged in to github.com account **llmragdev** (keyring)
    ...

> **Tip**: 계정이 다르다면 `gh auth logout` 후 `gh auth login`으로 다시 로그인하세요.

### 📌 Git 작성자 설정 (권장)

```powershell
# 현재 폴더(--local)에만 적용되는 설정입니다.
git config user.name "llmragdev"
git config user.email "llmragdev@gmail.com"

# 설정 확인
git config user.name
git config user.email
```

#### ❓ 왜 GitHub 계정과 설정을 일치시켜야 하나요?
설정을 일치시키지 않아도 실습은 가능하지만, 다음과 같은 문제가 발생합니다.

*   **user.name을 다르게 쓰면**: `git log`를 볼 때 실제 GitHub ID와 이름이 달라 누가 누구인지 헷갈립니다.
*   **user.email을 다르게 쓰면**: GitHub 웹사이트에서 커밋 목록을 볼 때 내 프로필 사진 대신 **회색 기본 아이콘**이 뜨고, 열심히 심은 **"내 잔디(Contribution)"**가 기록되지 않습니다.


## 2. 원격 저장소 연결 확인 (Remote Check)

이슈를 생성하기 전, 로컬 폴더가 GitHub 저장소와 올바르게 연결되어 있는지 확인합니다.

```powershell
git remote -v
```

**기대 결과:**
```text
origin  https://github.com/llmragdev/vibe-pr-practice-study-llmragdev.git (fetch)
origin  https://github.com/llmragdev/vibe-pr-practice-study-llmragdev.git (push)
```

> **오류 발생 시**: 만약 결과가 아무것도 나오지 않는다면, [03-0 가이드](./03-0-pr-branch-issue2_practice(pre-work).md)를 참고하여 `git remote add origin ...` 명령어로 수동 연결하세요.


## 3. GitHub Issue 생성

#### 🎯 이슈 생성의 목적
실무에서 이슈는 버그 제보, 기능 제안, 질문 등 다양한 용도로 사용됩니다. 
**이번 실습 시나리오**에서는 관리자가 작업자에게 업무를 할당하는 **'작업 지시서'**의 관점에서 접근합니다.

*   **작업 지시**: 관리자가 작업자에게 어떤 기능을 수정해야 할지 공식적으로 전달합니다.
*   **히스토리 추적**: 코드의 변경이 어떤 요구사항 때문에 발생했는지 기록을 남깁니다.
*   **자동 연동**: 나중에 작업자가 PR을 보낼 때 이 이슈 번호를 참조하면, 머지 시 이슈가 자동으로 닫힙니다.

GitHub Issues 기능을 사용하여 `backend/api.js`의 수정을 요청하는 티켓을 생성합니다. 
명령어 실행 후 출력되는 **이슈 URL**을 통해 웹 브라우저에서 생성된 내용을 직접 확인할 수 있습니다.

```powershell
gh issue create `
  --title "backend: add version to health response" `
  --body 'Update backend/api.js so health() returns { status: "ok", version: "1.0.0" }.'
```

**기대 결과 (성공 시):**
```text
Creating issue in llmragdev/vibe-pr-practice

https://github.com/llmragdev/vibe-pr-practice-study-llmragdev/issues/1
```

> **주의**: PowerShell에서 `--body` 내부에 큰따옴표(`"`)를 직접 사용하면 인식 오류가 발생할 수 있습니다. 가이드처럼 바깥쪽을 작은따옴표(`'`)로 감싸주세요. (오류 발생 시: `unknown arguments ["version:" ... ]` 출력됨)

## 4. Issue 번호 확인

웹 브라우저에서도 확인 가능하지만, GitHub CLI 명령어를 사용하면 터미널에서 즉시 이슈 목록과 번호를 확인할 수 있습니다. 이슈가 정상적으로 생성되었는지 목록을 확인합니다.

```powershell
gh issue list --state open
```

**기대 결과:**
```text
Showing 1 of 1 open issue in llmragdev/vibe-pr-practice-study-llmragdevv

ID  TITLE                                    LABELS  UPDATED               
#1  backend: add version to health response          less than a minute ago
```

---
**다음 단계**: [03-2-pr-branch-issue2_practice(worker-work).md](./03-2-pr-branch-issue2_practice(worker-work).md)
