# 과제 유형 판별표

> **대상: 사람 참고.** 시험 진행 순서와 프롬프트는 [EXAM_RUNBOOK.md](../EXAM_RUNBOOK.md)에만 있다. 이 표는 프롬프트 1 결과(CONTRACT 9절 "형태")를 검토할 때와 경로 B에서 Codex에게 줄 전략 문서를 고를 때 쓴다.

## 신호 → 유형 → 경로

| 과제 README의 신호 | 유형 | 런북 경로 | Codex에게 경로로 줄 참고 문서 (최대 1~2개) |
|---|---|---|---|
| 로그인·역할·데이터 저장·화면이 명시, 스택 지정 없음 | A 업무 웹앱 | **A** | (boilerplate의 `BOILERPLATE.md`) |
| A + 배차·경로·스케줄 등 계산 | A + 최적화 | **A** | `strategies/optimization.md` |
| 웹 화면은 필요하지만 스택·스타터가 지정됨, 또는 로그인·저장 없는 단순 화면 | 지정 스택·단순 웹 | **B-웹** | `strategies/fullstack.md` |
| 결과 파일·알고리즘만 요구 | B 최적화 엔진 | **B-결과물** | `strategies/optimization.md` |
| 문서 검색·자연어 답변·인용 | C RAG | **B-결과물** (로그인·저장까지 필요하면 A) | `strategies/rag-agent.md` |
| 분석 보고·시각화·의사결정 | D 데이터 분석 | **B-결과물** | `strategies/data-analysis.md` |
| 기존 소스 + 실패 테스트·버그 재현 | E 코드 수리 | **B-수리** | `strategies/codebase-repair.md` |
| 답변 품질·평가 하네스 | F AI 평가 | **B-결과물** | `strategies/ai-evaluation.md` |
| CSV·JSON 변환·정제·집계 | G ETL | **B-결과물** | `strategies/etl.md` |
| 위 어디에도 깔끔히 맞지 않음 | 예상 밖 | **B-결과물** | `strategies/unknown-task.md` |

## 판별 원칙

- 혼합 과제는 **제출물과 채점자가 직접 확인할 핵심 여정**으로 정한다. 도메인(최적화, RAG)과 출력 형태(웹, 파일, CLI)를 분리해서 본다.
- 기존 소스와 실패 테스트가 있으면 무조건 수리(E)다. 새 프로젝트를 만들지 않는다.
- 로그인·역할이 요구되지 않으면 Auth·UI·DB를 만들지 않는다.
- 10분이 지나도 유형이 애매하면 분류를 멈추고 "산출물·입력·검증 방법" 세 가지로 결정한다.
