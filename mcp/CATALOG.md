# MCP 선택 기준: 기본은 **추가 없음**

> **대상: 사람 참고.** 시험 진행 순서와 프롬프트는 [EXAM_RUNBOOK.md](../EXAM_RUNBOOK.md)에만 있다.

MCP는 전문 지시(Skill)나 별도 판단 주체(Subagent)가 아니라 **외부 서비스/데이터 접근 도구**다. 180분 과제에서 준비·인증·네트워크 실패 비용이 크므로 기본적으로 설치하지 않는다.

| 후보 | 필요한 조건 | 기본 대안 |
|---|---|---|
| OpenAI Docs MCP — https://developers.openai.com/resources/docs-mcp | OpenAI SDK/API 사용과 정확한 최신 문서가 필수, 플랫폼 외부 접근 허용 | 제공 SDK 코드/로컬 도움말 |
| Context7 — https://github.com/upstash/context7 | 버전별 라이브러리 API가 막혀 공식 문서가 꼭 필요, 연결·권한 허용 | 패키지 타입/README/로컬 코드 검색 |
| Playwright MCP — https://github.com/microsoft/playwright-mcp | 시험 내부 Browser에 실제 접속 가능한지 검증됨 | API smoke test, 내부 Browser 수동 확인 |
| Filesystem/Git MCP | CLI로 못 하는 특별한 기능이 확인됨 | Codex 기본 셸 명령 |

## 결정 게이트
1. 이 MCP 없이는 필수 요구사항을 충족하기 어려운가?
2. 제공 웹 IDE의 네트워크·감독 정책상 허용되는가?
3. 설치·연결·실패 복구가 5분 안팎으로 끝나는가? (운영상 경험적 한도)
4. 기본 CLI/API로 같은 일을 할 수 없는가?

어느 하나라도 불명확하면 생략. 시험 문제·입력 데이터·작업 결과를 외부 MCP 서버에 전송하지 않는다. 비공개 키를 외부 저장소나 로그에 기록하지 않는다.
