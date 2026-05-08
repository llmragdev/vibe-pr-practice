# 97. 실습 환경 초기화 가이드 (Clean Start)

실습 도중 설정이 꼬였거나, 처음부터 다시 깨끗하게 시작하고 싶을 때 이 가이드를 따르세요. 로컬 폴더와 GitHub 원격 저장소를 모두 삭제합니다.

## ⚠️ 주의사항
이 명령어를 실행하면 해당 폴더 내의 **모든 작업 내용이 삭제**됩니다. 중요한 코드가 있다면 미리 백업하세요.

## 🛠️ 초기화 절차 (PowerShell)

```powershell
# 1. 상위 작업 폴더로 이동
cd E:\proj_boot
```

```powershell
# 2. 로컬 연습 폴더 삭제
Remove-Item -Recurse -Force vibe-pr-practice-study-llmragdev
```
*   **기대 결과**: 아무런 메시지가 출력되지 않으면 성공입니다.
*   **오류 시**: "경로를 찾을 수 없습니다"라는 오류가 나면 이미 폴더가 삭제된 것이므로 무시하고 다음 단계로 가세요.

```powershell
# 3. GitHub 원격 저장소 삭제를 위한 권한 확인
gh auth refresh -h github.com -s delete_repo
```
*   **기대 결과**: 브라우저 창이 열리고 승인 후 터미널에 `✓ Authentication complete.` 메시지가 나타나야 합니다.

```powershell
# 4. GitHub 원격 저장소 삭제
gh repo delete llmragdev/vibe-pr-practice-study-llmragdev --yes
```
*   **기대 결과**: `✓ Deleted repository llmragdev/vibe-pr-practice-study-llmragdev` 메시지가 출력됩니다.
*   **오류 시 (404)**: "Not Found" 에러가 나면 이미 저장소가 삭제된 것이니 안심하셔도 됩니다.

```powershell
# 5. 삭제 결과 확인 (목록에서 사라졌는지 체크)
gh repo list --limit 5
```
*   **기대 결과**: 목록에 `vibe-pr-practice-study-llmragdev`라는 이름이 없으면 완벽하게 삭제된 것입니다.

## 🚀 다시 시작하기
모든 삭제가 완료되었다면, 이제 다음 가이드의 **1. 저장소 클론** 단계부터 다시 시작할 수 있습니다.
*   [03-0-pr-branch-issue2_practice(pre-work).md](./03-0-pr-branch-issue2_practice(pre-work).md)
