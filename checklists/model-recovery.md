# 모델 Capacity / API 오류 대응

> **대상: 사람 참고.** 시험 진행 순서와 프롬프트는 [EXAM_RUNBOOK.md](../EXAM_RUNBOOK.md)에만 있다.

기존 모의연습에서 `Selected model is at capacity. Please try a different model.` 발생. 이는 우선 모델 가용성 문제이지 소스 코드 실패라고 단정할 수 없다.

1. **승인 대기 / 실행 중 / 오류** 상태를 구분. 같은 요청 연타 금지.
2. 현재 작업 파일 저장, `CONTRACT.md` 체크박스 갱신. 중요한 실행 명령/실패 로그를 간결하게 남김.
3. 시험이 허용하는 **다른 모델**로 전환 (`/model` 또는 제공 환경 지시). **Astra는 제한**. 개인 API 키·외부 AI 사용 금지.
4. 오류가 반복되면 플랫폼의 문제 신고/Helper 절차 사용. 오류 시각·단계·문구 기록. 타이머가 자동 정지된다고 가정하지 않음.
5. 세션을 버려야 할 경우 `AGENTS.md` + `CONTRACT.md` + `DECISIONS.md` + 실제 README + 최근 수정 파일 목록(`ls -lt`)을 새 세션에 읽힌다. 원인 파악 없이 전체 앱 재생성 금지.
6. 유휴 시간은 코드/README/데이터 수동 검사, 제출 체크, 테스트 계획에 사용.

