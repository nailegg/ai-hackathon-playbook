# START HERE — 시험 시작 후 10분

> 실제 진행 순서와 프롬프트는 [EXAM_RUNBOOK.md](EXAM_RUNBOOK.md)를 따른다. 이 문서는 유형 판별 참고용이다.

## Step 0 (0~2분): 읽기
이 Playbook은 과제 폴더 **밖**(`~/playbook`)에 clone한다. → [저장소 반입 절차](checklists/boilerplate-intake.md)

시험 프로젝트의 `README.md`를 우선 읽는다. 본 저장소 README와 혼동하지 않는다. `ls`, 파일 개수/형식/헤더를 확인하고 **원본 데이터는 변경하지 않는다**. 전체 대용량 데이터를 컨텍스트에 출력하지 말고 샘플·통계만 확인한다.

## Step 1 (2~7분): 유형 판별
- 기존 소스와 실패 테스트가 존재 → **E Repair**. 새 프로젝트 생성 금지.
- 결과 파일/알고리즘만 요구 → **B OR** 또는 **G ETL**. UI/Auth부터 만들지 않는다.
- 문서 검색·자연어 답변 → **C RAG**, 평가기가 중심 → **F AI Evaluation**.
- 분석 보고·시각화 중심 → **D Data Analysis**.
- 로그인·역할·데이터 저장·화면이 명시 → **A Full-stack**, 알고리즘도 있으면 **A + B**. Web Boilerplate 사용 여부는 [반입 판정 체크리스트](checklists/boilerplate-intake.md#2-boilerplate-사용-여부-판정-t510-과제-readme를-읽은-뒤)로 결정.

혼합 과제에서는 **제출물과 사용자가 직접 확인할 핵심 여정**을 기준으로 전략을 1~2개 조합한다.

- **A–G와 맞지 않음·혼합이 과도함·평가 방식이 낯섦 → [Unknown Task Mode](strategies/unknown-task.md). 억지로 분류하지 않는다.**

## Step 2 (7~10분): 결정
다섯 줄로 확정: (1) 필수 산출물 (2) Hard constraints (3) 모호함·담당자 질문 (4) 최소 동작하는 수직 기능 (5) 검증 방법. 업무 담당자에게 질문 가능한 경우 **정답을 바꾸는 모호함만** 묻는다. 나머지는 가정 명시 후 진행.

## 시작 프롬프트
```text
이 프로젝트의 README.md와 제공 데이터를 먼저 읽어라(이 Playbook의 README가 아닌 실제 과제 README). 180분 안에 제출할 필수 산출물, 데이터 구조, 사용자 역할, hard constraint, 모호한 업무 규칙을 추려라. 과제 유형을 A~G 중 하나/둘로 선택하거나, 맞지 않으면 Unknown Task Mode로 전환하고 가장 작은 검증 가능한 end-to-end 결과, 구현 순서, 필요한 specialist/skill 여부를 간단히 제시해라. 아직 코드는 수정하지 마라. 10분 안에 구현을 시작할 수 있을 정도로만 분석해라.
```

## 진행
- 구현 시작: [prompts/implement-vertical-slice.md](prompts/implement-vertical-slice.md)
- 180분 관리: [checklists/180min.md](checklists/180min.md)
- 모델 장애: [checklists/model-recovery.md](checklists/model-recovery.md)
- 제출: [checklists/submission.md](checklists/submission.md)

**금지할 실수:** 긴 설계 문서 생산, 새 세션 남발, 모든 Skill 설치, 문제와 무관한 Auth/UI 구축, 매 작업 QA, 제출 직전 대규모 리팩터링.
