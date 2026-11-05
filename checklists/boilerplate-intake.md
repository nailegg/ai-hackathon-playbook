# 저장소 반입 절차 — Playbook과 Web Boilerplate

두 저장소는 **도구**이지 과제 지시가 아니다. 과제 README가 항상 우선한다.

| 저장소 | URL | 용도 |
|---|---|---|
| Playbook (이 저장소) | https://github.com/nailegg/ai-hackathon-playbook | 사람이 읽는 운영 절차·프롬프트 |
| Web Boilerplate | https://github.com/nailegg/web_boilerplate | Next.js + Prisma(SQLite) + 세션 인증 + USER/ADMIN + 관리자 화면 |

## 1. Playbook은 작업 폴더 밖에 둔다 (T+0, 1분)

```bash
git clone https://github.com/nailegg/ai-hackathon-playbook.git ~/playbook
```

과제 폴더 안에 clone하지 않는다. Codex가 과제 파일과 혼동하고, 검색·토큰 사용량이 늘어난다. 필요한 문서만 경로로 지정해 읽힌다. 예: "`~/playbook/strategies/optimization.md`를 읽고 따라라".

## 2. Boilerplate 사용 여부 판정 (T+5~10, 과제 README를 읽은 뒤)

**모두 해당할 때만** 사용한다.

- [ ] 과제 산출물이 브라우저에서 확인하는 웹 서비스다
- [ ] 로그인·역할(관리자/사용자 등)·데이터 저장 중 둘 이상이 필요하다
- [ ] 과제가 언어·프레임워크·스타터 코드를 지정하지 않았다
- [ ] 기존 코드 수리 과제가 아니다
- [ ] `node -v`가 20 이상이다

하나라도 아니면 사용하지 않는다. 인증만 필요하면 boilerplate의 `lib/auth/`(scrypt 해싱, DB 세션, HttpOnly 쿠키)와 `withAuth` 패턴을 참고해 과제 스택으로 옮겨 쓴다.

## 3. 반입 이후

반입 명령, 디렉터리 구조, 단계별 Codex 프롬프트는 [EXAM_RUNBOOK.md](../EXAM_RUNBOOK.md)를 따른다. 요점은 다음과 같다.

- 과제 루트에 `templates/AGENTS.md`와 `templates/CONTRACT.md`를 복사하고, **CONTRACT를 확정한 뒤에** boilerplate를 반입한다.
- boilerplate는 과제 루트의 `web/` 하위에 clone하고 `bash web/scripts/exam-init.sh`를 실행한다. 이 스크립트가 `web/AGENTS.md`를 삭제하므로 지시 문서는 루트 `AGENTS.md` 하나만 남는다.
- Codex는 항상 과제 루트에서 시작한다.
- 교체 대상 예시 코드는 `web/BOILERPLATE.md` 표와 `bash web/scripts/check-placeholders.sh`로 관리한다.
- **포기 기준:** `pnpm install` 또는 Prisma 엔진 다운로드가 5분 넘게 실패하면 `web/`을 지우고 런북의 경로 B로 간다.

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
