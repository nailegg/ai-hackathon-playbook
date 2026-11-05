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

## 3. 반입 (약 3분)

```bash
cd <과제 작업 폴더>
git clone https://github.com/nailegg/web_boilerplate.git web
cd web
bash scripts/exam-init.sh          # 개발 이력 제거, 출처 명시 baseline 커밋, .env·DECISIONS.md 생성
pnpm install --frozen-lockfile     # pnpm이 없으면 exam-init.sh가 출력한 명령 사용
pnpm bootstrap && pnpm dev         # 0.0.0.0:3000
```

- 과제 폴더 **루트가 아니라 `web/` 하위**에 둔다. boilerplate의 `README.md`·`AGENTS.md`가 과제 README와 섞이지 않게 하기 위해서다. Codex는 `web/` 안에서 작업할 때만 `web/AGENTS.md`(시험 모드 규칙)를 읽는다.
- `exam-init.sh`는 boilerplate의 git 이력을 지운다. git log 평가에 과거 개발 이력이 섞이지 않고, 첫 커밋에 출처가 남는다.
- **포기 기준:** `pnpm install` 또는 Prisma 엔진 다운로드가 5분 넘게 실패하면 `web/`을 지우고 과제가 허용하는 가장 단순한 스택으로 전환한다.

## 4. 반입 직후 확인 (2분)

```bash
bash scripts/smoke.sh               # 401/201/200/403/400/200/200 모두 PASS
```

내부 Browser에서 `/login` → `admin@example.com` / `admin12345` 로그인 → `/admin` 화면을 확인한다.

## 5. Codex에게 줄 첫 구현 지시

```text
과제 README를 기준으로 web/ 안에서 구현한다. web/AGENTS.md와 web/docs/FEATURE_TEMPLATE.md만 먼저 읽어라(다른 docs는 필요할 때만).
과제의 핵심 리소스 하나를 FEATURE_TEMPLATE을 복사해 만들고, 과제가 요구하는 역할별 접근 정책을 service.ts와 withAuth의 role로 구현해라.
로그인 → 핵심 입력 → 서버 검증 → 저장/계산 → 결과 화면까지 한 흐름을 끝낸 뒤 pnpm build와 bash scripts/smoke.sh 결과를 보고해라.
가정은 DECISIONS.md에 한 줄씩 기록하고 질문 없이 진행해라. 결과를 바꾸는 모호함만 질문해라.
```

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
