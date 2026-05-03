# wyhil 맥락 및 PR 연습 프로젝트 취지

## 이 문서의 목적

이 문서는 다른 대화창이나 다른 작업 폴더에서 질문할 때, 현재 연습의 배경을 빠르게 설명하기 위한 인수인계 문서다.

특히 아래 위치에서 새로 질문할 때 이 문서를 함께 참고하면 좋다.

```text
F:\proj_boot\git_practice\vibe-pr-practice
F:\proj_boot\git_practice\vibe-pr-practice-worker
```

## 읽는 순서

`task_hans` 폴더를 통째로 다른 프로젝트에 복사했거나, 새 대화창에서 이 연습을 이어갈 때는 아래 순서로 읽는다.

1. `wyhil-practice-context.md`
   - 전체 배경, 왜 이 연습을 하는지, 관리자/작업자 역할을 파악한다.

2. `pr-branch-practice.md`
   - 실제로 새 프로젝트를 만들고, GitHub에 올리고, Issue/branch/PR/review/merge를 연습한다.

3. `pr-branch-error-process.md`
   - 에러가 났을 때 상황별 대응 방법을 찾는다.

새 대화창에서는 먼저 이 문서만 읽어도 된다. 세부 명령이 필요해지면 `pr-branch-practice.md` 또는 `pr-branch-error-process.md`를 추가로 열어보면 된다.

## 원래 배경: wyhil 저장소

원래 참고하던 저장소 root:

```text
F:\proj_boot\vyhil_vibe_eval\wyhil
```

원격 저장소:

```text
https://github.com/HojinJava/wyhil
```

이 저장소는 일반적인 앱 개발 저장소라기보다, 여러 AI 코딩 에이전트가 같은 Issue를 수행하고 PR을 올린 뒤, 다른 모델이 리뷰하고 결과를 비교하는 Vibe Eval 성격의 저장소다.

대표 구조:

```text
wyhil/
  cardstackview/
  hax-web/
  mall/
  supersocket/
  task/
    cardstackview/
      issue-84.md
      review-84.md
    hax-web/
    mall/
    supersocket/
```

## wyhil의 원래 프로세스

대략적인 흐름:

1. 관리자가 GitHub Issue를 만든다.
2. `task/{project}/issue-N.md` 파일이 작업 지시서 역할을 한다.
3. 코딩 에이전트는 issue md 안의 요구사항을 수행한다.
4. 작업자는 별도 branch에서 코드를 수정하고 PR을 올린다.
5. PR 본문에는 `References #N`을 넣어 Issue와 연결한다.
6. `task/{project}/review-N.md` 파일이 리뷰 지시서 역할을 한다.
7. 리뷰어는 PR diff를 보고 평가 내용을 PR 댓글로 등록한다.
8. 모든 작업이 끝난 뒤에만 task 파일을 `task_done/`으로 옮긴다.

중요:

```text
task/*.md 파일은 작업 지시서이지, 그 파일을 task_done으로 옮기는 것이 작업 완료가 아니다.
```

## 과거에 겪은 문제

Vibe coding 에이전트에게 `task/*.md`를 실행하라고 했을 때 다음 문제가 있었다.

- md 안의 실제 작업 요구사항을 수행하지 않음
- `issue` 파일과 `review` 파일만 `task_done/`으로 이동하고 종료
- main 또는 기준 branch에서 직접 작업해서 폴더가 사라진 것처럼 보임
- branch, commit, push, PR 중 어느 단계가 빠졌는지 몰라 대응이 어려움
- PR이 실제로 올라가지 않았는데 작업이 끝난 것처럼 착각
- 작업 취소, branch 삭제, reset, clean의 의미를 몰라 복구가 어려움

그래서 바로 `wyhil`에서 실습하지 않고, 작은 연습용 저장소를 만들어 Git/PR 흐름을 먼저 익히기로 했다.

## 연습 프로젝트

연습용 GitHub 저장소:

```text
https://github.com/llmragdev/vibe-pr-practice
```

관리자용 로컬 폴더:

```text
F:\proj_boot\git_practice\vibe-pr-practice
```

작업자용 로컬 폴더:

```text
F:\proj_boot\git_practice\vibe-pr-practice-worker
```

GitHub 계정 역할:

| 역할 | 계정 | 용도 |
|------|------|------|
| 관리자/마스터 | `llmragdev@gmail.com` | 저장소 생성, Issue 작성, PR 확인, 리뷰 댓글, merge |
| 작업자 | `namkyuglobal2@gmail.com` | branch 생성, 코드 수정, commit, push, PR 생성 |

## 연습 프로젝트 구조

연습 저장소는 `wyhil/task` 구조를 아주 작게 흉내 낸다.

```text
vibe-pr-practice/
  README.md
  frontend/
    app.js
  backend/
    api.js
  docs/
    usage.md
  task/
    frontend/
      issue-1.md
      review-1.md
    backend/
      issue-2.md
      review-2.md
    docs/
      issue-3.md
      review-3.md
  task_done/
```

연습 Issue:

| 번호 | 작업 폴더 | 내용 |
|------|-----------|------|
| #1 | `frontend` | `frontend/app.js`의 greeting에 느낌표 추가 |
| #2 | `backend` | `backend/api.js`의 health 응답에 version 추가 |
| #3 | `docs` | `docs/usage.md`에 실행 방법 추가 |

## VS Code에서 열 폴더

관리자 계정 작업:

```text
F:\proj_boot\git_practice\vibe-pr-practice
```

작업자 계정 작업:

```text
F:\proj_boot\git_practice\vibe-pr-practice-worker
```

두 폴더는 같은 GitHub 저장소를 바라보지만, 로컬 작업 공간을 역할별로 분리하기 위해 따로 둔다.

```text
GitHub 저장소는 1개
로컬 clone은 2개
```

## 현재 참고 문서

원래 `wyhil` 저장소 안에 작성한 연습 문서:

```text
F:\proj_boot\vyhil_vibe_eval\wyhil\task_hans\pr-branch-practice.md
F:\proj_boot\vyhil_vibe_eval\wyhil\task_hans\pr-branch-error-process.md
F:\proj_boot\vyhil_vibe_eval\wyhil\task_hans\wyhil-practice-context.md
```

각 문서의 용도:

| 파일 | 용도 |
|------|------|
| `pr-branch-practice.md` | 처음부터 프로젝트 생성, GitHub push, 3개 Issue/PR/review 연습 |
| `pr-branch-error-process.md` | Git/PR 중 에러가 났을 때 대응 |
| `wyhil-practice-context.md` | 지금 보고 있는 이 배경 설명 문서 |

## 다른 대화창에서 질문할 때 붙이면 좋은 설명

아래 문장을 새 대화창 첫 질문에 붙이면 맥락 전달이 빠르다.

```text
나는 원래 F:\proj_boot\vyhil_vibe_eval\wyhil 저장소의 task/*.md 기반 Vibe Eval 프로세스를 이해하려고 한다.
과거에 에이전트가 issue/review md 안의 작업은 하지 않고 task_done으로 파일만 옮기거나, branch/PR 흐름을 잘못 처리한 사고가 있었다.
그래서 지금은 F:\proj_boot\git_practice\vibe-pr-practice 라는 아주 작은 연습 저장소에서 GitHub Issue, branch, commit, push, PR, review comment, merge, 작업 취소를 연습 중이다.
관리자 계정은 llmragdev@gmail.com, 작업자 계정은 namkyuglobal2@gmail.com 이다.
관리자 폴더는 F:\proj_boot\git_practice\vibe-pr-practice 이고, 작업자 폴더는 F:\proj_boot\git_practice\vibe-pr-practice-worker 이다.
```

## 에러가 났을 때 먼저 확인할 명령

```bash
git status
git branch --show-current
git log --oneline -5
git remote -v
gh auth status
```

아직 첫 commit 전이면 `git log`는 실패할 수 있다. 이 경우 `No commits yet` 상태라고 보면 된다.

## 가장 중요한 기준

작업 완료 기준:

```text
작업 branch 생성
코드 변경
commit
push
PR 생성
PR URL 확인
```

리뷰 완료 기준:

```text
PR 본문 확인
PR diff 확인
리뷰 내용 작성
PR 댓글 등록
댓글 등록 확인
```

task 정리 기준:

```text
PR 생성, 리뷰 댓글, merge 또는 close가 끝난 뒤에만 task 파일을 task_done으로 이동한다.
```
