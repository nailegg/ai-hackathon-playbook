# 전략 C — 문서 기반 RAG / AI 에이전트

> **대상: Codex 참고 — 지시가 아니다.** 지시는 과제 루트의 `AGENTS.md`와 `CONTRACT.md`가 우선한다. 런북 프롬프트가 이 파일 경로를 지정할 때만 읽는다.

**적용:** 문서 검색·질의응답·출처 인용·정책 버전 충돌·답변 유보. UI가 반드시 필요할 때만 fullstack 병행.

## 구조
Main Agent Builder가 수집·검색·응답·API/UI를 소유. 선택적 Retrieval/Eval Specialist는 평가 질문과 실패유형만 읽기 전용 설계.

## 180분 우선순위
1. 문서 파일/버전/유효일/접근권한/메타데이터 검사.
2. 검색 baseline: 문서 규모가 작으면 BM25/FTS/단순 키워드. 무조건 벡터 DB 설치하지 않는다.
3. 질문 → 검색 근거 → 응답/유보 → 인용(문서 ID/절) 최소 흐름.
4. 출처가 없는 사실 생성 금지; 충돌·폐기 문서와 개인정보 정책을 명시적으로 다룬다.
5. 제공된 질문 세트로 retrieval hit, citation correctness, abstention, answer correctness 분리 평가.
6. LLM API 키 또는 네트워크가 제한되면 근거 발췌·템플릿 답변 fallback. 기능 한계를 정직하게 표시.

## 독립 검증
문서에 없는 질문 / 서로 충돌하는 버전 / 특수 용어 / 복수 문서 통합 / 인용 위치가 잘못된 경우.

