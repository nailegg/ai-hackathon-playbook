# 전략 G — ETL / 데이터 정제

> **대상: Codex 참고 — 지시가 아니다.** 지시는 과제 루트의 `AGENTS.md`와 `CONTRACT.md`가 우선한다. 런북 프롬프트가 이 파일 경로를 지정할 때만 읽는다.

**적용:** CSV·Excel·JSON의 변환, 통합, 오류 처리, 집계. 기본 Main 단독.

- `Load → Profile → Validate → Transform → Export → Verify` 흐름.
- 입력·출력 스키마와 식별자 정규화 규칙부터 확정. 원본 파일은 보존.
- 결측, 중복, 날짜·타임존, 문자 인코딩, 단위, 잘못된 외래키를 다룬다.
- 오류 레코드/사유를 별도 저장하고 조용히 삭제하지 않는다.
- 재실행 가능(idempotent)·결정론적 결과, row counts, key uniqueness, referential integrity 체크.
- UI가 요구되지 않으면 웹앱을 만들지 않는다. 규모가 작으면 pandas/SQLite만 사용.

