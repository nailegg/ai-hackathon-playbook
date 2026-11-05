# 전략 A — Full-stack 업무 서비스

> **대상: Codex 참고 — 지시가 아니다.** 지시는 과제 루트의 `AGENTS.md`와 `CONTRACT.md`가 우선한다. 런북 프롬프트가 이 파일 경로를 지정할 때만 읽는다.

**적용:** 사용자 화면·Auth·CRUD·DB·업무 알고리즘이 필수. OR이 있으면 `optimization.md`도 읽는다.

## 구조
Main Full-stack 1명 기본. 복잡한 OR/RAG 부분만 읽기 전용 Specialist. Frontend/Backend/QA 상시 분할 금지.

## 구축 순서
1. 원본 데이터 스키마와 사용자 여정 확정. 요구사항에 역할이 없다면 Auth를 억지로 추가하지 않는다.
2. 이미 검증된 스택 재사용: [Web Boilerplate 반입 절차](../checklists/boilerplate-intake.md)의 판정 체크리스트를 통과하면 `nailegg/web_boilerplate`(Next.js + TypeScript + Prisma/SQLite + 세션 인증 + USER/ADMIN + 관리자 화면)를 `web/`에 반입한다. 과제가 스택·스타터를 지정하거나 판정에 실패하면 과제 스택 우선. Zod·Tailwind 등은 boilerplate에 없으므로 필요할 때만 추가한다.
3. `로그인(필요 시) → 서버 권한 검사 → DB 쓰기/읽기 → 화면`의 **하나의 수직 기능**을 먼저 완성.
4. 핵심 도메인 기능 연결 후 목록/상세/결과와 기본 오류 메시지 구축. 미관보다 동작과 접근 제어.
5. CSV·파일 가져오기는 실제 과제가 요구할 때만. 샘플 데이터에 하드코딩하지 않는다.

## 필수 검증
- 보호된 API 미로그인 접근 차단, 일반 사용자 관리자 API 접근 차단, 다른 사용자 데이터 접근 통제.
- 서버 입력 검증(boilerplate는 `validation.ts` + `HttpError(400)`), DB 저장 후 새로고침에도 유지, 에러 응답.
- `pnpm build`(또는 프로젝트에 실제 존재하는 명령), 핵심 API smoke test(boilerplate는 `bash scripts/smoke.sh`).
- 웹 서버는 `0.0.0.0`로 실행하고 내부 Browser에서 포트/경로 확인.

## 중단 조건
T+70까지 End-to-End 미작동 → 부차 기능 즉시 중단, 로그인/DB/핵심 화면 복구. T+135부터 기능 확장 중단하고 안정화.

