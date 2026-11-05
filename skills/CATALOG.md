# 공개 Skill 카탈로그 — 필요한 경우에만

> **대상: 사람 참고.** 시험 진행 순서와 프롬프트는 [EXAM_RUNBOOK.md](../EXAM_RUNBOOK.md)에만 있다.

**원칙:** 이 저장소의 `exam-or-triage`, `exam-rag-triage`는 **직접 제작한 경량 제어 Skill**이다. 공개 전문 Skill은 부족한 분야에서만 *선별* 사용한다. 외부 저장소의 라이선스·보안·필요 도구를 확인하고, 검증하지 않은 도구가 작동한다고 가정하지 않는다.

| 후보 | 용도 | 언제? | 링크 |
|---|---|---|---|
| Operations Research Skill | LP/MIP/스케줄·솔버 선택 | 목적·제약 모델이 난해할 때 | https://github.com/ShayanDarabi/operations-research-skill |
| Combinatorial Optimization Skills | problem formulation, multiobjective, validation, scheduling | 문제 유형이 명확하고 전문 패턴 필요 | https://github.com/hajibabaie/combinatorial-optimization-skills |
| OR-Tools routing skill 예제 | 차량 경로·시간창·용량 | VRP가 핵심일 때 | https://github.com/benchflow-ai/skillsbench/tree/main/tasks/paratransit-routing/environment/skills/ortools-routing-modeling |
| Anthropic Public Skills | 참고용 범용 Skill 작성 예시 | 해당 Skill의 목적·의존성이 적합할 때 | https://github.com/anthropics/skills |

**검증 상태:** 후보 공개 저장소 링크를 카탈로그로 수록한 것이며, 이 시험 환경에서 설치·Codex 호출·성능이 검증되었다는 의미는 아니다. 특정 하위 디렉터리 경로가 변경되었으면 저장소 루트에서 찾는다.

## 실제 선택 규칙
- 0~15분: 설치 탐색보다 문제 정식화 우선.
- 1개 경량 Skill로 충분하면 외부 Skill을 추가하지 않는다.
- 외부 Skill 설치/의존성이 막히면 빠르게 Main 직접 구현으로 전환.
- 프로젝트에 직접 적용하려면 해당 Skill의 `SKILL.md`를 포함한 디렉터리를 `.agents/skills/<name>/`에 배치하고 `/skills` 등 제공 Codex 버전의 탐색 기능으로 확인한다. 이미 로드된 Codex 세션은 재시작이 필요할 수 있다.
- 저장소 전체를 무조건 Clone해도 모든 Skill이 자동 등록되는 것은 아니다.
