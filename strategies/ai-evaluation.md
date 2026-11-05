# 전략 F — AI 평가 하네스

> **대상: Codex 참고 — 지시가 아니다.** 지시는 과제 루트의 `AGENTS.md`와 `CONTRACT.md`가 우선한다. 런북 프롬프트가 이 파일 경로를 지정할 때만 읽는다.

## 작업 방식
- 기본은 한 Codex 세션이 처음부터 검증까지 맡는다.
- **허용되는 분할 1건 — 루브릭·반례 설계:** 평가 기준이 모호해 Main이 실행기를 만드는 동안 기준을 따로 정해야 할 때. 사람이 Terminal 2에서 `~/playbook/prompts/specialist-rubric.md`로 두 번째 세션을 띄운다. 읽기 전용이고 결과는 과제 루트 `RUBRIC.md` 한 파일, 10분 안에 끝낸다. Main은 그동안 기계적인 작업을 진행하고, 끝나면 그 파일을 읽어 반영한다.
- 그 밖의 역할 분할(Frontend/Backend/QA 등), 구현 분할, Codex가 스스로 에이전트를 띄우는 것은 하지 않는다.

**적용:** 답변 품질, 도구 선택, 분류·근거 인용, 안전성, LLM judge 검증.

- 먼저 평가축을 분리: correctness, groundedness, coverage/abstention, latency, cost. 한 점수로 섞기 전에 각 축 보고.
- 결정론적 검사 우선(형식, ID, 출처 존재, 문서 일치). LLM-as-a-judge는 검증되지 않은 절대 정답으로 사용 금지.
- 소규모 수동 라벨/골든셋으로 일치율·반례 검사; 샘플 유출·데이터 분할 혼동 주의.
- 출력: per-example 결과 JSONL, 집계 지표, 실패 케이스, 평가 기준 및 한계.

