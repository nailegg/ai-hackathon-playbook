# 전략 B — Operations Research / 최적화

**적용:** 배차·스케줄·자원 할당·경로·생산 계획. 웹 필수면 `fullstack.md`와 함께 사용.

## 에이전트
Main Solver 또는 Main Full-stack이 코드를 소유. OR Specialist는 **읽기 전용 수학적 정식화**만. OR Skill: `skills/exam-or-triage/SKILL.md` 먼저, 고난도면 공개 전문 Skill 문서 참고.

## 6단계
1. 입력/출력과 규모, 단위, 시간창·용량 확인. 누락·불가능 사례 식별.
2. **Hard constraints**(반드시 충족)와 **Soft objectives**(최소화/최대화) 분리.
3. 목적함수에 측정 가능한 지표와 기준선. 단위가 다른 목표는 정규화/계층적 최적화 고려.
4. 항상 **유효한 baseline**부터 구현: Greedy, earliest deadline, nearest-first 등.
5. 제약이 복잡하고 설치·시간이 허락되면 CP-SAT/MILP/OR-Tools; 아니면 Local Search/Swap/2-opt. 솔버 timeout 설정.
6. **독립 검증 함수**가 해의 feasibility와 점수를 재계산. Baseline과 개선결과 수치 비교, infeasible 이유 기록.

## 출력 계약
`assumptions`, `hard_constraints`, `metrics`, `baseline`, `candidate_algorithm`, `runtime_limit`, `fallback`, `validator_cases`.

## 경고
솔버 설치/모델링이 길어지면 Greedy로 복귀. 멋진 점수만 보이고 불가능한 배정을 내면 실패. 결과 설명 가능한 지표를 UI/리포트에 포함.

## 호출
```text
이 문제의 OR 정식화만 맡아라. 데이터를 읽고 hard constraints, soft objectives, 목적함수의 단위/정규화, 20~30분 안에 구현 가능한 baseline, 개선 방법 하나, 독립 검증 사례를 한 페이지로 작성해라. 코드 수정은 하지 마라. 설치가 어려운 솔버의 대체 휴리스틱을 명시해라.
```
