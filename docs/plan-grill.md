# plan-grill

계획 작성과 설계 검증을 한 번에 실행합니다.  
`plan-creator`로 계획을 작성한 직후 `grill-me`를 체인으로 실행하여 설계 약점을 파악합니다.

## 언제 사용하나요?

- 계획을 쓰면서 동시에 설계를 검증하고 싶을 때
- "이 방향이 맞나?" 확인이 필요할 때
- 계획서 + 심문을 한 흐름으로 처리하고 싶을 때

## 트리거 문구

```
"plan-grill"
"계획 작성하고 검증해줘"
"계획 짜고 심문해줘"
"계획 만들고 파고들어줘"
```

## 실행 흐름

```
Phase 1: plan-creator Steps 1~3 실행
         (코드 탐색 → 질문 → 계획 문서 작성)
         ↓
         "계획 문서 작성 완료. 검토 후 'grill me'라고 입력해 주세요."
         ↓ (사용자가 "grill me" 입력)
Phase 2: grill-me 실행
         (설계 결정 트리 전체 검증)
```

## 관련 스킬

- [plan-creator](./plan-creator.md) — Phase 1에서 실행
- [grill-me](./grill-me.md) — Phase 2에서 실행
