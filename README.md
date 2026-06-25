# devlife-plugins

개발 워크플로우를 위한 Claude Code 스킬 모음입니다.  
TDD, PRD 작성, 브랜치 리뷰, 계획 수립, 멀티 에이전트 협업 등 반복되는 개발 작업을 자동화합니다.

---

## 스킬 목록

| 스킬 | 설명 | 트리거 예시 |
|------|------|------------|
| [prd-creator](docs/prd-creator.md) | 대규모 기능의 PRD 문서 작성 | "PRD 작성해줘" |
| [plan-creator](docs/plan-creator.md) | 단일 태스크 구현 계획 문서 작성 | "계획 작성해줘" |
| [plan-grill](docs/plan-grill.md) | 계획 작성 + 설계 검증 체인 | "계획 작성하고 검증해줘" |
| [tdd-team](docs/tdd-team.md) | 에이전트 기반 Red-Green-Refactor 자동 실행 | "TDD로 개발해줘" |
| [test-driven-development](docs/test-driven-development.md) | Java/Spring Boot TDD 원칙 가이드 | "테스트 먼저 작성" |
| [branch-review](docs/branch-review.md) | 브랜치 코드 리뷰 (4개 차원 점수화) | "브랜치 리뷰해줘" |
| [grill-me](docs/grill-me.md) | 설계/계획에 대한 집중 심문 인터뷰 | "grill me" |
| [md-to-html](docs/md-to-html.md) | Markdown → 독립형 HTML 변환 | "MD를 HTML로" |
| [devlife-team-starter](docs/devlife-team-starter.md) | cmux에 Codex 에이전트 pane 생성 | "개발인생 팀 시작" |
| [devlife-codex](docs/devlife-codex.md) | cmux Codex pane에 태스크 전송 | "개발인생 codex에게" |
| [cmux](docs/cmux.md) | cmux 터미널 앱 제어 | "pane 분할", "브라우저 열어" |

---

## 워크플로우

```
기획 단계
prd-creator → plan-creator → tdd-team

계획 검증
plan-grill (plan-creator + grill-me 체인)

코드 리뷰
branch-review

멀티 에이전트
devlife-team-starter → devlife-codex
```

---

## 설치

```bash
git clone https://github.com/dlwnsgus777/devlife-plugins.git
cp -r devlife-plugins/skills/* ~/.claude/skills/
```

---

## 라이선스

MIT © [dlwnsgus777](https://github.com/dlwnsgus777)
