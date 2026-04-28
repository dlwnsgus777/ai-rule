# Branch Code Review

Analyze all changes on the current branch against `main`, score across 4 dimensions, and save a timestamped review report.

## Step 1 — Gather Branch Changes

```bash
git diff main...HEAD
git log --no-merges main..HEAD --oneline
git branch --show-current && date +%Y%m%d
```

Delegate all evaluation to a single fresh sub-agent — pass the full diff as context so it reads it only once across all 4 dimensions.

## Step 2 — Evaluation (4 Dimensions)

### A — Code Conventions (25%)
- Domain prefix naming (`Ecp`, `App`, `Admin`, `Fitting`, etc.)
- DTO pattern: `{Domain}{Feature}RequestV{N}` / `ResponseV{N}`
- `@RequiredArgsConstructor` + `private final` constructor injection
- Method reference preferred; `it ->` for single-param lambdas
- Line ≤150 chars, 4-space indent, public before private
- No `var` usage — always declare explicit types
- When a method has 4 or more parameters, wrap them in an object instead of passing individually
- When a constructor has many parameters, use the builder pattern (`@Builder`)

### B — Test Quality (30%)
- Test package path mirrors source path exactly
- AAA comments: `// arrange`, `// act`, `// assert`
- `isEmpty()` vs `doesNotContain()` intent
- `@Nested` grouping for 10+ related tests
- SUT named `sut`; no tests outside `acuvue-application`
- `@DisplayName` in Korean

### C — Domain Logic (30%)
- Business rule branches correct and complete
- `@Transactional` propagation design
- Exception type appropriateness
- `@Nationalized` on NVARCHAR columns
- Feign client for external calls
- Domain logic encapsulated in enum methods

### D — Design Quality (15%)

Before scoring, scan `CLAUDE.md` (and any module-level CLAUDE.md) to locate shared modules; flag new code that reimplements logic already available there.

- Single responsibility; no circular dependencies
- Repository pattern; minimal direct EntityManager use
- No magic numbers/strings; record-based DTOs

#### Maintainability
- Method >~30 lines → likely mixed responsibilities
- Deep nesting (>3 levels) or high branch complexity → prefer early returns or extracted methods
- Names reveal intent without a comment; no dead code or hidden side effects

#### Code Smells
- **Duplicate code** → missing abstraction
- **Large class** → split by responsibility
- **Data clumps** → extract repeated field/param groups into a class
- **Feature envy** → method belongs in the class whose data it uses most
- **Message chains** `a.b().c().do()` → Law of Demeter violation
- **Speculative generality** → remove unused abstractions (YAGNI)
- **Switch/if-else on type** → replace with polymorphism or strategy
- **Shotgun surgery** → one change scattered across many unrelated files

#### Domain-Driven Design (DDD)
- **Ubiquitous language** — names match the domain terminology a business expert would use
- **Value objects** — domain concepts identified by value (Money, Email) must be immutable types, not raw primitives; flag primitive obsession
- **Aggregate boundaries** — external code accesses aggregates only through the root
- **Rich domain model** — invariants live inside domain objects, not services; flag anemic models where classes are pure data containers
- **Domain events** — significant state transitions are explicit events, not buried side effects
- **Repository per aggregate** — not per table; flag arbitrary query repositories
- **Domain services** — cross-aggregate logic that belongs to no single entity

## Step 3 — Scoring

Grades: A(90+) B(80+) C(70+) D(60+) E(50+) F(<50)

**Overall** = (A×0.25) + (B×0.30) + (C×0.30) + (D×0.15)

## Step 4 — Save Report

- Path: `.reviews/review-{YYYYMMDD}-{branch}.md` (create folder if missing)
- Compare with latest existing review for trend (▲ ▼ —)

Report schema:

```
# 브랜치 리뷰 — {브랜치명}
> 날짜: {YYYY-MM-DD} | 커밋: {N} | 변경 파일: {N}개

## 점수 요약
| 차원 | 점수 | 등급 | 추이 |
코드 컨벤션 / 테스트 품질 / 도메인 로직 / 설계 품질 / 종합

## 차원별 상세
각 차원: 강점 bullet + 개선 필요 `file:line` — 설명

## 우선 개선 항목 Top 5
[심각도] `file:line` — 설명

## 인사이트
이번 브랜치에서 발견된 주목할 패턴 2~3가지
```
