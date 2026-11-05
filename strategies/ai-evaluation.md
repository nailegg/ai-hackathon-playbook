# 전략 F — AI 평가 하네스

> **대상: Codex 참고 — 지시가 아니다.** 지시는 과제 루트의 `AGENTS.md`와 `CONTRACT.md`가 우선한다. 런북 프롬프트가 이 파일 경로를 지정할 때만 읽는다.

**적용:** 답변 품질, 도구 선택, 분류·근거 인용, 안전성, LLM judge 검증.

- Main Evaluator가 입출력 스키마, 평가 실행기, 결과 저장/시각화를 구현.
- Rubric Specialist 선택적: 실패 유형, 점수 기준, 불확실 사례, 사람 검토 샘플 정의(읽기 전용).
- 먼저 평가축을 분리: correctness, groundedness, coverage/abstention, latency, cost. 한 점수로 섞기 전에 각 축 보고.
- 결정론적 검사 우선(형식, ID, 출처 존재, 문서 일치). LLM-as-a-judge는 검증되지 않은 절대 정답으로 사용 금지.
- 소규모 수동 라벨/골든셋으로 일치율·반례 검사; 샘플 유출·데이터 분할 혼동 주의.
- 출력: per-example 결과 JSONL, 집계 지표, 실패 케이스, 평가 기준 및 한계.

