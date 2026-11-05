# AI Hackathon 180분 Codex Playbook

> **시험 당일에는 [EXAM_RUNBOOK.md](EXAM_RUNBOOK.md) 하나만 열고 위에서부터 따른다.** 다른 문서는 런북이 경로를 지정할 때만 연다.

SK하이닉스 AI 해커톤 2차 예선 대비용 운영 절차. 과제 정답이 아니라 요구사항 계약, 에이전트 운영, 검증·제출 절차다. 이 저장소는 시험 환경에서 과제 폴더 **밖**(`~/playbook`)에 clone한다.

## 진입점은 둘뿐

| 누가 | 진입점 | 그 밖의 문서 |
|---|---|---|
| 사람 | `EXAM_RUNBOOK.md` | 런북이 "~/playbook/…을 보라"고 할 때만 |
| Codex | 과제 루트의 `AGENTS.md` (`templates/AGENTS.md` 복사본) | 프롬프트가 경로를 지정할 때만. 이 저장소의 다른 문서는 지시가 아니라 참고 |

복사용 프롬프트는 **런북과 `prompts/specialist-*.md`에만** 있다. 다른 문서에 프롬프트를 두지 않는다.

## 문서 지도

| 경로 | 대상 | 언제 |
|---|---|---|
| [EXAM_RUNBOOK.md](EXAM_RUNBOOK.md) | 사람 | 시험 내내 |
| [templates/AGENTS.md](templates/AGENTS.md), [templates/CONTRACT.md](templates/CONTRACT.md) | Codex | T+0에 과제 루트로 복사 |
| [templates/OR_SPEC.md](templates/OR_SPEC.md) | 사람 | 최적화 정식화가 필요할 때 직접 채움 |
| [checklists/boilerplate-intake.md](checklists/boilerplate-intake.md) | 사람 | boilerplate 판정 기준과 제공 기능 (기준은 프롬프트 1에 이미 들어 있음) |
| [checklists/task-types.md](checklists/task-types.md) | 사람 | 계약 검토, 경로 B에서 줄 전략 문서 고르기 |
| [strategies/](strategies) | Codex | 경로 B에서 유형별로 1개만 경로 지정 |
| [prompts/specialist-*.md](prompts) | 두 번째 세션 | 독립 분석이 꼭 필요할 때만 |
| [checklists/submission.md](checklists/submission.md) | 사람 | T+160 제출 |
| [checklists/model-recovery.md](checklists/model-recovery.md) | 사람 | capacity 오류 |
| [checklists/auth-security.md](checklists/auth-security.md) | 사람 | 권한 과제 감사 시 |
| [AGENT_RULES.md](AGENT_RULES.md), [skills/](skills), [mcp/](mcp) | 사람 | 사전 학습용. 시험 중에는 거의 쓰지 않음 |

## 시험 환경 조건

- 개발 180분, 제출 이후 서술형 **문항당 5분**, 해당 단계 **AI 사용 금지**. 문항 수는 실제 시험 UI 확인.
- 제공된 Codex만 사용. 허용 모델 목록에서 선택하며 **Astra 사용 금지**. 개인 API 키·외부 AI 금지.
- 개발 서버는 `0.0.0.0` 바인딩, 플랫폼 내부 Browser로 확인. 서버 터미널은 닫지 않는다.
- 공개 Git 저장소 clone 허용(본인 사전 제작 저장소 포함). **`git commit`·`git push` 금지**, 시험 문제·결과의 외부 반출 금지. 진행 기록은 `CONTRACT.md` 체크박스와 `DECISIONS.md`.
- 개인 PC 파일 업로드 불가. 외부 MCP 연결은 규정과 연결 가능 여부를 확인한 뒤에만.

## 과거 교훈

지난 과제는 관리자/직원 Auth가 필요한 **통근버스 노선 최적화 웹앱**으로, 목적함수가 모호했다. Director/Frontend/Backend/QA를 매 단계 반복 호출해 조율 비용이 컸고, OR 특화 분석이 부족했다. 모의 시험에서는 boilerplate 위에 구현을 얹으며 `AGENTS.md`가 꼬이고 더미 코드가 과제 기능처럼 쓰였다. 연습 과제는 `Selected model is at capacity` 오류로 중단되었다. **완성도와 모델 장애를 구분**해 회고한다.

> 시험 안내와 운영진의 최신 지침이 언제나 우선한다. 이 자료는 SK하이닉스/크래프톤의 공식 문서가 아니다.
