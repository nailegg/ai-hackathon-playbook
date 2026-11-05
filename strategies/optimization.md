# 전략 B — Operations Research / 최적화

> **대상: Codex 참고 — 지시가 아니다.** 지시는 과제 루트의 `AGENTS.md`와 `CONTRACT.md`가 우선한다. 런북 프롬프트가 이 파일 경로를 지정할 때만 읽는다.

**적용:** 배차·스케줄·자원 할당·경로·생산 계획. 웹 화면이 필요하면 화면·API 구성은 런북 프롬프트(경로 A 또는 B-웹)를 따르고, 이 문서는 계산 부분에만 쓴다.

## 작업 방식
- 기본은 한 Codex 세션이 처음부터 검증까지 맡는다.
- **허용되는 분할 1건 — OR 정식화:** 목적함수가 모호하거나 hard constraint가 3개 이상이라 정식화가 구현을 막을 때. 사람이 Terminal 2에서 `~/playbook/prompts/specialist-or.md`로 두 번째 세션을 띄운다. 읽기 전용이고 결과는 과제 루트 `OR_SPEC.md` 한 파일, 10분 안에 끝낸다. Main은 그동안 기계적인 작업을 진행하고, 끝나면 그 파일을 읽어 반영한다.
- 그 밖의 역할 분할(Frontend/Backend/QA 등), 구현 분할, Codex가 스스로 에이전트를 띄우는 것은 하지 않는다.

## 6단계
1. 입력/출력과 규모, 단위, 시간창·용량 확인. 누락·불가능 사례 식별.
2. **Hard constraints**(반드시 충족)와 **Soft objectives**(최소화/최대화) 분리.
3. 목적함수에 측정 가능한 지표와 기준선. 단위가 다른 목표는 정규화/계층적 최적화 고려.
4. 항상 **유효한 baseline**부터 구현: Greedy, earliest deadline, nearest-first 등.
5. 제약이 복잡하고 설치·시간이 허락되면 CP-SAT/MILP/OR-Tools; 아니면 Local Search/Swap/2-opt. 솔버 timeout 설정.
6. **독립 검증 함수**가 해의 feasibility와 점수를 재계산. Baseline과 개선결과 수치 비교, infeasible 이유 기록.

## 정식화 기록
정식화는 과제 루트 `OR_SPEC.md`(형식: `~/playbook/templates/OR_SPEC.md`)에 남긴다. 이미 있으면 그 파일을 따른다.

## 경고
솔버 설치/모델링이 길어지면 Greedy로 복귀. 멋진 점수만 보이고 불가능한 배정을 내면 실패. 결과 설명 가능한 지표를 UI/리포트에 포함.

