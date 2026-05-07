# Git Permission Guide

GitHub repository 권한과 branch protection을 정리한 참고 문서입니다.

## 1. 권한을 왜 나누는가

PR 실습에서는 사용자가 어떤 방식으로 협업하는지에 따라 권한을 다르게 줍니다.

```text
팀원 방식:
같은 repository에 branch를 push하고 PR 생성

외부 기여자 방식:
repository 권한 없이 fork에서 작업하고 PR 생성
```

## 2. GitHub repository 권한

| 권한 | 가능한 일 | push 가능 여부 |
| --- | --- | --- |
| Read | 코드 보기, clone, issue/PR 읽기 | 불가 |
| Triage | issue/PR 분류와 관리 일부 | 불가 |
| Write | branch 생성, commit push, PR 생성 | 가능 |
| Maintain | repository 운영 관리 일부 | 가능 |
| Admin | 설정 포함 전체 관리 | 가능 |

같은 repository 안에서 branch를 만들고 push하게 하려면 보통 `Write` 권한이 필요합니다.

## 3. 브랜치 작업만 가능하게 하려면

목표:

```text
작업 branch push는 허용
main 직접 push는 차단
PR review 후 merge만 허용
```

추천 설정:

```text
사용자 권한: Write
main branch: branch protection 설정
```

`Write` 권한만 주면 사용자가 `main`에도 직접 push할 수 있습니다. 그래서 `main` branch 보호 설정이 필요합니다.

## 4. main branch protection 추천값

GitHub repository에서 설정합니다.

```text
Settings
-> Branches
-> Branch protection rules
-> Add branch protection rule
```

Branch name pattern:

```text
main
```

추천으로 켤 항목:

```text
Require a pull request before merging
Require approvals
Require status checks to pass before merging
Require branches to be up to date before merging
Restrict who can push to matching branches
Do not allow bypassing the above settings
```

실습 repository라면 최소한 아래 두 가지는 켜는 편이 좋습니다.

```text
Require a pull request before merging
Restrict who can push to matching branches
```

## 5. 팀원 방식과 fork 방식 비교

| 구분 | 팀원 branch 방식 | fork 방식 |
| --- | --- | --- |
| repository 권한 | 보통 Write 필요 | 보통 필요 없음 |
| 작업 위치 | 원본 repository의 branch | 개인 fork repository |
| push 대상 | 원본 repository | 개인 fork repository |
| PR 방향 | `원본:작업branch` -> `원본:main` | `fork:작업branch` -> `원본:main` |
| main 보호 필요성 | 높음 | 높음 |

## 6. 실습별 추천 권한

### branch PR 실습

같은 repository 안에서 작업 branch를 push하는 실습입니다.

```text
Worker 권한: Write
main 보호: 필요
```

예시 흐름:

```text
worker가 원본 repository에서 branch 생성
-> branch에 push
-> main으로 PR 생성
-> manager가 review/merge
```

### fork PR 실습

원본 repository에 직접 push 권한이 없는 상황을 가정합니다.

```text
Worker 권한: 없어도 됨
원본 repository가 public이면 fork 가능
private이면 Read 권한과 fork 허용 설정 필요
main 보호: 필요
```

예시 흐름:

```text
worker가 원본 repository fork
-> fork repository에서 branch 생성
-> fork로 push
-> 원본 main으로 PR 생성
-> manager가 review/merge
```

## 7. 자주 헷갈리는 점

### git config 계정과 GitHub 로그인 계정은 다르다

```powershell
git config user.name
git config user.email
gh auth status
```

`git config user.name`, `git config user.email`은 commit 작성자 표시입니다.

실제로 GitHub에 push할 수 있는지는 `gh auth status`에 나온 로그인 계정과 repository 권한으로 결정됩니다.

### Write 권한은 branch 작업 권한이다

`Write` 권한은 작업 branch push에는 필요하지만, 그대로 두면 `main`에도 push할 수 있습니다.

그래서 실무에서는 보통 다음처럼 함께 사용합니다.

```text
Write 권한
+ main branch protection
```

### 권한이 없어도 PR은 가능할 수 있다

public repository라면 원본 repository에 권한이 없어도 fork를 만들고 PR을 보낼 수 있습니다.

```text
원본 repo write 권한: 필요 없음
fork 생성: 가능
PR 생성: 가능
merge: 원본 repo 권한 있는 사람이 수행
```

private repository는 다릅니다. private repository는 사용자가 repository를 볼 수 있어야 하고, repository나 organization 설정에서 fork가 허용되어 있어야 합니다.
