# Git Merge Skill for AI-DLC

AI-DLC 프로젝트에서 여러 개발자가 서로 다른 unit을 병렬로 개발할 때 발생하는 git merge conflict를 해결하는 스킬입니다.

## 언제 사용하나요?

INCEPTION phase가 완료되어 unit이 나뉜 프로젝트를 여러 개발자가 각자 PC에서 CONSTRUCTION phase를 진행한 후, git에 push/merge할 때 conflict가 발생하는 경우에 사용합니다.

### 발생하는 Conflict 유형

**1. AI-DLC 상태 파일 conflict**

각 개발자가 자기 unit의 진행 상태를 기록하면서 같은 파일을 수정합니다:
- `aidlc-docs/aidlc-state.md` — unit별 stage progress 체크박스
- `aidlc-docs/audit.md` — 작업 이력 로그

이 파일들은 논리적으로는 conflict가 아닙니다 (각자 다른 unit에 대한 기록). 스킬이 자동으로 병합합니다.

**2. 공통 코드 conflict**

공통 unit(shared library 등)을 한 개발자가 수정한 후, 다른 개발자가 같은 파일을 수정하거나 의존하는 경우:
- 공통 모듈에 서로 다른 함수/타입 추가
- 같은 설정 파일에 서로 다른 의존성 추가
- 공통 API 변경으로 인한 호환성 문제

## 사용 방법

### 1. 일반적인 workflow

```bash
# 개발자 A가 unit-a 작업 완료 후 push
git push origin main

# 개발자 B가 unit-b 작업 완료 후 pull → conflict 발생
git pull origin main
# CONFLICT 발생!
```

### 2. 스킬 실행

conflict가 발생한 상태에서 aidlc-main agent에게 요청합니다:

```
merge conflict 해결해줘
```

또는:

```
git merge conflict가 발생했어. 해결해줘.
```

### 3. 스킬이 하는 일

1. `git diff --name-only --diff-filter=U`로 conflict 파일 목록을 확인합니다
2. 각 파일을 **상태 파일** / **코드 파일**로 분류합니다
3. 상태 파일은 자동으로 병합합니다:
   - `aidlc-state.md` — 양쪽 unit의 progress를 모두 반영
   - `audit.md` — 양쪽 기록을 timestamp 순으로 정렬 합침
4. 코드 파일은 분석 후 해결 방안을 제시합니다:
   - **Additive** (양쪽 추가가 공존 가능) → 자동 merge 제안
   - **Overlapping** (같은 코드 수정) → 양쪽 비교 후 사용자 선택
   - **Dependency** (공통 API 변경) → 공통 unit 우선, 의존 unit 적응 제안
5. 모든 conflict 해결 후 검증합니다

## 예시 시나리오

```
프로젝트: e-commerce platform
├── unit-a (User Service) — 개발자 A
├── unit-b (Order Service) — 개발자 B
└── shared (Common Utils) — 양쪽에서 수정

개발자 A: shared/utils.ts에 generateId() 추가, bcrypt 의존성 추가
개발자 B: shared/utils.ts에 formatCurrency() 추가, stripe 의존성 추가

→ merge 시 utils.ts, package.json, aidlc-state.md, audit.md에서 conflict 발생
→ 스킬이 4개 파일 모두 해결
```
