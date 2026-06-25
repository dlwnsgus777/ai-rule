# md-to-html

Markdown 파일을 독립형 HTML 페이지로 변환합니다.  
외부 의존성 없이 CSS/JS가 인라인으로 포함된 단일 `.html` 파일을 생성합니다.

## 언제 사용하나요?

- 계획 문서나 PRD를 시각적으로 보기 좋게 만들고 싶을 때
- Markdown을 웹페이지나 공유 문서로 변환할 때
- 외부 서버 없이 브라우저에서 바로 열 수 있는 문서가 필요할 때

## 트리거 문구

```
"MD를 HTML로 변환해줘"
"마크다운으로 웹페이지 만들어줘"
"이 MD 파일로 HTML 만들어줘"
"render this"
"make it look nice"
```

Markdown 내용을 직접 붙여넣어도 동작합니다.

## 사용 예시

```
"task-payment-refund.md를 HTML로 변환해줘"
→ task-payment-refund.html 생성
```

## 출력 파일 특성

- 모든 CSS/JS 인라인 포함 (외부 CDN 없음)
- 헤딩, 리스트, 인용문, 코드 블록, 테이블, 링크 모두 렌더링
- 소스 파일과 같은 디렉토리에 저장
- 브라우저에서 바로 열 수 있는 완전한 독립 파일
