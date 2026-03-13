# 글로벌 Claude Code 설정
# ~/.claude/CLAUDE.md

## 언어 설정
- 모든 응답은 한국어로 작성한다.
- 코드, 변수명, 주석, 커밋 메시지는 영어로 유지한다.
- 기술 용어는 영문 그대로 사용해도 무방하다 (예: "commit", "branch", "deploy").

---

## 작업 철학 (superpowers Iron Laws)

### 설계 우선 (Design First)
- 코드 작성 전 반드시 요구사항을 파악하고 설계를 먼저 제안한다.
- brainstorming → plan → execute 순서를 지킨다.
- 사용자 승인 없이 코드 구현 단계로 넘어가지 않는다.

### TDD (Test-Driven Development)
- 항상 실패하는 테스트를 먼저 작성한다.
- RED → GREEN → REFACTOR 사이클을 준수한다.
- 테스트 없이 구현 코드를 먼저 작성하지 않는다.

### 검증 우선 (Verification First)
- "완료했습니다" 주장 전 반드시 fresh 검증 증거를 제시한다.
- 명령 실행 결과, 테스트 통과 로그, 실제 출력을 확인한다.
- 추측으로 완료를 선언하지 않는다.

### 근본 원인 파악 (Root Cause Analysis)
- 버그 수정 전 반드시 근본 원인을 파악한다.
- 증상만 고치지 않는다.
- 디버깅: 조사 → 가설 → 검증 → 수정 순서를 따른다.

---

## 기술 스택 선호도

### 프론트엔드
- React 18+ (함수형 컴포넌트, Hooks 기반)
- Vue 3 (Composition API)
- Next.js 14+ (App Router)
- TypeScript 우선

### 백엔드
- Spring Boot (Java/Kotlin)
- Node.js / Express
- Python (FastAPI, Django)

### 인프라 & 도구
- Docker / Docker Compose
- GitHub Actions (CI/CD)
- MCP (Model Context Protocol) 생태계 적극 활용

### 개발 방법론
- 초기 개발: Waterfall (단계별 산출물, 체계적 문서화)
- 운영/유지보수: Kanban (SLA 기반, 인시던트 대응)

---

## 코드 스타일

- 과도한 엔지니어링 금지: 현재 요구사항에 최소한의 복잡성
- 불필요한 추상화, 헬퍼, 유틸리티 생성 금지
- 에러 핸들링은 시스템 경계(사용자 입력, 외부 API)에서만
- 문서화되지 않은 변경은 하지 않는다

---

## Superpowers 스킬 라이브러리

`/home/user/superpowers/skills/` 에 다음 스킬이 있으며 필요시 활용:

| 스킬 | 용도 |
|------|------|
| brainstorming | 설계 탐색, 대안 제안 |
| writing-plans | 구현 계획 작성 (2-5분 단위 태스크) |
| executing-plans | 계획 실행, 배치 리뷰 |
| test-driven-development | TDD 사이클 |
| systematic-debugging | 근본 원인 분석 |
| requesting-code-review | 코드 리뷰 요청 |
| verification-before-completion | 완료 전 검증 |
| finishing-a-development-branch | 브랜치 마무리 (merge/PR) |

---

## 리서치 컨텍스트

현재 진행 중인 연구 분야 (`/home/user/research-notes`):
- AI 자동화 및 부의 파이프라인 투자 전략 (2026)
- 멀티에이전트 오케스트레이션 패턴
- 바이브 코딩(vibe-coding) 생산성 최적화

---

## 응답 형식 선호도

- 간결하게: 필요한 내용만, 장황한 설명 금지
- 코드는 완전한 버전으로: 생략(`...`) 없이 전체 코드 제공
- 파일 경로:줄번호 형식으로 코드 위치 참조
- 목록 대신 실행 가능한 단계로 작성
