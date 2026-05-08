# 03-0. PR Branch Practice: Issue 2 (Pre-work)

이 단계는 실습을 시작하기 전, 원본 저장소를 본인의 GitHub 계정으로 가져와 실습 환경을 구축하는 과정입니다.

## 1. 저장소 클론 (Clone)

원본 저장소를 본인의 로컬 PC로 가져옵니다. 이때 폴더명을 브랜치 실습용임을 알 수 있게 지정합니다.

```powershell
# 상위 작업 폴더로 이동
cd E:\proj_boot

# 원본 저장소 클론 (폴더명 지정)
gh repo clone llmragdev/vibe-pr-practice vibe-pr-practice-branch-llmragdev

# 해당 폴더로 이동
cd vibe-pr-practice-branch-llmragdev
```

## 2. 본인의 GitHub에 새 저장소 생성 및 Push

이 실습은 본인이 관리자(Manager)와 작업자(Worker) 역할을 모두 수행하므로, 본인 계정의 GitHub에 새로운 저장소를 만들어 관리합니다.

```powershell
# 1. 기존 원격(origin) 연결 제거
git remote remove origin

# 2. 본인 계정에 새 public 저장소 생성 및 로컬 소스 Push
# (저장소 이름은 폴더명과 동일하게 생성됩니다)
gh repo create vibe-pr-practice-branch-llmragdev --public --source=. --remote=origin --push

# [필요시] 만약 "X Unable to add remote 'origin'" 오류가 발생한다면 수동으로 연결합니다.
# (이미 GitHub에 저장소가 생성된 경우 발생할 수 있습니다)
git remote add origin https://github.com/llmragdev/vibe-pr-practice-branch-llmragdev.git
git remote -v
```

## 3. 환경 확인

이제 본인의 GitHub 저장소(`https://github.com/[본인계정]/vibe-pr-practice-branch-llmragdev`)가 생성되었습니다. 

다음 단계부터는 이 저장소를 대상으로 **Manager**와 **Worker** 역할을 오가며 실습을 진행합니다.

---
**다음 단계**: [03-1-pr-branch-issue2_practice(manager).md](./03-1-pr-branch-issue2_practice(manager).md)
