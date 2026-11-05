# 시험 당일 런북 — T+0부터 제출까지

> **대상: 사람. 시험 당일 유일한 진입점이다.** 위에서부터 따른다. 복사용 프롬프트는 이 문서와 `prompts/specialist-*.md`(선택)에만 있다.
>
> 경로 선택: 프롬프트 1(계약) 후 CONTRACT 9절에서 boilerplate 사용 = **경로 A**(T+10 프롬프트 2부터), 사용 안 함 = **[경로 B](#경로-b--boilerplate를-쓰지-않는-과제)**(B-결과물 / B-수리 / B-웹).

**핵심 원칙: 계약 먼저, 기반 코드는 나중.** Codex가 boilerplate를 보기 전에 과제 요구사항을 `CONTRACT.md`로 고정한다. 그다음 boilerplate를 계약에 맞춰 **깎아서** 쓴다. 순서가 반대면 boilerplate의 화면·모델·예시가 요구사항처럼 보이기 시작한다.

**시험 규정: `git commit`·`git push` 금지.** clone만 하고 git 쓰기 명령은 쓰지 않는다. 진행 기록은 `CONTRACT.md` 체크박스와 `DECISIONS.md`가 대신한다. git으로 되돌릴 수 없으므로 큰 삭제·구조 변경 전에는 Codex가 대상 목록을 먼저 보고하게 한다.

## 용어

| 용어 | 뜻 |
|---|---|
| `CONTRACT.md` | 과제 README를 9개 절로 정리한 요구사항 계약서. 프롬프트 1에서 Codex가 채우고 사람이 확정한다. 이후 모든 프롬프트는 이 문서의 절 번호를 가리킨다 |
| AC (Acceptance Criteria, 수용 기준) | CONTRACT 6절의 AC1, AC2… 채점자가 화면이나 명령으로 "됐다/안 됐다"를 확인할 수 있는 문장. 번호가 작을수록 중요하다 |
| 수직 흐름 (vertical slice) | AC 하나를 화면 → API → 서버 로직 → DB/계산 → 결과 화면까지 끝까지 동작하게 만드는 작업 단위. "DB 전부 → API 전부 → 화면 전부"처럼 층별로 쌓지 않는다 |
| placeholder | boilerplate의 예시 코드. `BOILERPLATE-PLACEHOLDER` 주석, `Item` 템플릿, 예시 Job, "작업공간" 화면. 과제 기능이 아니다 |

## CONTRACT.md 절 구성

| 절 | 제목 | 채우는 내용 | 주로 쓰는 단계 |
|---|---|---|---|
| 1 | 한 줄 요약 | 무엇을 만드는가 | 전체 |
| 2 | 필수 산출물 | 제출물·제출 형식 | 제출 |
| 3 | 사용자·역할 | 역할별로 할 수 있는 일 / 없는 일 | 프롬프트 3 (AC1에서 역할·권한 구현) |
| 4 | 데이터·리소스 | 저장할 대상, 출처, 필드, 접근 정책 | 프롬프트 3·4 (AC마다 필요한 모델·시드) |
| 5 | 핵심 계산 | 알고리즘 입출력, hard constraint, 목표, baseline | 프롬프트 3 |
| 6 | 수용 기준 | AC1, AC2… 중요한 순서 | 프롬프트 3·4·5 |
| 7 | 범위 밖 | 하지 않을 것 | 범위 다툼 시 |
| 8 | 모호함 | 질문할 것 / 가정할 것 | 프롬프트 1 검토 |
| 9 | 구현 기반 결정 | boilerplate 사용 여부, Job 필요 여부, 재사용·교체·삭제 매핑 | 프롬프트 2 |

<details><summary>채운 예시 (지난 통근버스 과제를 가정)</summary>

**3절 사용자·역할**
| 역할 | 할 수 있음 | 할 수 없음 |
|---|---|---|
| 관리자 | 정류장·버스 관리, 최적화 실행, 노선 확정 | — |
| 직원 | 내 탑승 정류장 등록, 확정 노선 조회 | 다른 직원 정보 조회, 최적화 실행 |

**4절 데이터·리소스**
| 리소스 | 출처 | 접근 정책 |
|---|---|---|
| 정류장 | 제공 CSV | 공유 조회, 관리자만 변경 |
| 버스(정원) | 제공 CSV | 관리자만 변경 |
| 탑승 신청 | 직원 입력 | 개인 데이터(본인 + 관리자) |
| 노선 결과 | 최적화 계산 | 공유 조회, 관리자만 생성 |

**6절 수용 기준**
- [ ] AC1: 관리자가 최적화를 실행하면 모든 버스가 정원을 넘지 않는 노선이 화면에 표시된다
- [ ] AC2: 직원이 탑승 정류장을 등록하면 본인 신청 목록에 보이고 다른 직원은 볼 수 없다
- [ ] AC3: 직원은 확정된 노선에서 자기 버스와 탑승 시각을 조회한다

이 경우 **AC1 수직 흐름** = 관리자 로그인 → 최적화 실행 버튼 → `withAuth(…, ADMIN)` API → greedy 계산 + 정원 검증 함수 → 결과 DB 저장 → 노선 결과 화면.
</details>

## 최종 구조

```text
<과제 루트>/                  ← Codex를 항상 여기서 시작 (web/ 안에서 시작하지 않는다)
├── README.md, data/ ...      과제 원본 — 읽기 전용
├── AGENTS.md                 playbook 템플릿 복사본 — 유일한 지시 문서
├── CONTRACT.md               T+3~10에 Codex가 작성하고 사람이 확정
├── DECISIONS.md              가정·결정·완료 AC 기록 (서술형 답안의 근거)
└── web/                      boilerplate (사용할 때만)
    ├── BOILERPLATE.md        기반 코드 안내 — 지시가 아니라 참고
    └── ...                   .git·AGENTS.md 없음 (exam-init.sh가 삭제)

~/playbook/                   과제 폴더 밖. 사람이 읽고, Codex에게는 경로로 지정한 파일만 읽힌다
```

꼬임을 막는 세 가지 장치:
1. **`AGENTS.md`는 하나만 둔다.** `web/AGENTS.md`는 `exam-init.sh`가 삭제한다. playbook은 과제 폴더 밖에 둔다.
2. **범위의 기준은 `CONTRACT.md` 하나다.** 모든 구현 프롬프트는 "CONTRACT의 ACn"을 가리킨다.
3. **더미 코드에는 표시가 있다.** `BOILERPLATE-PLACEHOLDER` 표시와 `check-placeholders.sh` 결과가 0건이어야 제출한다.

### exam-init.sh

**언제·누가:** T+3에 **사람이 Terminal 2에서** `git clone … web` 직후 `--install`과 함께 실행한다(Codex가 계약을 쓰는 동안 병렬). 과제 루트에 `AGENTS.md`가 없거나 `web/` 바로 아래가 아니면 ERROR로 멈춘다. 다시 실행해도 정리 단계는 건너뛰고 설치만 다시 한다. 경로 B(boilerplate 미사용)에서는 실행하지 않는다.

`web/scripts/exam-init.sh`가 하는 일은 다음이 전부다. **git init·add·commit은 하지 않는다.**
- `web/.git`, `web/AGENTS.md`, boilerplate 개발 기록(`COMPLETED.md` 등) 삭제
- 시험에 쓰지 않는 파일 삭제: `web/README.md`, `docs/` 중 `FEATURE_TEMPLATE.md`·`ACCESS_POLICY_GUIDE.md` 외, 브라우저 E2E(`tests/`, `playwright.config.ts`)
- `web/.env` 생성, 과제 루트 `DECISIONS.md`에 반입 출처 한 줄 기록(없으면 파일 생성)
- pnpm이 없으면 corepack 또는 `npm i -g pnpm@10.33.0`으로 확보
- `--install`이면 `pnpm install --frozen-lockfile`과 `pnpm bootstrap`(DB 생성 + 시드)까지 실행

---

## 터미널 배치

| 터미널 | 주인 | 용도 |
|---|---|---|
| Terminal 1 | Codex (Main) | 과제 루트에서 시작. 프롬프트 1부터 끝까지 하나의 세션 |
| Terminal 2 | 사람 | 반입·설치·짧은 확인 명령 (T+3 병렬 반입) |
| Terminal 3 | 사람 또는 Codex | 개발 서버 하나 (`cd web && pnpm dev`, 0.0.0.0:3000) |

**서버 규칙:** 서버는 **하나만** 띄운다. 누가 띄워도 되지만 3000번이 이미 응답하면 그 서버를 재사용하고, 다른 프로세스를 kill하지 않는다. 확인: `curl -s -o /dev/null -w '%{http_code}\n' http://127.0.0.1:3000/login` → `200`이면 실행 중.

---

## T+0~3 · 사람: 준비 (Codex 아직 시작하지 않음)

```bash
cd <과제 루트>
ls -la                                             # 과제 원본 파일 목록 확인
git clone --depth 1 https://github.com/nailegg/ai-hackathon-playbook.git ~/playbook
cp ~/playbook/templates/AGENTS.md ./AGENTS.md      # 이미 AGENTS.md가 있으면 아래 참고
cp ~/playbook/templates/CONTRACT.md ./CONTRACT.md
printf '# 결정 기록\n\n' > DECISIONS.md
node -v; command -v pnpm || echo "no pnpm"; python3 -V  # 사용 가능한 런타임 확인
```

- 과제 루트에 `AGENTS.md`가 이미 있으면 덮어쓰지 않는다. `cat ~/playbook/templates/AGENTS.md >> AGENTS.md`로 뒤에 붙인다. 과제 지시가 앞에 오게 하기 위해서다.
- `AGENTS.md`는 세션 시작 시 로드되므로 **복사를 끝낸 뒤 Codex를 시작한다.**
- **제출 방식을 먼저 확인한다:** 폴더 전체 업로드인지, 특정 파일만인지, 용량 제한이 있는지. 폴더 전체 + 용량 제한이면 제출 직전 `web/node_modules`·`web/.next` 처리 방법을 T+160 전에 정해 둔다.
- `~`에 쓸 수 없으면 `/tmp/playbook`을 쓰고, 이후 경로의 `~/playbook`을 그대로 바꿔 읽는다.

## T+3~10 · Codex 프롬프트 1: 계약 추출 (코드 금지) ‖ 사람: 병렬 반입

**Terminal 1 — Codex:**
```text
과제 README.md와 제공 파일을 읽고 CONTRACT.md 템플릿을 채워라. 데이터 파일은 헤더·행 수·샘플 5행·결측만 확인한다.
아직 web/ 디렉터리나 다른 boilerplate는 보지 마라. 코드와 다른 파일은 만들지 마라.
- 모든 항목에 README 근거(절 또는 짧은 인용)를 단다. 근거가 없는 것은 "가정"으로 표시하고 8절(모호함)에 넣는다.
- 6절(수용 기준)은 채점자가 브라우저/명령으로 확인할 수 있는 문장으로, 가장 중요한 사용자 여정부터 5~8개.
- 9절(구현 기반 결정)의 boilerplate 사용은 다음을 모두 만족할 때만 "예": 브라우저로 확인하는 웹 서비스 / 로그인·역할·데이터 저장 중 둘 이상 필요 / 언어·프레임워크·스타터 지정 없음 / 기존 코드 수리 아님 / node 20 이상. 외부 LLM API 제공 여부와 검증 명령도 9절에 적는다.
끝나면 CONTRACT.md 요약과 내가 확인해야 할 모호함(결과를 바꾸는 것만)을 보고해라.
```

**Terminal 2 — 사람 (과제 README를 보고 웹 서비스일 가능성이 있으면 바로):** Codex가 계약을 쓰는 동안 기계적인 반입을 끝내 둔다. Codex 컨텍스트에 설치 로그가 쌓이지 않는다.
```bash
cd <과제 루트>
git clone --depth 1 https://github.com/nailegg/web_boilerplate.git web   # 대상 이름 web 필수 (이중 디렉터리 방지)
bash web/scripts/exam-init.sh --install    # 정리 + DECISIONS 출처 기록 + pnpm 확보 + install + bootstrap. 위치가 틀리면 ERROR
```
Terminal 3에서 `cd <과제 루트>/web && pnpm dev`를 띄우고, 내부 Browser에서 `/login` → `admin@example.com` / `admin12345` → `/admin`을 확인한다. 로그인 후 `/api/auth/me`가 200인지도 본다. **로그인해도 `/login`으로 되돌아오면** `web/.env`에 `COOKIE_SAMESITE="none"`, `COOKIE_SECURE="true"`를 넣고 서버를 재시작한다(내부 Browser가 다른 origin일 때).

- 설치나 Prisma 엔진 다운로드가 **5분 넘게** 실패하면 멈추고 `rm -rf web`. 경로 B로 간다.
- 계약 결과가 "boilerplate 사용 안 함"이면 `rm -rf web` 후 `DECISIONS.md`에서 반입 줄을 지운다.

**사람이 계약을 2~3분 검토할 것:**
- 3절(사용자·역할) 표가 README와 맞는가
- AC1이 정말 가장 중요한 여정인가
- 7절(범위 밖)에 필요한 기능이 들어가 있지 않은가
- 8절(모호함) 중 업무 담당자에게 물어볼 것은 직접 질문하고, 답을 CONTRACT에 반영한다

확정되면 `DECISIONS.md`에 `T+10 CONTRACT 확정`을 한 줄 남긴다. git 커밋은 하지 않는다.

> 웹 서비스가 아니거나 9절 판정이 "아니오"면 **경로 B**로 간다(맨 아래).

## T+10~15 · Codex 프롬프트 2: 매핑과 정리 (경로 A)

반입은 이미 끝났다. Codex는 기반 코드를 계약에 맞춰 **깎기만** 한다.
```text
web/은 이미 반입·설치되어 있고 서버가 3000번에서 실행 중이다(재사용하고 새로 띄우거나 종료하지 마라).
1) web/BOILERPLATE.md만 읽어라. web/docs/는 필요할 때 해당 파일만 읽는다. 이하 경로는 모두 web/ 기준이다.
2) CONTRACT 9절의 "Boilerplate 매핑"을 채워라: 재사용(인증/역할/관리자 화면 등), 교체(dashboard·내비·seed·title), 삭제.
3) Job이 필요 없으면 cd web && node scripts/prune-jobs.mjs && pnpm db:push --accept-data-loss.
4) 역할이 USER/ADMIN과 다르면 바꿀 위치(Role 타입, 관리자 화면 선택지, seed)만 매핑에 적어라. 구현은 AC1에서 한다.
5) cd web && bash scripts/smoke.sh, bash scripts/check-placeholders.sh 결과를 보고하고 DECISIONS.md에 한 줄 남겨라(build는 AC1 이후에 한다).
기능 구현은 아직 하지 마라.
```

**보조 세션 1건 (선택, 사람이 판단):** 과제 유형에 허용된 분할이 하나 있다(최적화 → OR 정식화, RAG → 평가 세트, 분석 → 데이터 품질 등. 표는 [AGENT_RULES.md](AGENT_RULES.md)). 조건에 맞으면 **T+10 계약 확정 직후** Terminal 2에서 두 번째 Codex를 과제 루트에서 시작하고 `~/playbook/prompts/specialist-*.md`의 프롬프트를 붙여 넣는다. 결과 파일 하나(예: `OR_SPEC.md`), 10분, 읽기 전용이다. Main은 그동안 프롬프트 2·3을 진행하고 결과 파일이 생기면 반영한다.
- 최적화 정식화가 명백하면(예: greedy + 정원 검증) 보조 세션 없이 `cp ~/playbook/templates/OR_SPEC.md .` 후 사람이 5분 안에 직접 채운다. 토큰이 들지 않고 서술형 준비도 된다.
- Main Codex가 보조 세션을 직접 띄우게 하지 않는다. 동시에 1개까지만.

## T+15~70 · Codex 프롬프트 3: AC1 수직 흐름 (게이트: T+70)

가장 중요한 수용 기준 AC1 하나를 화면부터 결과까지 실제로 동작하게 만든다. **AC1에 필요한 모델·시드·역할만** 만들고 다른 AC의 리소스는 그 AC에서 추가한다(층별로 먼저 다 만들지 않는다).
```text
CONTRACT 6절의 AC1만 끝까지 구현해라: 로그인 → 화면 입력 → withAuth API → service 검증 → DB 저장/계산 → 결과 화면.
- 데이터: AC1에 필요한 리소스만 web/prisma/schema.prisma에 추가하고 cd web && pnpm db:push. 그 뒤 개발 서버 재시작이 필요하다(내가 띄운 서버면 나에게 요청하고 기다려라). web/docs/FEATURE_TEMPLATE.md는 형식 참고로만 쓰고 Item이라는 이름은 남기지 마라.
- 시드: web/prisma/seed.ts를 계약의 역할별 테스트 계정 + 제공 데이터 파일을 읽어 적재하는 코드로 교체한다(하드코딩 금지, PLACEHOLDER 주석 제거). 관리자 이메일을 바꾸면 web/scripts/smoke.sh 상단 ADMIN_EMAIL도 맞춘다. CSV는 따옴표·쉼표가 들어간 필드를 고려해 파싱한다.
- 역할: 계약 3절과 이름이 다르면 web/lib/auth/require-user.ts의 Role, 관리자 화면 선택지, seed를 함께 바꾼다.
- 접근 정책: CONTRACT 4절에 맞게 web/server/<feature>/service.ts에 구현한다.
- 화면: web/app/(app)/<feature>/에, 역할 제한 화면은 requirePageUser(...). web/app/(app)/dashboard는 AC1 진입 화면으로, 내비게이션은 계약의 화면으로 교체(관련 PLACEHOLDER 주석 제거).
- 계산이 있으면 ~/playbook/strategies/optimization.md와 과제 루트의 OR_SPEC.md(있으면)를 참고해, 몇 초 안에 끝나는 baseline을 API 안에서 동기로 실행하고 CONTRACT 5절 hard constraint를 검사하는 독립 검증 함수를 함께 만든다.
- web/scripts/smoke.sh 하단에 AC1의 성공 1개 + 권한/입력 오류 1개 check를 추가한다.
실행 중인 3000번 서버를 재사용해라. cd web && pnpm bootstrap && pnpm build, smoke 결과, 브라우저 확인 경로를 보고하고, CONTRACT의 AC1 체크와 DECISIONS.md에 "AC1 완료 — 확인 방법, 테스트 계정"을 남겨라.
```

T+70에 AC1이 동작하지 않으면 다른 기능을 모두 멈추고 아래 프롬프트로 복구한다.
```text
AC1 이외 작업을 멈춰라. AC1 흐름에서 실패하는 첫 지점을 재현하고 최소 수정으로 복구해라. 새 구조를 도입하지 마라.
```

## T+70~135 · Codex 프롬프트 4: 나머지 AC 반복

AC마다 같은 프롬프트를 반복한다. **한 번에 하나씩 맡긴다.**
```text
CONTRACT 6절의 다음 미완료 AC 하나(ACn)를 수직 흐름으로 구현해라. 필요한 모델·시드는 이 AC에서 추가한다(스키마를 바꿨으면 db:push 후 서버 재시작을 요청해라). 기존 흐름을 깨지 않게 하고, web/scripts/smoke.sh에 check를 1~2개 추가하고, cd web && pnpm build·smoke 결과를 보고한 뒤 CONTRACT 체크와 DECISIONS 한 줄을 남겨라. 계약 밖 기능은 추가하지 마라.
```

**T+100 체크포인트:**
```text
남은 시간 기준으로 CONTRACT의 AC 완료/미완료/위험을 10줄로 요약하고, T+135까지 끝낼 수 있는 AC만 남겨 순서를 다시 정해라. 버릴 AC는 CONTRACT 7절(범위 밖)로 옮기고 DECISIONS에 이유를 쓴다.
```

알고리즘 개선(`OR_SPEC.md`의 개선안)은 모든 필수 AC가 끝난 뒤에만 한다. 개선 전후 지표를 DECISIONS에 기록한다.

## T+135~150 · Codex 프롬프트 5: 정리·감사

```text
기능 추가를 멈추고 제출 감사를 해라.
1) cd web && bash scripts/check-placeholders.sh 가 0건이 되게 한다: 쓰지 않는 예시는 삭제, 쓰는 것은 과제 이름으로 교체. 이 스크립트는 placeholder 주석, Item 템플릿, "작업공간"·"Web Service Boilerplate"·Jobs 링크 같은 화면 문구를 함께 검사한다.
2) CONTRACT 6절 AC를 하나씩 실제로 실행해 ✅/❌/미검증으로 표시한다.
3) 권한: 비로그인 401, 역할 불일치 403, 다른 사용자 데이터 접근 차단을 smoke로 확인한다. 계산이 있으면 독립 검증 함수로 hard constraint 위반 0건을 확인한다.
4) cd web && pnpm build와 smoke 결과를 붙인다.
최소 수정만 하고 결과를 DECISIONS.md에 남겨라.
```

## T+150~160 · 사람: 서술형 메모 + Codex 프롬프트 6: 제출 문서 (프롬프트 5가 끝난 직후)

사람은 `DECISIONS.md`를 읽으며 서술형용 메모를 **직접** 3줄씩 정리한다. 이 시간에 Codex에게는 제출 문서를 맡긴다.
- 왜 이 구조와 스택인가 (boilerplate를 쓴 이유와 깎아낸 것)
- 핵심 알고리즘·정책과 그 tradeoff
- 검증한 것, 검증하지 못한 것, 남은 한계

```text
과제 루트에 제출용 SUBMISSION.md를 작성해라(과제 README가 다른 파일명·형식을 요구하면 그것을 따른다): 실행 방법(설치·시드·서버), 역할별 테스트 계정, 구현한 AC와 확인 방법, 주요 가정(DECISIONS 요약), 알려진 한계. 실제로 실행해 본 명령만 적어라.
```

## T+160~180 · 제출

`~/playbook/checklists/submission.md`를 따른다. 서버 터미널을 유지한다. git commit·push는 하지 않는다. T+170에 제출 미리보기를 시작한다.

---

## 경로 B — boilerplate를 쓰지 않는 과제

T+0 준비와 프롬프트 1(계약)은 경로 A와 같다. Terminal 2의 병렬 반입은 하지 않는다(이미 했다면 `rm -rf web`). 경로 B로 확정되면 과제 루트 `AGENTS.md`의 디렉터리 지도에서 `web/` 행을 지운다(Codex가 없는 파일을 찾지 않도록). 계약의 9절 "형태"로 세 갈래 중 하나를 고른다. 어느 갈래인지와 Codex에게 줄 전략 문서는 [checklists/task-types.md](checklists/task-types.md) 표를 본다.

| 갈래 | 해당 과제 | 프롬프트 |
|---|---|---|
| **B-결과물** | 결과 파일·CLI·분석 보고·RAG·ETL·최적화 엔진·평가 하네스·예상 밖 | B1 → B2 → B3 → B4 → B5 |
| **B-수리** | 제공된 기존 코드의 실패 테스트·버그 | R1 → R2 → R3 → B5 |
| **B-웹** | 스택·스타터가 지정된 웹, 또는 로그인·저장 없는 단순 웹 화면 | W1 → W2 → W3 → W4 → B5 |

### B-결과물

**T+10~20 · B1: 환경 확인과 입력 로더**
```text
CONTRACT 9절의 형태로 구현을 시작한다. ~/playbook/strategies/<유형>.md 하나만 참고로 읽어라(지시는 AGENTS.md·CONTRACT 우선).
1) 사용할 언어·런타임과 필요한 라이브러리가 실제로 import/실행되는지 1분 안에 확인한다. 설치가 5분 넘게 막히면 표준 라이브러리로 바꾼다.
2) 가장 위험한 기술 가정 하나를 작은 실험으로 확인한다(예: 데이터 크기, 한글 CSV 인코딩(utf-8/cp949), CONTRACT 9절의 LLM API 실제 호출 1회, 솔버 설치). 그래프가 필요하면 화면 없는 환경이므로 MPLBACKEND=Agg로 파일 저장한다.
3) 제공 입력을 읽어 내부 구조로 만드는 로더와, 출력 형식을 검사하는 검증 스크립트의 뼈대를 만든다.
4) 실행·검증 명령을 CONTRACT 9절 "검증 명령"에 적고, 실행 결과를 보고해라. 원본 데이터는 수정하지 마라.
```

**T+20~70 · B2: AC1 수직 흐름 (입력 → 계산 → 출력 → 검증기)**
```text
CONTRACT 6절의 AC1만 끝까지 구현해라: 제공 입력 → 계산/처리 → 과제가 요구한 형식의 출력(파일·CLI 출력·보고서 섹션) → 검증 스크립트 통과.
- 처음에는 단순하지만 항상 유효한 baseline으로 만든다(최적화는 greedy, 검색은 키워드/BM25, 분석은 기술통계).
- 과제 루트에 보조 세션 결과 파일(OR_SPEC.md·EVAL_SET.md·DATA_QUALITY.md·REPAIR_MAP.md·RUBRIC.md·RESEARCH.md)이 있으면 읽고 반영한다.
- 검증 스크립트는 계산 코드와 독립적으로 CONTRACT 5절 hard constraint와 출력 스키마를 다시 검사한다.
- 실행은 한 줄 명령으로 재현 가능해야 한다.
검증 명령 결과를 보고하고 CONTRACT의 AC1 체크와 DECISIONS.md에 "AC1 완료 — 재현 명령, 지표"를 남겨라.
```

**T+70~135 · B3: 나머지 AC 반복** (AC마다 반복, 한 번에 하나)
```text
CONTRACT 6절의 다음 미완료 AC 하나(ACn)를 수직 흐름으로 구현해라. 필요한 데이터·모델은 이 AC에서 추가한다. 기존 흐름을 깨지 않게 하고, CONTRACT 9절 검증 명령에 이 AC 확인용 check를 1~2개 추가해 전체 검증 명령 결과를 보고한 뒤 CONTRACT 체크와 DECISIONS 한 줄을 남겨라. 계약 밖 기능은 추가하지 마라.
```
T+100 체크포인트는 경로 A와 같은 프롬프트를 쓴다. 품질 개선(더 나은 알고리즘·검색·모델)은 필수 AC가 끝난 뒤에만 하고, 개선 전후 지표를 DECISIONS에 남긴다.

**T+135~150 · B4: 감사**
```text
기능 추가를 멈추고 제출 감사를 해라.
1) CONTRACT 2절 필수 산출물이 정확한 경로·파일명·형식으로 존재하는지 확인한다.
2) 깨끗한 상태에서 재현 명령을 처음부터 다시 실행해 같은 결과가 나오는지 확인한다.
3) 검증 스크립트로 hard constraint 위반 0건, 출력 스키마 일치를 확인한다. RAG는 근거 없는 답변·잘못된 인용이 없는지 샘플로 확인한다.
4) CONTRACT 6절 AC를 ✅/❌/미검증으로 표시하고 결과를 DECISIONS.md에 남긴다. 최소 수정만 한다.
```

**T+150~160 · B5: 제출 문서** (사람은 같은 시간에 서술형 메모)
```text
과제 루트에 SUBMISSION.md를 작성해라(과제 README가 다른 형식을 요구하면 그것을 따른다): 실행 환경과 재현 명령, 산출물 목록과 위치, 방법(입력 → 처리 → 출력), 결과 지표, 주요 가정(DECISIONS 요약), 한계와 개선 방향. 분석·보고서 과제면 질문 → 데이터 → 방법 → 결과 → 한계 순서로 쓴다. 실제로 실행해 본 명령만 적어라.
```

### B-수리

**T+10~25 · R1: 재현 (코드 수정 금지)**
```text
기존 코드 수리 과제다. 코드를 수정하지 말고 ~/playbook/strategies/codebase-repair.md 를 참고해:
1) README·requirements.txt·pyproject.toml·package.json에서 의존성 설치와 테스트 명령을 찾아 설치하고(예: pip install -r requirements.txt 또는 pip install -e .), 테스트를 실행해 실패 목록을 정리한다.
2) CONTRACT 6절 AC(고칠 실패)마다 재현 명령, 관련 파일·함수, 원인 후보 1~3개를 적는다.
3) 테스트 명령을 CONTRACT 9절 "검증 명령"에 적는다.
새 프로젝트를 만들거나 구조를 바꾸지 마라. git status/diff로 현재 상태만 확인하라.
```

**T+25~135 · R2: AC 하나씩 최소 수정**
```text
과제 루트에 REPAIR_MAP.md가 있으면 먼저 읽어라. CONTRACT 6절의 다음 미해결 AC 하나만 고쳐라: 실패 재현 → 최소 수정 → 해당 테스트 → 전체 테스트 회귀 확인. 수정이 실패하면 git restore로 그 파일을 되돌리고 다른 원인 후보로 간다(commit은 하지 않는다). 결과와 git diff 요약을 보고하고 CONTRACT 체크와 DECISIONS 한 줄을 남겨라.
```

**T+135~150 · R3: 감사**
```text
기능 수정을 멈추고 감사를 해라.
1) 전체 테스트를 한 번 더 실행해 결과를 붙인다.
2) git diff --stat 와 git diff로 의도하지 않은 변경(포맷 일괄 변경, 디버그 출력, 주석 처리된 코드, 테스트 파일 수정)이 없는지 확인한다. 테스트를 고쳐서 통과시킨 경우는 git restore로 되돌리고 원인을 다시 고친다.
3) 과제 저장소에 원래 있던 AGENTS.md에 내용을 덧붙였다면 git restore AGENTS.md로 원복한다(CONTRACT.md·DECISIONS.md는 새 파일이라 그대로 둔다).
4) CONTRACT 6절 AC를 ✅/❌/미검증으로 표시하고 DECISIONS에 남긴다.
```
이후 B5로 제출 문서를 쓴다.

### B-웹

로그인 이식이 필요하면 W1 전에 사람이 Terminal 2에서 참고용 사본을 과제 폴더 **밖**에 받아 둔다(exam-init 실행하지 않음):
```bash
git clone --depth 1 -q https://github.com/nailegg/web_boilerplate.git /tmp/ref_web
```

**T+10~20 · W1: 지정 스택 기동**
```text
CONTRACT 9절의 스택으로 최소 실행 상태를 만든다. ~/playbook/strategies/fullstack.md 하나만 참고로 읽어라.
1) 제공 스타터가 있으면 그 실행 명령으로 서버를 하나 띄운다. 외부 접속을 위해 0.0.0.0에 바인딩한다
   (예: uvicorn app:app --host 0.0.0.0 --port 3000 / streamlit run app.py --server.address 0.0.0.0 --server.port 3000 / flask run --host 0.0.0.0 --port 3000 / vite --host 0.0.0.0 --port 3000).
   3000번이 이미 응답하면 그 서버를 재사용한다. 스타터가 없으면 지정 스택의 최소 프로젝트를 만든다.
2) 로그인이 필요하면 /tmp/ref_web/lib/auth/ 와 /tmp/ref_web/lib/http.ts 만 참고해 지정 스택으로 이식한다(복사가 아니라 이식: 해시된 비밀번호, DB 세션, HttpOnly 쿠키, 서버측 역할 검사, 401/403 구분). 그 외 파일은 보지 마라.
3) 실행·검증 명령(서버 실행, curl로 확인할 엔드포인트, 테스트 명령)을 CONTRACT 9절에 적고 결과를 보고해라.
```

**T+20~70 · W2: AC1 수직 흐름 (게이트: T+70)**
```text
CONTRACT 6절의 AC1만 끝까지 구현해라: (로그인이 필요하면 로그인 →) 화면 입력 → API → 서버 검증 → 저장/계산 → 결과 화면.
- AC1에 필요한 데이터 모델·시드만 만든다. 제공 데이터는 파일을 읽어 적재한다(하드코딩 금지).
- 과제 루트에 보조 세션 결과 파일(OR_SPEC.md·EVAL_SET.md·DATA_QUALITY.md·REPAIR_MAP.md·RUBRIC.md·RESEARCH.md)이 있으면 읽고 반영한다.
- 권한이 있으면 서버에서 검사한다(비로그인 401, 역할 불일치 403). 화면에서 버튼만 숨기지 않는다.
- 계산이 있으면 몇 초 안에 끝나는 baseline과 CONTRACT 5절 hard constraint를 검사하는 독립 검증 함수를 함께 만든다.
- CONTRACT 9절 검증 명령에 AC1 확인용 curl 또는 테스트를 1~2개 추가한다.
실행 중인 서버를 재사용해라. 검증 명령 결과와 브라우저 확인 경로를 보고하고, CONTRACT의 AC1 체크와 DECISIONS.md에 "AC1 완료 — 확인 방법, 테스트 계정"을 남겨라.
```

**T+70~135 · W3: 나머지 AC 반복** (B3와 같은 프롬프트)
```text
CONTRACT 6절의 다음 미완료 AC 하나(ACn)를 수직 흐름으로 구현해라. 필요한 데이터·모델은 이 AC에서 추가한다. 기존 흐름을 깨지 않게 하고, CONTRACT 9절 검증 명령에 이 AC 확인용 check를 1~2개 추가해 전체 검증 명령 결과를 보고한 뒤 CONTRACT 체크와 DECISIONS 한 줄을 남겨라. 계약 밖 기능은 추가하지 마라.
```
T+100 체크포인트는 경로 A와 같은 프롬프트를 쓴다.

**T+135~150 · W4: 감사**
```text
기능 추가를 멈추고 제출 감사를 해라.
1) CONTRACT 6절 AC를 하나씩 실제로 실행해 ✅/❌/미검증으로 표시한다.
2) 권한이 있으면 비로그인 401, 역할 불일치 403, 다른 사용자 데이터 차단을 curl로 확인한다. 계산이 있으면 독립 검증 함수로 hard constraint 위반 0건을 확인한다.
3) 지정 스택의 빌드·테스트 명령과 CONTRACT 9절 검증 명령 결과를 붙인다.
4) 화면에 템플릿·예시 문구가 남지 않았는지 확인한다.
최소 수정만 하고 결과를 DECISIONS.md에 남겨라.
```
이후 B5로 제출 문서를 쓴다(실행 방법·역할별 테스트 계정 포함).

## 흔한 꼬임과 즉시 처방

| 증상 | 처방 프롬프트 |
|---|---|
| Codex가 boilerplate 화면·Item·Job을 과제 기능처럼 확장 | "CONTRACT 6절에 없는 기능이다. 해당 코드를 삭제하고 check-placeholders 결과를 보고해라." |
| Codex가 git commit·push·init을 시도 | "시험 규정상 git 쓰기 명령은 금지다. 실행하지 말고 진행 기록은 CONTRACT 체크와 DECISIONS.md로 남겨라." |
| Codex가 web/docs 전체를 읽으며 토큰 소모 | "BOILERPLATE.md 외의 문서는 내가 지정할 때만 읽는다." |
| 계약과 다른 역할·권한으로 구현 | "CONTRACT 3절(사용자·역할) 표와 현재 withAuth/requirePageUser 호출을 대조해 불일치만 고쳐라." |
| 데이터 값을 코드에 하드코딩 | "seed가 제공 파일을 읽도록 바꾸고, 하드코딩된 값을 grep으로 찾아 제거해라." |
| `EADDRINUSE` / 3000번 사용 중 | "3000번 서버는 이미 실행 중이다. 재사용하고 종료하지 마라." (서버는 하나만) |
| install 중 `pnpm approve-builds` 안내 | 실행하지 않는다. boilerplate의 `package.json`에 허용 목록이 있어 보통 나오지 않는다. 나오면 무시하고 `pnpm bootstrap` 진행 |
| 화면 전체가 500, 로그에 `Cannot find module './NNN.js'` | dev 서버가 쓰는 `.next`가 깨진 것(옛 boilerplate나 다른 폴더에서 build를 같이 돌린 경우). Terminal 3에서 `Ctrl+C` → `rm -rf .next` → `pnpm dev`. 현재 boilerplate의 `pnpm build`는 `.next-build`에 출력하므로 dev 서버와 충돌하지 않는다 |
| 새 모델 API만 500 (`Cannot read properties of undefined (reading 'create')`) | 스키마 변경 후 서버를 재시작하지 않은 것. Terminal 3에서 `Ctrl+C` 후 `pnpm dev` |
| 로그인 후 계속 `/login`으로 돌아옴 | 사람이 `web/.env`에 `COOKIE_SAMESITE="none"`, `COOKIE_SECURE="true"` 추가 후 서버 재시작 |
| 과제 README가 커밋을 명시적으로 요구 | README가 우선한다. 요구된 범위에서만 커밋하고 DECISIONS에 기록. push는 하지 않는다 |
| 세션 재시작 필요 (capacity 등) | "AGENTS.md, CONTRACT.md, DECISIONS.md를 읽고 web/ 아래 최근 수정 파일(ls -lt)을 확인한 뒤 미완료 AC 하나부터 이어서 해라. 완료된 것을 다시 만들지 마라." |
