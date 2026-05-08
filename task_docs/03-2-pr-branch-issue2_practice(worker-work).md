# 03-2. PR Branch Practice: Issue 2 (Worker-Work)

- **실습 저장소**: [vibe-pr-practice-study-llmragdev](https://github.com/llmragdev/vibe-pr-practice-study-llmragdev)

이 단계에서는 **Worker**가 할당된 이슈를 확인하고, 브랜치를 생성하여 코드를 수정합니다.

### ⚠️ 권한 확인 (중요: Fork vs Branch 차이)

이전 실습(Fork)과 달리, 이번 실습은 **동일한 저장소 내에서 협업**하는 방식입니다. 따라서 반드시 관리자의 권한 승인이 필요합니다.

| 구분 | Fork 방식 (이전 실습) | Branch 방식 (현재 실습) |
| :--- | :--- | :--- |
| **브랜치 푸시 위치** | **내 계정**의 복제된 저장소 | **관리자 계정**의 원본 저장소 |
| **관리자 승인** | 필요 없음 (누구나 Fork 가능) | **필요 (Collaborator 등록 필수)** |
| **사용 환경** | 불특정 다수가 참여하는 오픈소스 | **팀 단위의 기업 내부 프로젝트** |

> **확인 사항**: Manager가 본인의 계정을 **Collaborator**로 등록했는지 확인하세요. GitHub 알림이나 메일에서 **[Accept Invitation]**을 클릭해야 권한이 활성화됩니다.

> **💡 권한 확인 명령어**: 초대를 수락한 후, 터미널에서 아래 명령어를 입력하여 본인의 권한을 확인해 보세요.
> ```powershell
> gh repo view --json viewerPermission
> ```
> * 결과에 `"viewerPermission": "WRITE"` (또는 ADMIN)라고 나오면 푸시가 가능한 상태입니다!

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

# 2. 현재 로그인된 계정 상태 및 권한 확인
gh auth status
gh repo view --json viewerPermission
```

**기대 결과:**
```text
# 1. 로그인 계정 확인 (gh auth status)
github.com
  ✓ Logged in to github.com account hanschoi20 (keyring)
  ...

# 2. 저장소 권한 확인 (gh repo view --json viewerPermission)
{
  "viewerPermission": "WRITE"
}
```

> **Tip**: 만약 권한이 `READ`로 나온다면, 아직 관리자의 초대를 수락하지 않은 것입니다. GitHub 웹사이트 저장소 페이지 상단의 배너를 확인하여 **[Accept Invitation]**을 완료하세요.

---

### 💡 실무 지식: 브랜치 보호 규칙 (Branch Protection Rules)

`WRITE` 권한이 있는 작업자는 이론적으로 `main` 브랜치에 직접 푸시할 수 있습니다. 하지만 실무에서는 사고를 방지하기 위해 **Branch Protection Rule**을 설정합니다.

*   **설정 목적**: 관리자의 승인(PR) 없이는 `main` 브랜치를 직접 수정할 수 없도록 강제합니다.
*   **설정 방법 (참고용 - 이번 실습에서는 생략)**:
    1. GitHub 저장소의 **Settings > Branches** 메뉴로 이동
    2. **[Add branch protection rule]** 클릭
    3. `Branch name pattern`에 **`main`** 입력
    4. **"Require a pull request before merging"** 체크 후 저장
*   **효과**: 설정이 완료되면 `WRITE` 권한이 있더라도 `main`에 직접 `push` 할 수 없게 됩니다. 반드시 브랜치를 생성하여 PR을 올리고 승인을 받아야만 코드를 반영할 수 있습니다.

> **※ 주의**: 이번 실습에서는 원활한 진행을 위해 이 설정 과정을 생략하고 진행합니다.

---

```powershell
# 3. Git 작성자 정보 설정 (실습자의 실제 ID와 이메일 입력)
git config user.name "hanschoi20"
git config user.email "hanschoi20@example.com"

# 설정 확인
git config user.name
git config user.email
```

## 2. 할당된 이슈 확인 및 댓글 작성

작업을 시작하기 전, 할당된 이슈의 목록과 상세 내용을 확인합니다. 

*   **웹에서 확인**: [이슈 목록 페이지](https://github.com/llmragdev/vibe-pr-practice-study-llmragdev/issues)에서 나에게 할당된 이슈를 확인합니다.
*   **터미널에서 확인**: 아래 명령어를 사용하여 전체 이슈 목록을 조회한 뒤, 상세 내용을 확인하고 댓글을 남깁니다.

```powershell
# 1. 이슈 목록 확인
gh issue list

# 2. 이슈 상세 내용 확인 (번호 #1)
gh issue view 1

# 3. 이슈에 댓글 남기기 (작업 수락)
gh issue comment 1 --body "제가 이 작업을 담당하겠습니다. 브랜치 생성하여 작업 후 PR 올리겠습니다!"
```

**기대 결과:**
```text
https://github.com/llmragdev/vibe-pr-practice-study-llmragdev/issues/1#issuecomment-xxxxxxxx
```

## 3. main 최신화 및 브랜치 생성

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

**기대 결과:**
```text
Switched to a new branch 'vibe/worker/backend-health-version-2'
```

> **💡 실무 팁 (Checkout vs Switch)**: `git checkout`은 브랜치 전환과 파일 복구 기능을 모두 가진 포괄적인 명령어입니다. 그래서 최신 Git(2.23 이상)에서는 브랜치 전환 전용인 **`git switch`** 명령어를 새로 도입하여 사용을 권장하고 있습니다. (예: `git switch -c [브랜치명]`) 하지만 여전히 많은 현장에서 `checkout`을 혼용하여 사용합니다.

## 4. 코드 수정 (backend/api.js)

`backend/api.js`를 열어 `health()` 함수가 버전을 포함하도록 수정합니다.

> **Tip**: 코드를 어떻게 수정해야 할지 헷갈린다면 샘플 파일인 [api.js(revised)](../src_samples/revised/api.js) 내용을 참고하세요.

```js
function health() {
  return { status: "ok", version: "1.0.0" };
}

module.exports = { health };
```

## 5. 커밋 및 푸시 (Commit & Push)

수정사항을 로컬에 저장하고 원격 저장소(GitHub)에 올립니다. Git은 안전한 관리를 위해 아래와 같은 **3단계 과정**을 거칩니다.

1.  **`git add` (장바구니 담기)**: 수정된 파일 중 서버에 올릴 파일을 선택합니다.
2.  **`git commit` (포장 및 라벨링)**: 선택한 파일들을 하나의 꾸러미로 묶고, 작업 내용에 대한 설명(`-m "메시지"`)을 붙입니다.
3.  **`git push` (배송)**: 내 컴퓨터에 저장된 꾸러미를 GitHub 서버로 전송합니다.

```powershell
# 1. 수정된 파일을 스테이징 영역(장바구니)에 추가
git add backend/api.js

# 2. 로컬 저장소에 확정 기록 (포장)
git commit -m "feat: add version to health response"

# 3. GitHub 원격 저장소에 업로드 (배송)
# (-u 옵션은 앞으로 이 브랜치를 origin의 동일 브랜치와 연결하겠다는 뜻입니다)
git push -u origin vibe/worker/backend-health-version-2
```

**기대 결과 (`git push` 시):**
```text
[vibe/worker/backend-health-version-2 319c3a7] feat: add version to health response
 1 file changed, 1 insertion(+), 1 deletion(-)
 ...
 To https://github.com/llmragdev/vibe-pr-practice-study-llmragdev.git
 * [new branch]      vibe/worker/backend-health-version-2 -> vibe/worker/backend-health-version-2
```

# [최종 확인] GitHub 웹에서 코드 확인하기
브라우저에서 저장소에 접속하여 코드가 잘 올라갔는지 확인합니다.

1.  GitHub 저장소 페이지로 이동합니다.
2.  브랜치 선택 버튼(`main`)을 눌러 **`vibe/worker/backend-health-version-2`** 브랜치로 전환합니다.
3.  `backend/api.js` 파일을 클릭하여 버전 정보(`version: "1.0.0"`)가 포함되어 있는지 확인합니다.

---
**다음 단계**: [03-3-pr-branch-issue2_practice(worker-pr).md](./03-3-pr-branch-issue2_practice(worker-pr).md)
