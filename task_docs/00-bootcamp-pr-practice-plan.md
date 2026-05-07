# 00. Bootcamp PR Practice Plan

이 문서는 부트캠프 Git/GitHub PR 실습의 전체 운영 방향을 설명합니다.

실습은 두 단계로 나누는 것을 권장합니다.

```text
1회차: 강사 저장소를 fork한 뒤 PR 보내기
2회차: 자기 GitHub 저장소를 만들어 전체 PR 사이클 수행하기
```

## 왜 두 단계로 나누는가

초보자에게 Git/GitHub PR 실습은 한 번에 다루기에는 내용이 많습니다.

한 번에 아래 내용을 모두 하면 2시간 안에 끝내기 어렵습니다.

```text
GitHub 계정 확인
GitHub CLI 로그인
fork
clone
branch 생성
코드 수정
commit
push
PR 생성
review
merge
GitHub Issue 생성
Closes #번호
Issue 자동 close
remote/repository 관리
```

그래서 첫 번째 수업에서는 fork 기반 PR을 통해 외부 기여자 흐름을 체험하고, 두 번째 수업에서는 자기 저장소를 만들어 전체 과정을 운영해보는 방식이 좋습니다.

## 1회차: 강사 저장소를 fork해서 PR 보내기

1회차 목표는 "외부 기여자" 역할을 체험하는 것입니다.

수강생은 강사 저장소에 직접 branch를 만들지 않습니다.

대신 강사 저장소를 자기 GitHub 계정으로 fork하고, 자기 fork에서 branch를 만들어 작업한 뒤 원본 저장소로 PR을 보냅니다.

원본 저장소 예:

```text
https://github.com/llmragdev/vibe-pr-practice
```

흐름:

```text
강사 저장소 fork
수강생 fork clone
branch 생성
코드 수정
commit
push to fork
PR 생성: 수강생 fork branch -> 강사 저장소 main
강사가 review
강사가 merge
```

수강생이 수행하는 일:

```text
fork
clone
main 최신화
branch 생성
코드 수정
commit
push
PR 생성
```

강사가 수행하는 일:

```text
PR 확인
diff 확인
review comment 작성
squash merge
필요 시 task_done 이동
```

1회차에서는 GitHub Issue까지 깊게 다루지 않아도 됩니다.

핵심은 다음 흐름입니다.

```text
fork -> branch -> commit -> push -> PR -> review -> merge
```

1회차 참고 문서:

```text
task_docs/01-pr-branch-issue1_practice.md
```

## 1회차를 fork로 하는 이유

강사 저장소에 직접 branch를 push하려면 수강생에게 write 권한을 줘야 합니다.

초보자 수업에서 모든 수강생에게 write 권한을 주면 다음 위험이 있습니다.

```text
실수로 main에 push할 수 있음
다른 사람 branch를 건드릴 수 있음
원격 branch가 많아져 관리가 어려워짐
권한 초대와 수락 절차가 번거로움
```

fork 방식은 수강생이 자기 저장소에서 안전하게 작업하고, 강사 저장소에는 PR만 보내는 방식입니다.

```text
원본 저장소:
llmragdev/vibe-pr-practice

수강생 fork:
student-id/vibe-pr-practice

PR 방향:
student-id:vibe/worker/... -> llmragdev:main
```

이 방식은 오픈소스 기여 흐름과도 비슷합니다.

## branch와 fork 차이

branch는 같은 저장소 안에서 작업선을 나누는 것입니다.

```text
llmragdev/vibe-pr-practice
  main
  vibe/worker/task-1
```

fork는 저장소 자체를 자기 GitHub 계정으로 복사하는 것입니다.

```text
원본:
llmragdev/vibe-pr-practice

fork:
student-id/vibe-pr-practice
```

정리:

| 구분 | branch | fork |
| --- | --- | --- |
| 위치 | 같은 저장소 안 | 자기 계정의 별도 저장소 |
| 권한 | 원본 저장소 write 권한 필요 | 원본 저장소 write 권한 불필요 |
| 주 용도 | 팀 내부 협업 | 외부 기여, 오픈소스 PR |
| PR 방향 | 같은 저장소 branch -> main | fork branch -> 원본 저장소 main |

## 2회차: 자기 GitHub 저장소에서 전체 사이클 수행

2회차 목표는 "저장소 관리자" 역할까지 체험하는 것입니다.

수강생은 강사 저장소에 PR만 보내는 것이 아니라, 자기 GitHub 저장소를 새로 만들어 전체 흐름을 직접 운영합니다.

권장 흐름:

```text
실습 자료 내려받기
자기 GitHub repository 생성
local repository와 자기 remote 연결
GitHub Issue 생성
branch 생성
코드 수정
commit
push
PR 생성
PR body에 Closes #번호 작성
review
squash merge
Issue 자동 close 확인
```

2회차에서는 GitHub Issue와 PR 연결을 함께 실습합니다.

핵심은 다음 흐름입니다.

```text
GitHub Issue -> branch -> commit -> push -> PR with Closes #번호 -> merge -> Issue closed
```

2회차 참고 문서:

```text
task_docs/02-0-pr-branch-issue2_practice.md
task_docs/02-sub-github-issue-closing-keywords.md
```

## 깨끗하게 다시 시작하는 방식

완전히 깨끗한 실습을 하려면 강사 저장소에 직접 branch를 만드는 방식보다, 각자 자기 GitHub 저장소를 만들어 진행하는 것이 좋습니다.

GitHub Issue와 PR은 branch가 아니라 repository 단위 기록입니다.

따라서 branch를 초기화해도 GitHub Issues와 Pull Requests 기록은 깨끗해지지 않습니다.

깨끗한 실습을 원하면 다음 방식 중 하나를 사용합니다.

```text
강사 자료를 zip으로 다운로드 후 자기 저장소에 push
GitHub template repository를 사용해서 자기 저장소 생성
fork 후 자기 fork에서 실습
새 repository를 직접 만들고 파일을 복사
```

부트캠프에서는 아래 방식이 가장 이해하기 쉽습니다.

```text
1회차: 강사 저장소를 fork해서 PR 보내기
2회차: 자기 GitHub 저장소를 새로 만들어 전체 흐름 수행하기
```

## task, task_done, task_docs 역할

이 저장소의 폴더 역할은 다음과 같습니다.

```text
task/
```

현재 수행할 작업 카드입니다.

예:

```text
task/backend/issue-2.md
task/backend/review-2.md
```

```text
task_done/
```

완료된 작업 카드를 보관하는 곳입니다.

부트캠프 실습에서는 완료 상태를 눈으로 확인하기 위해 task 파일을 `task_done`으로 옮깁니다.

실무에서는 보통 GitHub Issue, PR, merge 기록이 완료 기록 역할을 하므로 task 파일을 직접 이동하지 않는 경우가 많습니다.

```text
task_docs/
```

강의용 설명서, 샘플 소스, 실행 로그 예시를 보관하는 곳입니다.

예:

```text
task_docs/01-pr-branch-issue1_practice.md
task_docs/02-0-pr-branch-issue2_practice.md
task_docs/log_samples/
task_docs/src_samples/
```

## 추천 수업 시간 배분

2시간 수업에서는 issue1과 issue2를 모두 완주하기 어렵습니다.

권장 구성:

```text
00:00-00:20  Git/GitHub/fork/branch/PR 개념 설명
00:20-00:35  환경 확인: gh auth status, git status, 폴더 구조
00:35-01:25  issue1 실습: fork, clone, branch, commit, push, PR
01:25-01:45  강사 review/merge
01:45-02:00  issue2 예고: GitHub Issue, Closes #번호, squash merge 설명
```

issue1과 issue2를 모두 직접 실습하려면 최소 3시간 이상을 권장합니다.

## 실습자가 가장 먼저 볼 문서

처음에는 이 문서를 보고 전체 구조를 이해합니다.

그 다음 아래 순서로 봅니다.

```text
1. task_docs/01-pr-branch-issue1_practice.md
2. task_docs/02-0-pr-branch-issue2_practice.md
3. task_docs/02-sub-github-issue-closing-keywords.md
```

실제 명령을 따라 칠 때는 로그 샘플을 같이 봅니다.

```text
task_docs/log_samples/
```

