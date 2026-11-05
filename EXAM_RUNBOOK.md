# 시험 당일 런북 — T+0부터 제출까지

**핵심 원칙: 계약 먼저, 기반 코드는 나중.** Codex가 boilerplate를 보기 전에 과제 요구사항을 `CONTRACT.md`로 고정한다. 그다음 boilerplate를 계약에 맞춰 **깎아서** 쓴다. 순서가 반대면 boilerplate의 화면·모델·예시가 요구사항처럼 보이기 시작한다.

## 최종 구조

```text
<과제 루트>/                  ← Codex를 항상 여기서 시작 (web/ 안에서 시작하지 않는다)
├── README.md, data/ ...      과제 원본 — 읽기 전용
├── AGENTS.md                 playbook 템플릿 복사본 — 유일한 지시 문서
├── CONTRACT.md               T+3~10에 Codex가 작성하고 사람이 확정
├── DECISIONS.md              가정·결정 기록 (서술형 답안의 근거)
└── web/                      boilerplate (사용할 때만)
    ├── BOILERPLATE.md        기반 코드 안내 — 지시가 아니라 참고
    └── ...                   AGENTS.md 없음 (exam-init.sh가 삭제)

~/playbook/                   과제 폴더 밖. 사람이 읽고, Codex에게는 경로로 지정한 파일만 읽힌다
```

꼬임을 막는 세 가지 장치:
1. **`AGENTS.md`는 하나만 둔다.** `web/AGENTS.md`는 `exam-init.sh`가 삭제한다. playbook은 과제 폴더 밖에 둔다.
2. **범위의 기준은 `CONTRACT.md` 하나다.** 모든 구현 프롬프트는 "CONTRACT의 ACn"을 가리킨다.
3. **더미 코드에는 표시가 있다.** `BOILERPLATE-PLACEHOLDER` 표시와 `check-placeholders.sh` 결과가 0건이어야 제출한다.

---

## T+0~3 · 사람: 준비 (Codex 아직 시작하지 않음)

```bash
cd <과제 루트>
ls -la                                             # 과제 원본 파일 목록 확인
git clone https://github.com/nailegg/ai-hackathon-playbook.git ~/playbook
cp ~/playbook/templates/AGENTS.md ./AGENTS.md      # 이미 AGENTS.md가 있으면 아래 참고
cp ~/playbook/templates/CONTRACT.md ./CONTRACT.md
node -v; command -v pnpm || echo "no pnpm"         # boilerplate 가능 여부 사전 확인
```

- 과제 루트에 `AGENTS.md`가 이미 있으면 덮어쓰지 않는다. `cat ~/playbook/templates/AGENTS.md >> AGENTS.md`로 뒤에 붙인다. 과제 지시가 앞에 오게 하기 위해서다.
- `AGENTS.md`는 세션 시작 시 로드되므로 **복사를 끝낸 뒤 Codex를 시작한다.**
- 그동안 사람도 과제 README를 직접 훑는다. 서술형은 AI 없이 쓰므로 직접 읽어 두어야 한다.

## T+3~10 · Codex 프롬프트 1: 계약 추출 (코드 금지)

```text
과제 README.md와 제공 파일을 읽고 CONTRACT.md 템플릿을 채워라. 데이터 파일은 헤더·행 수·샘플 5행·결측만 확인한다.
아직 web/ 디렉터리나 다른 boilerplate는 보지 마라. 코드와 다른 파일은 만들지 마라.
- 모든 항목에 README 근거(절 또는 짧은 인용)를 단다. 근거가 없는 것은 "가정"으로 표시하고 8. 모호함에 넣는다.
- 6. 수용 기준은 채점자가 브라우저/명령으로 확인할 수 있는 문장으로, 가장 중요한 사용자 여정부터 5~8개.
- 9. 구현 기반 결정은 ~/playbook/checklists/boilerplate-intake.md 의 "2. 사용 여부 판정"만 읽고 판단한다.
끝나면 CONTRACT.md 요약과 내가 확인해야 할 모호함(결과를 바꾸는 것만)을 보고해라.
```

**사람이 2~3분 검토할 것:**
- 역할과 권한 표가 README와 맞는가
- AC1이 정말 가장 중요한 여정인가
- "범위 밖"에 필요한 기능이 들어가 있지 않은가
- 모호함 중 업무 담당자에게 물어볼 것은 직접 질문하고, 답을 CONTRACT에 반영한다

확정되면 `git init -q 2>/dev/null; git add AGENTS.md CONTRACT.md && git commit -m "contract"`. 커밋이 사용자 정보 없음으로 실패하면 `git config user.name examinee && git config user.email examinee@localhost`를 먼저 실행한다.

> 웹 서비스가 아니거나 boilerplate 판정이 "아니오"면 **경로 B**로 간다(맨 아래).

## T+10~18 · Codex 프롬프트 2: 반입과 정리 (경로 A)

```text
CONTRACT.md 9절 결정대로 web boilerplate를 반입하고 계약에 맞게 정리해라. 기능 구현은 아직 하지 마라.
1) git clone https://github.com/nailegg/web_boilerplate.git web && bash web/scripts/exam-init.sh
2) cd web && pnpm install --frozen-lockfile && pnpm bootstrap  (pnpm이 없으면 exam-init이 출력한 명령. 5분 넘게 실패하면 중단하고 보고)
3) web/BOILERPLATE.md만 읽어라. web/docs/는 필요할 때 해당 파일만 읽는다.
4) CONTRACT 9절 "Boilerplate 매핑"을 채워라: 재사용(인증/역할/관리자 화면 등), 교체(dashboard·내비·seed·title), 삭제(Job 불필요 시 node scripts/prune-jobs.mjs 후 pnpm db:push --accept-data-loss).
5) 역할이 USER/ADMIN과 다르면 이름과 의미를 계약에 맞게 바꾸는 계획만 적는다(구현은 프롬프트 3).
6) pnpm build, 서버를 띄워 bash scripts/smoke.sh, bash scripts/check-placeholders.sh 결과를 보고하고 commit "setup".
```

같은 시간에 사람은 별도 터미널에서 서버를 띄운다(`cd web && pnpm dev`). 내부 Browser에서 `/login` → 관리자 로그인 → `/admin`을 확인한다.

**OR·알고리즘 과제라면 병렬로 (Terminal 2, 읽기 전용):**
```text
~/playbook/prompts/specialist-or.md 의 지시를 따른다. 입력은 과제 README.md, CONTRACT.md 5절, 데이터 샘플. 결과를 과제 루트의 OR_SPEC.md 한 파일로만 쓰고 다른 파일은 수정하지 마라.
```

## T+18~40 · Codex 프롬프트 3: 데이터 모델·시드·역할

```text
CONTRACT 3·4절을 구현 기반으로 옮겨라. web/docs/FEATURE_TEMPLATE.md를 형식 참고로만 쓰고 Item이라는 이름은 남기지 마라.
- prisma/schema.prisma에 계약의 리소스와 관계를 추가하고 pnpm db:push.
- prisma/seed.ts를 계약의 역할별 테스트 계정 + 제공 데이터 파일을 읽어 적재하는 코드로 교체한다(하드코딩 금지). 교체 후 seed의 PLACEHOLDER 주석을 지운다.
- 역할 이름이 다르면 lib/auth/require-user.ts의 Role, 관리자 화면 선택지, seed를 함께 바꾼다.
- 각 리소스의 server/<feature>/service.ts에 CONTRACT 4절 접근 정책을 구현한다(아직 화면 없음).
pnpm bootstrap, pnpm build 결과와 적재된 행 수를 보고하고, 계정 정보를 DECISIONS.md에 기록한 뒤 commit "model".
```

## T+40~70 · Codex 프롬프트 4: AC1 수직 흐름 (게이트: T+70)

```text
CONTRACT의 AC1만 끝까지 구현해라: 로그인 → 화면 입력 → withAuth API → service 검증 → DB 저장/계산 → 결과 화면.
- 화면은 web/app/(app)/<feature>/에, 역할 제한 화면은 requirePageUser(...)를 쓴다.
- app/(app)/dashboard는 AC1의 진입 화면으로 교체하고, 내비게이션은 계약의 화면으로 바꾼다(관련 PLACEHOLDER 주석 제거).
- 계산이 있으면 몇 초 안에 끝나는 baseline을 API 안에서 동기로 실행하고, CONTRACT 5절 hard constraint를 검사하는 독립 검증 함수를 함께 만든다.
- scripts/smoke.sh 하단에 AC1의 성공 1개 + 권한/입력 오류 1개 check를 추가한다.
pnpm build, smoke 결과, 브라우저 확인 경로를 보고하고 CONTRACT의 AC1 체크 후 commit.
```

T+70에 AC1이 동작하지 않으면 다른 기능을 모두 멈추고 아래 프롬프트로 복구한다.
```text
AC1 이외 작업을 멈춰라. AC1 흐름에서 실패하는 첫 지점을 재현하고 최소 수정으로 복구해라. 새 구조를 도입하지 마라.
```

## T+70~135 · Codex 프롬프트 5: 나머지 AC 반복

AC마다 같은 프롬프트를 반복한다. **한 번에 하나씩 맡긴다.**
```text
CONTRACT의 다음 미완료 AC 하나(ACn)를 구현해라. 기존 흐름을 깨지 않게 하고, smoke check를 1~2개 추가하고, pnpm build·smoke 결과를 보고한 뒤 CONTRACT 체크와 commit. 계약 밖 기능은 추가하지 마라.
```

**T+100 체크포인트:**
```text
남은 시간 기준으로 CONTRACT의 AC 완료/미완료/위험을 10줄로 요약하고, T+135까지 끝낼 수 있는 AC만 남겨 순서를 다시 정해라. 버릴 AC는 CONTRACT 7절로 옮기고 DECISIONS에 이유를 쓴다.
```

알고리즘 개선(OR_SPEC.md의 개선안)은 모든 필수 AC가 끝난 뒤에만 한다. 개선 전후 지표를 DECISIONS에 기록한다.

## T+135~155 · Codex 프롬프트 6: 정리·감사

```text
기능 추가를 멈추고 제출 감사를 해라.
1) cd web && bash scripts/check-placeholders.sh 가 0건이 되게 한다: 쓰지 않는 예시는 삭제, 쓰는 것은 과제 이름으로 교체. Jobs·Item·"작업공간"·"Web Service Boilerplate" 같은 boilerplate 문구가 화면에 남지 않게 한다.
2) CONTRACT 6절 AC를 하나씩 실제로 실행해 ✅/❌/미검증으로 표시한다.
3) 권한: 비로그인 401, 역할 불일치 403, 다른 사용자 데이터 접근 차단을 smoke로 확인한다. 계산이 있으면 독립 검증 함수로 hard constraint 위반 0건을 확인한다.
4) pnpm build, smoke, (가능하면) pnpm test:e2e 결과를 붙인다.
최소 수정만 하고 commit "audit".
```

## T+150~160 · 사람: 서술형 메모 + Codex 프롬프트 7: 제출 문서

사람은 `DECISIONS.md`를 읽으며 서술형용 메모를 **직접** 3줄씩 정리한다. 이 시간에 Codex에게는 제출 문서를 맡긴다.
- 왜 이 구조와 스택인가 (boilerplate를 쓴 이유와 깎아낸 것)
- 핵심 알고리즘·정책과 그 tradeoff
- 검증한 것, 검증하지 못한 것, 남은 한계

```text
과제 루트에 제출용 SUBMISSION.md를 작성해라(과제 README가 다른 파일명·형식을 요구하면 그것을 따른다): 실행 방법(설치·시드·서버), 역할별 테스트 계정, 구현한 AC와 확인 방법, 주요 가정(DECISIONS 요약), 알려진 한계. 실제로 실행해 본 명령만 적어라.
```

## T+160~180 · 제출

`~/playbook/checklists/submission.md`를 따른다. 서버 터미널을 유지하고, 마지막으로 commit한다(push 금지). T+170에 제출 미리보기를 시작한다.

---

## 경로 B — boilerplate를 쓰지 않는 과제

프롬프트 1은 같다. 이후 단계는 다음과 같다.
- **결과 파일·CLI·분석:** `~/playbook/strategies/unknown-task.md` 또는 해당 strategy 하나만 읽힌다. 프롬프트 4의 "수직 흐름"을 "입력 → 계산 → 출력 파일 → 검증기"로 바꿔 쓴다.
- **기존 코드 수리:** `~/playbook/strategies/codebase-repair.md`. 새 프로젝트를 만들지 않는다.
- **웹이지만 스택이 지정됨:** 지정 스택으로 만든다. 인증이 필요하면 boilerplate를 과제 폴더 밖에 clone해 `lib/auth/`와 `withAuth` 패턴만 참고하도록 경로를 지정해 준다.
  ```text
  인증 구현 패턴은 ~/ref/web_boilerplate/lib/auth/ 와 lib/http.ts 만 참고해라(복사가 아니라 과제 스택으로 이식). 그 외 파일은 보지 마라.
  ```

## 흔한 꼬임과 즉시 처방

| 증상 | 처방 프롬프트 |
|---|---|
| Codex가 boilerplate 화면·Item·Job을 과제 기능처럼 확장 | "CONTRACT 6절에 없는 기능이다. 되돌리고 check-placeholders 결과를 보고해라." |
| Codex가 web/docs 전체를 읽으며 토큰 소모 | "BOILERPLATE.md 외의 문서는 내가 지정할 때만 읽는다." |
| 계약과 다른 역할·권한으로 구현 | "CONTRACT 3절 표와 현재 withAuth/requirePageUser 호출을 대조해 불일치만 고쳐라." |
| 데이터 값을 코드에 하드코딩 | "seed가 제공 파일을 읽도록 바꾸고, 하드코딩된 값을 grep으로 찾아 제거해라." |
| 세션 재시작 필요 (capacity 등) | "AGENTS.md, CONTRACT.md, DECISIONS.md, git log -10을 읽고 미완료 AC 하나부터 이어서 해라. 완료된 것을 다시 만들지 마라." |
