# 03-0. PR Branch Practice: Issue 2 (Pre-work)

이 단계는 실습을 시작하기 전, 원본 저장소를 본인의 GitHub 계정으로 가져와 실습 환경을 구축하는 과정입니다.

## 📋 사전 준비물

이 실습은 혼자서 **관리자(Manager)**와 **작업자(Worker)** 역할을 번갈아 가며 수행하는 1인 2역 실습입니다. 시작 전 다음을 확인하세요.

1.  **두 개의 GitHub 계정**: 서로 다른 이메일로 가입된 2개의 계정이 필요합니다.
    *   **Manager 계정**: 프로젝트 관리 및 코드 리뷰 담당
    *   **Worker 계정**: 실제 코드 수정 및 PR 생성 담당
2.  **GitHub CLI 설치**: `gh` 명령어를 사용할 수 있어야 합니다.

> **📖 본 가이드의 예시 계정**
> 실습의 이해를 돕기 위해 아래 계정명을 예시로 사용합니다. 실제 실습 시에는 본인의 계정 ID를 사용하세요.
> *   **관리자(Manager) 예시**: `llmragdev`
> *   **작업자(Worker) 예시**: `hanschoi20`

---

## 1. 저장소 클론 (Clone)

원본 저장소(`llmragdev/vibe-pr-practice`)를 본인의 로컬 PC로 복제(Clone)합니다. 

> **💡 폴더 생성 원리**: 명령어 끝에 본인이 작업할 저장소 이름을 지정하면, **내 로컬 컴퓨터에 해당 이름으로 폴더가 생기며** 그 안에 소스 코드가 다운로드됩니다. 이 폴더 이름이 나중에 본인의 GitHub에 생성할 저장소 이름이 됩니다.

> **📌 폴더명 지정 규칙**: `vibe-pr-practice-study-[본인의 GitHub ID]`
> (예: `vibe-pr-practice-study-llmragdev`)

```powershell
# 상위 작업 폴더로 이동
cd E:\proj_boot

# 원본 저장소 클론 (폴더명 지정)
# (vibe-pr-practice-study-[본인ID] 형식으로 지정하세요)
gh repo clone llmragdev/vibe-pr-practice vibe-pr-practice-study-llmragdev

# 해당 폴더로 이동
cd vibe-pr-practice-study-llmragdev
```

## 2. 본인의 GitHub에 새 저장소 생성 및 Push

이 실습은 본인이 관리자(Manager)와 작업자(Worker) 역할을 모두 수행하므로, 본인 계정의 GitHub에 새로운 저장소를 만들어 소스를 올립니다.

> **📌 저장소 이름 규칙**: `vibe-pr-practice-study-[본인의 GitHub ID]`
> (예: ID가 `llmragdev`라면 `vibe-pr-practice-study-llmragdev`)

```powershell
# 1. 원본 저장소(llmragdev/vibe-pr-practice)와의 연결 제거
# (새로운 나의 저장소와 연결하기 위해 기존 원격 설정을 지웁니다)
git remote remove origin

# 2. 본인 계정에 새 public 저장소 생성
# (vibe-pr-practice-study-[본인ID] 형식으로 이름을 지정하세요)
gh repo create vibe-pr-practice-study-llmragdev --public

# [중간 확인 1] 브라우저에서 접속해 보세요. 
# 주소: https://github.com/[본인계정ID]/vibe-pr-practice-study-llmragdev
# (저장소는 생겼지만 'Quick setup' 화면이 나오며 코드가 없는 상태여야 합니다)

# 3. 새로 만든 원격 저장소를 로컬과 연결
# (URL의 'llmragdev' 부분은 본인의 GitHub ID로 확인하세요)
git remote add origin https://github.com/llmragdev/vibe-pr-practice-study-llmragdev.git

# 4. 로컬 소스를 main 브랜치로 푸시 (매우 중요!)
# 이 단계가 완료되어야 GitHub 저장소에 코드가 채워집니다.
git push -u origin main

# [최종 확인 2] 브라우저에서 다시 새로고침(F5) 해보세요.
# (이제 'backend', 'frontend' 폴더 등 소스 코드가 보인다면 성공입니다!)

# 5. 연결 상태 확인 (터미널)
git remote -v
```

## 3. 환경 확인

이제 본인의 GitHub 저장소에 초기 소스 코드가 정상적으로 업로드되었습니다. 

다음 단계부터는 이 저장소를 대상으로 **Manager**와 **Worker** 역할을 오가며 실습을 진행합니다.

---
**다음 단계**: [03-1-pr-branch-issue2_practice(manager-issue).md](./03-1-pr-branch-issue2_practice(manager-issue).md)

## 💡 실습 환경 초기화가 필요한 경우
설정이 꼬여서 처음부터 다시 시작하고 싶다면 [97-pr-branch-cleanup.md](./97-pr-branch-cleanup.md) 가이드를 참고하여 환경을 초기화하세요.
