# spec: devlife 워크플로우 체인 구축

## 1. 기능 개요

devlife-brainstorming → spec-creator → plan-creator → tdd-team 으로 이어지는 4단계 개발 워크플로우 체인을 구축한다. 각 스킬은 이전 스킬의 출력 문서를 입력으로 받아 중복 작업 없이 연결되며, 동시에 문서 없이 독립적으로도 사용할 수 있다.

### 작업 구성

| 작업 | 유형 | 설명 |
|------|------|------|
| **devlife-brainstorming 신규 생성** | 신규 | what/why 결정, 대화형, terminal state → spec-creator |
| **spec-creator 수정** | 수정 | brainstorming 문서 입력 분기 추가 + terminal state → plan-creator |
| **plan-creator 수정** | 수정 | spec 문서 입력 분기 추가 + terminal state → tdd-team |
| **tdd-team 수정** | 수정 | plan 문서 입력 분기 추가 (섹션 2+7 직접 읽기) |

---

## 2. 도메인 문맥 및 불변성

### 도메인 문맥

각 스킬은 체인으로 연결되어 사용될 때와 단독으로 사용될 때 모두 올바르게 동작해야 한다. 체인으로 사용 시에는 이전 스킬의 출력 문서를 입력받아 중복 단계를 건너뛰고, 단독 사용 시에는 기존 동작 방식을 그대로 유지한다. 각 스킬의 역할 구분은 다음과 같다: brainstorming은 제품/설계 결정(what/why), spec-creator는 기술 명세 및 서브태스크 분해, plan-creator는 태스크별 구현 상세, tdd-team은 TDD 사이클 실행.

### 비즈니스 불변성

- 문서 입력이 없어도 각 스킬은 독립적으로 완전히 동작해야 한다. 이전 단계 문서가 없다고 스킬이 중단되거나 오류를 내면 독립 사용이 불가능해진다.
- 체인 연결은 각 스킬의 terminal state에서만 이루어진다. 중간에 다른 스킬을 호출하면 워크플로우 흐름이 예측 불가능해진다.
- 도메인 불변성은 체인 내에서 한 번만 도출되어야 한다. 문서를 통해 전달받은 불변성을 재도출하면 충돌이 발생할 수 있다.

---

## 3. 비즈니스 로직

### 3-1. 문서 입력 분기 패턴 (spec-creator, plan-creator, tdd-team 공통)

```
스킬 시작 시:
  ├─ 이전 단계 문서가 제공되었는가?
  │   ├─ YES → 문서에서 필요한 섹션 읽기, 중복 단계 건너뜀
  │   └─ NO  → 기존 방식 (직접 탐색 + 질문)
  └─ 이후 로직 동일
```

### 3-2. 각 스킬의 입출력 계약

| 스킬 | 입력 (optional) | 읽는 섹션 | 건너뛰는 단계 | 출력 |
|------|----------------|---------|------------|------|
| devlife-brainstorming | 없음 | — | — | brainstorming 문서 |
| spec-creator | brainstorming 문서 | 전체 | 초기 목적/배경 질문 | `spec-{feature}.md` |
| plan-creator | spec 문서 | 전체 | 도메인 불변성 재질문 | `task-{feature}.md` |
| tdd-team | plan 문서 | 섹션 2 + 섹션 7 | 불변성 도출 + 태스크 분해 | TDD 사이클 실행 |

### 3-3. devlife-brainstorming 동작

1. 아이디어를 대화형으로 탐색 — 질문 하나씩
2. what(무엇을 만드는가), why(왜 만드는가), 핵심 설계 결정 확정
3. 결과물을 brainstorming 문서로 저장 (프로젝트 루트)
4. terminal state: spec-creator 호출

---

## 4. 구현 대상 파일

| 파일 경로 | 작업 | 변경 내용 |
|----------|------|---------|
| `~/.claude/skills/devlife-brainstorming/SKILL.md` | 신규 생성 ✅ | 대화형 브레인스토밍 스킬 전체 |
| `~/.claude/skills/spec-creator/SKILL.md` | 수정 ✅ | 문서 입력 분기 + Step 3.5 리뷰어 디스패치 + Step 5 terminal state |
| `~/.claude/skills/spec-creator/references/spec-reviewer.md` | 신규 생성 ✅ | 독립 서브에이전트 spec 리뷰어 |
| `~/.claude/skills/spec-creator/assets/spec-template.md` | 수정 ✅ | 섹션 4 제거, 섹션 3 체크리스트 → 상태 테이블 |
| `~/.claude/skills/plan-creator/SKILL.md` | 수정 ✅ | 문서 입력 분기 + Step 3.5 self-review + Step 5 terminal state |
| `~/.claude/skills/tdd-team/SKILL.md` | 수정 ✅ | 문서 입력 분기 + 중간 체크포인트 제거 + Cycle/Final Reviewer 디스패치 |
| `~/.claude/skills/tdd-team/references/cycle-reviewer.md` | 신규 생성 ✅ | 사이클별 독립 리뷰어 (TDD 프로세스 + 코드 품질) |
| `~/.claude/skills/tdd-team/references/final-reviewer.md` | 신규 생성 ✅ | 전체 완료 후 독립 리뷰어 (plan 대비 구현 완성도) |

---

## 5. 주요 고려사항

1. **brainstorming 문서 저장 위치**: 프로젝트 루트에 `brainstorming-{topic}.md`로 저장 예정. spec-creator가 동일 위치에서 읽을 수 있도록 경로를 일치시켜야 한다.

2. **tdd-team 섹션 7 활용**: plan 문서를 받았을 때 섹션 7 태스크 목록을 그대로 사용할지, 한 번 보여주고 확인받을지 결정 필요.
   - 대안 A: 그대로 사용 (빠름, 확인 없음)
   - 대안 B: 목록 보여주고 "진행할까요?" 확인 후 시작 (현재 tdd-team 방식과 일관성 있음)

3. **기존 스킬 파일 수정 범위**: spec-creator, plan-creator, tdd-team은 최소한의 변경만 가한다. 기존 동작 방식은 그대로 유지한다.

---

## 6. 구현 순서

### T1. devlife-brainstorming 신규 생성 ✅

- [x] `~/.claude/skills/devlife-brainstorming/SKILL.md` 생성
  - 스킬 메타데이터 (name, description, trigger 조건)
  - Setup: 프로젝트 컨텍스트 탐색
  - 대화 단계: 질문 하나씩, what/why/핵심 설계 결정
  - 문서 작성 단계: `brainstorming-{topic}.md` 프로젝트 루트에 저장
  - terminal state: spec-creator 호출 (독립 사용 시 선택)

### T2. spec-creator 수정 ✅

- [x] Document Input 섹션 추가 (brainstorming 문서 입력 분기)
- [x] Step 3.5 추가: 독립 서브에이전트 spec 리뷰어 디스패치
- [x] Step 5 terminal state 변경: plan-creator 호출 명시
- [x] `references/spec-reviewer.md` 신규 생성
- [x] `assets/spec-template.md` 수정: 섹션 4 제거, 섹션 3 태스크 상태 테이블로 개선

### T3. plan-creator 수정 ✅

- [x] Document Input 섹션 추가 (spec 문서 입력 분기)
- [x] Step 3.5 추가: self-review (spec 커버리지, placeholder, DisplayName, consistency)
- [x] Step 5 terminal state 추가: tdd-team 호출 명시

### T4. tdd-team 수정 ✅

- [x] Setup Step 3 앞에 plan 문서 입력 분기 추가
- [x] 중간 사용자 체크포인트 제거 (전체 자동 실행)
- [x] Cycle Reviewer 디스패치 추가 (사이클마다 독립 서브에이전트)
- [x] Final Review 섹션 추가 (전체 완료 후 독립 서브에이전트)
- [x] `references/cycle-reviewer.md` 신규 생성
- [x] `references/final-reviewer.md` 신규 생성

### T5. 토큰 최적화 ✅ (추가 작업)

- [x] spec-creator, plan-creator 체인 다이어그램 → 1줄 주석으로 교체
- [x] devlife-brainstorming Step 5 + Standalone Use 섹션 합침
- [x] cycle-reviewer / final-reviewer 공통 도입부 압축
- [x] cycle-reviewer APPROVED 중복 출력 템플릿 제거

---

## 7. 인수 조건

- [ ] devlife-brainstorming이 단독으로 실행되고 brainstorming 문서를 생성한다
- [ ] devlife-brainstorming 완료 후 spec-creator로 연결된다
- [ ] spec-creator가 brainstorming 문서 없이 단독 실행된다
- [ ] spec-creator가 brainstorming 문서를 받아 중복 질문 없이 실행된다
- [ ] spec-creator 완료 후 독립 리뷰어가 spec을 검토한다
- [ ] spec-creator 완료 후 plan-creator로 연결된다
- [ ] plan-creator가 spec 문서 없이 단독 실행된다
- [ ] plan-creator가 spec 문서를 받아 실행되고 tdd-team으로 연결된다
- [ ] plan-creator가 self-review로 placeholder/DisplayName/consistency를 검사한다
- [ ] tdd-team이 plan 문서 없이 단독 실행된다
- [ ] tdd-team이 plan 문서를 받아 섹션 2+7에서 직접 시작된다
- [ ] tdd-team이 각 사이클 후 독립 cycle-reviewer를 실행한다
- [ ] tdd-team이 전체 완료 후 독립 final-reviewer를 실행한다
- [ ] 전체 체인 (brainstorming → spec → plan → tdd)이 문서 전달로 연속 실행된다
