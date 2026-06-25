# devlife-codex

cmux에서 실행 중인 Codex pane에 태스크를 전송하고 결과를 파일로 수집합니다.  
`devlife-team-starter`로 시작한 Codex 인스턴스에 프롬프트를 보내고, 결과가 파일에 작성되면 자동으로 읽어옵니다.

## 언제 사용하나요?

- Claude가 진행 중인 작업을 방해하지 않고 Codex에 병렬 태스크를 위임할 때
- Codex의 결과를 파일로 받아 Claude가 이어서 처리할 때

## 트리거 문구

> **`"개발인생 codex"` 접두사가 반드시 필요합니다.**  
> 단순히 `"codex"` 라고만 하면 다른 codex 스킬과 혼동됩니다.

```
"개발인생 codex에게 [태스크] 시켜줘"
"개발인생 codex한테 [태스크] 맡겨줘"
"개발인생 codex로 [태스크] 처리해줘"
"개발인생 codex에게 물어봐"
```

## 실행 흐름

```
1. cmux에서 "Codex" 제목의 pane 탐색
2. 결과 파일 경로 준비 (devlifeteam/codex-result-{timestamp}.md)
3. Codex pane에 프롬프트 전송
4. 백그라운드에서 파일 생성 감지 (최대 30분 대기)
5. 결과 파일이 생성되면 내용을 읽어 사용자에게 표시
```

## 사전 요구사항

`devlife-team-starter`로 Codex pane이 먼저 실행 중이어야 합니다.  
pane이 없으면 다음 메시지를 표시합니다:
```
"Codex pane을 찾을 수 없습니다. devlife-team-starter 스킬로 먼저 시작하세요."
```

## 결과 파일 위치

```
{프로젝트루트}/devlifeteam/codex-result-{timestamp}.md
```

결과 파일은 누적 저장되며, 정리는 사용자가 직접 합니다.

## 관련 스킬

- [devlife-team-starter](./devlife-team-starter.md) — Codex pane 생성 선행 작업
- [cmux](./cmux.md) — pane 제어 기반
