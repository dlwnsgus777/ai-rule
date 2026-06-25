# cmux

cmux 터미널 앱을 프로그래밍 방식으로 제어합니다.  
pane 분할, 브라우저 내장, 알림, 터미널 입력 전송 등을 CLI 또는 소켓 API로 조작합니다.

## 언제 사용하나요?

- 터미널 pane을 자동으로 분할하고 싶을 때
- URL을 터미널 옆에 나란히 열고 싶을 때
- 작업 완료 시 알림을 받고 싶을 때
- 다른 pane에 명령어를 자동으로 전송할 때

> cmux는 tmux와 다른 별도의 앱입니다. cmux가 실행 중이 아니면 동작하지 않습니다.

## 트리거 문구

```
"cmux"
"pane 분할"
"브라우저 열어"
"새 pane 열어"
"알림 보내"
"cmux로 [작업]"
"workspace 만들어"
```

## 계층 구조

```
Window → Workspace (사이드바 탭) → Pane (분할 영역) → Surface (탭)
```

## 주요 명령어

### pane 분할

```bash
cmux new-split right     # 우측 분할
cmux new-split down      # 하단 분할
```

### 브라우저 열기

```bash
cmux browser open-split https://docs.example.com
# → OK surface=surface:2 pane=pane:2
```

### 다른 pane에 명령 전송

```bash
cmux send "npm run build\n"                     # 현재 포커스된 터미널
cmux send --surface surface:3 "pytest -v\n"     # 특정 surface
```

### 터미널 내용 읽기

```bash
cmux read-screen --surface surface:3 --lines 50
```

### 알림 전송

```bash
cmux notify --title "Build Complete" --body "All tests passed"
```

### 사이드바 상태 표시

```bash
cmux set-status build "compiling" --icon hammer --color "#ff9500"
cmux set-progress 0.5 --label "Building..."
cmux log --level success -- "All 42 tests passed"
```

## 환경변수

cmux 터미널 내에서 자동으로 설정됩니다:

| 변수 | 설명 |
|------|------|
| `CMUX_WORKSPACE_ID` | 현재 workspace ID |
| `CMUX_SURFACE_ID` | 현재 surface ID |
| `CMUX_SOCKET_PATH` | 소켓 경로 |

## 문제 해결

| 증상 | 해결 방법 |
|------|----------|
| `cmux: command not found` | `sudo ln -sf "/Applications/cmux.app/Contents/Resources/bin/cmux" /usr/local/bin/cmux` |
| 소켓 없음 | cmux 앱이 실행 중인지 확인 |
| `surface not found` | `cmux list-pane-surfaces`로 유효한 ID 확인 |
| Permission denied | Settings에서 소켓 모드 확인 또는 `CMUX_SOCKET_MODE=allowAll` 설정 |

## 관련 스킬

- [devlife-team-starter](./devlife-team-starter.md) — cmux 기반 멀티 에이전트 환경 구성
- [devlife-codex](./devlife-codex.md) — cmux pane을 통한 Codex 연동
