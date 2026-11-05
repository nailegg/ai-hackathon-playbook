# AI Hackathon 180분 Codex CLI Playbook

SK하이닉스 AI 해커톤 2차 예선 대비용 **실행 전략 레퍼런스**. 공개 GitHub에 게시하고 시험 환경에서 Clone해 사용할 수 있는 문서·경량 Skill 모음이다. **과제 정답이나 사전 작성된 답안이 아니라** 문제 유형 판별, 에이전트 운영, 테스트/제출에 관한 절차다.

## 사용법: 딱 3단계
> 시험 시작 직후에는 **[EXAM_RUNBOOK.md](EXAM_RUNBOOK.md)**를 열고 그대로 따른다. 아래 3단계와 나머지 문서는 런북이 필요할 때 가리키는 참고 자료다.

1. 문제 폴더의 `README.md`와 데이터를 먼저 읽고 [START_HERE.md](START_HERE.md) 또는 [prompts/initial-triage.md](prompts/initial-triage.md)로 유형을 판별한다.
2. 웹 서비스 과제면 [반입 절차](checklists/boilerplate-intake.md)로 Web Boilerplate 사용 여부를 정한다. 예상 유형이 맞으면 필요한 전략 문서 **최대 2개**만 골라 읽는다. 맞지 않으면 [Unknown Task Mode](strategies/unknown-task.md)를 사용한다. 예: 업무 최적화 웹앱은 [fullstack](strategies/fullstack.md) + [optimization](strategies/optimization.md).
3. Main Codex가 구현을 소유하고, **필요한 경우에만** 읽기 전용 Specialist를 호출한다. T+135분 이후 기능 추가보다 검증·제출을 우선한다.

## 유형 선택표
| 유형 | 전략 | 기본 구조 |
|---|---|---|
| A 업무 최적화 웹앱 | fullstack + optimization | Main Full-stack + 선택적 OR specialist |
| B 최적화 엔진 | optimization | Main Solver + 선택적 독립 검증 |
| C 사내 문서 RAG | rag-agent (+ fullstack UI 필수 시) | Main + 선택적 Retrieval/Eval specialist |
| D 데이터 분석·의사결정 | data-analysis | Main Analyst + 선택적 Data quality specialist |
| E 기존 코드 수리 | codebase-repair | Main Repair + 선택적 읽기 전용 Explorer |
| F AI 품질 평가 | ai-evaluation | Main Evaluator + 선택적 Rubric specialist |
| G ETL·정제 | etl | 기본 Main 단독 |
| **예상 밖/혼합/새 형식** | [unknown-task](strategies/unknown-task.md) | Main 단독 + 필요한 경우 읽기 전용 탐색 |

## 시험 환경에서 확정된 조건
- 개발 180분, 제출 이후 후속 서술형 **문항당 5분**, 해당 단계 **AI 사용 금지**. 문항 수는 실제 시험 UI 확인.
- Codex CLI는 제공 환경에서 실행. 외부 브라우저 직접 탐색은 제한되고 서비스 확인은 플랫폼 내부 Browser 미리보기 활용.
- 개발 서버는 `0.0.0.0` 바인딩. 서버 터미널은 닫지 않는다.
- 공개 Git 저장소 Clone 허용. **사용자 확인에 따르면 본인 사전 제작 공개 Git 저장소의 Clone도 허용됨.** 단, 시험 문제·작업 결과의 외부 반출과 **`git commit`·`git push`는 금지**된다. 진행 기록은 `CONTRACT.md` 체크박스와 `DECISIONS.md`로 남긴다.
- 개인 PC 파일 업로드 불가. 모델은 시험 제공 목록의 허용 모델만 사용하며 **Astra 사용 금지**.
- 개인 API 키, 비공개 파일 및 시험 데이터의 외부 전송 금지. 외부 MCP 연결도 시험 규정과 연결 가능 여부를 확인한다.
- 15분 사전 체험은 사용 완료.

## 과거 교훈
지난 과제는 관리자/직원 Auth가 필요한 **통근버스 노선 최적화 웹앱**으로, 목적함수는 모호했다. Director/Frontend/Backend/QA를 매 단계 반복 호출해 조율 비용이 컸고, OR 특화 분석과 CLI 활용이 부족했다. 연습용 제조 스케줄링 과제는 `Selected model is at capacity. Please try a different model.` 오류로 중단되었다. **완성도와 모델 장애를 구분**해 회고한다.

## 파일 안내
- [EXAM_RUNBOOK.md](EXAM_RUNBOOK.md): **T+0부터 제출까지의 단계와 복사용 프롬프트 (시험 당일 기준 문서)**
- [templates/](templates): 과제 루트에 복사할 `AGENTS.md`, `CONTRACT.md`
- [START_HERE.md](START_HERE.md): 5분 분기표와 실행 시작
- [checklists/boilerplate-intake.md](checklists/boilerplate-intake.md): Playbook·Web Boilerplate 반입 위치, 사용 여부 판정, 첫 지시
- [strategies/unknown-task.md](strategies/unknown-task.md): 새로운 출제 유형을 강제로 분류하지 않는 범용 fallback
- [AGENT_RULES.md](AGENT_RULES.md): Main / Specialist / Reviewer 규칙
- [checklists/180min.md](checklists/180min.md), [checklists/submission.md](checklists/submission.md): 시간·제출 게이트
- [prompts/](prompts): 복사 가능한 한국어 지시문
- [skills/CATALOG.md](skills/CATALOG.md), [mcp/CATALOG.md](mcp/CATALOG.md): 선택·설치 기준
- [checklists/model-recovery.md](checklists/model-recovery.md): Capacity 오류 대응

> **주의:** 사용자 확인에 따라 공개 Clone 허용을 반영했지만, 시험 안내와 운영진의 최신 지침이 언제나 우선한다. 이 자료는 SK하이닉스/Cofa의 공식 출제·채점 문서가 아니다.
