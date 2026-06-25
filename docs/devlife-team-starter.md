# devlife-team-starter

cmux 터미널 앱에 Codex 에이전트 pane을 생성합니다.  
Claude 옆에 Codex를 나란히 실행하여 멀티 에이전트 개발 환경을 구성합니다.

## 언제 사용하나요?

- Claude와 Codex를 동시에 실행하여 병렬 작업을 하고 싶을 때
- `devlife-codex` 스킬로 태스크를 위임하기 전에 Codex pane을 준비할 때

## 트리거 문구

```
"개발인생 team"
"개발인생 팀 시작"
"멀티 에이전트 시작"
"codex 같이 띄워"
"에이전트 팀 만들어"
"start agent team"
"spawn agents"
```

## 사전 요구사항

| 항목 | 확인 방법 |
|------|----------|
| cmux 앱 실행 중 | `cmux ping` |
| cmux 터미널 내 실행 | `echo $CMUX_WORKSPACE_ID` (값이 있어야 함) |
| `codex` CLI 설치 | `command -v codex` |

## 실행 결과

```
Agent Team 구성 완료:
┌─────────────────┬─────────────────┐
│                 │ Codex           │
│  Claude (현재)   │ (-a never)      │
│                 │                 │
└─────────────────┴─────────────────┘
```

- Codex pane은 `"Codex"` 탭 이름으로 레이블됩니다
- 이미 실행 중인 경우 중복 생성하지 않고 알림만 표시합니다
- 실행 완료 후 포커스는 Claude pane으로 돌아옵니다

## 관련 스킬

- [devlife-codex](./devlife-codex.md) — Codex pane에 태스크 전송
- [cmux](./cmux.md) — cmux 터미널 제어 기반
