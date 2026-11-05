# Web Boilerplate 사용 판정

> **대상: Codex 참고(프롬프트 1에서 이 파일만 읽는다).** 지시가 아니라 판정 기준이다. 반입 명령과 순서는 사람용 [EXAM_RUNBOOK.md](../EXAM_RUNBOOK.md)에 있다.

Web Boilerplate: https://github.com/nailegg/web_boilerplate — Next.js + Prisma(SQLite) + 세션 인증 + USER/ADMIN + 관리자 화면.

## 사용 여부 판정

**모두 해당할 때만** 사용한다.

- [ ] 과제 산출물이 브라우저에서 확인하는 웹 서비스다
- [ ] 로그인·역할(관리자/사용자 등)·데이터 저장 중 둘 이상이 필요하다
- [ ] 과제가 언어·프레임워크·스타터 코드를 지정하지 않았다
- [ ] 기존 코드 수리 과제가 아니다
- [ ] `node -v`가 20 이상이다

하나라도 아니면 사용하지 않는다. 인증만 필요하면 boilerplate의 `lib/auth/`(scrypt 해싱, DB 세션, HttpOnly 쿠키)와 `withAuth` 패턴을 참고해 과제 스택으로 옮겨 쓴다.

## Boilerplate가 제공하는 것 (다시 만들지 않는다)

- 회원가입·로그인·로그아웃, DB 세션, `USER`/`ADMIN`
- `withAuth(handler, { role })`: 401/403 자동 처리, `HttpError` → JSON 응답
- `requirePageUser("ADMIN")`: 화면 가드, `app/(app)/` 아래는 로그인 검사와 내비게이션이 자동 적용
- `/admin`: 사용자 목록과 역할 변경
- `docs/FEATURE_TEMPLATE.md`: build·E2E를 통과한 CRUD 코드 (model, validation, service, API, 화면, E2E)
- `scripts/smoke.sh`: Chromium 없이 curl로 권한 검사
- Job/Worker: 수십 초 이상 걸리는 계산에만 사용. 짧은 계산은 API 요청 안에서 동기로 실행한다

## Boilerplate에 없는 것 (필요할 때만 추가)

Zod, Tailwind, 차트, 지도, CSV 업로드, 최적화 솔버. 설치하기 전에 표준 기능으로 충분한지 먼저 판단한다.
