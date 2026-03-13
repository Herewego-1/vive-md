# Codex 글로벌 설정
# ~/.codex/AGENTS.md
#
# 소스: superpowers(⭐81K) + vive-md + research-notes + everything-claude-code(⭐74K)
#        + claude-skills(⭐6.5K) + plugins-for-claude-natives + fireauto
# 업데이트: 2026-03-13

---

## 언어 & 커뮤니케이션

- **모든 응답은 한국어**로 작성한다.
- 코드, 변수명, 주석, 커밋 메시지는 **영어**로 유지한다.
- 기술 용어는 영문 그대로 사용한다 (commit, branch, deploy, skill, hook 등).
- 응답은 간결하게 — 필요한 내용만, 장황한 설명 금지.
- 코드는 **생략(`...`) 없이 완전한 버전** 제공.
- 파일 경로:줄번호 형식으로 코드 위치 참조.

---

## 프로젝트 시작 체크리스트 (common-ground 패턴)

새 프로젝트 또는 기존 코드베이스에 처음 접근할 때:

1. 파일 구조, 문서, 최근 커밋 탐색
2. 기술 스택 & 의존성 확인
3. 테스트 명령어 확인 (`package.json`, `Makefile`, `pyproject.toml` 등)
4. 현재 브랜치 & git 상태 확인
5. **가정(assumption)을 명시적으로 확인**받고 구현 시작

> **Anti-pattern**: 프로젝트 컨텍스트 없이 바로 코드 작성 금지.

---

## 개발 워크플로우 Iron Laws

### Law 1: 설계 우선 (Design First)

```
코드 작성 전 반드시 설계를 제안하고 사용자 승인을 받는다.
```

- brainstorming → plan → execute 순서 준수
- 아무리 간단한 작업도 설계 승인 없이 구현 단계로 진행 금지
- 설계 문서 저장 위치: `docs/plans/YYYY-MM-DD-<topic>-design.md`

### Law 2: TDD (Test-Driven Development)

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

- RED → GREEN → REFACTOR 사이클 준수
- 테스트 없이 구현 코드를 먼저 작성하면 → 삭제 후 재시작
- 예외: 일회성 프로토타입, 설정 파일 (반드시 사전 확인)

### Law 3: 검증 우선 (Verification First)

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

- "완료했습니다" 주장 전 반드시 실제 실행 결과 제시
- 추측으로 완료 선언 금지
- 검증 방법: 테스트 통과 로그, 실제 출력, 커맨드 실행 결과

### Law 4: 근본 원인 파악 (Root Cause Analysis)

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

- 버그 수정 전 4단계 프로세스:
  1. 에러 메시지 완전히 읽기
  2. 일관되게 재현
  3. 원인 범위 격리
  4. 수정 전 가설 검증
- 증상만 고치는 패치 금지

### Law 5: 최소 복잡도 (Minimal Complexity)

- 과도한 엔지니어링 금지 — 현재 요구사항에 필요한 최소한만
- 불필요한 추상화, 헬퍼, 유틸리티 생성 금지
- 에러 핸들링은 시스템 경계(사용자 입력, 외부 API)에서만

### Law 6: 서브에이전트 활용 (Subagent-Driven Development)

- 독립적인 태스크는 서브에이전트로 분리 실행
- 각 서브에이전트에게 fresh context 제공
- 태스크 완료 후 2단계 리뷰: spec 준수 → 코드 품질

---

## 설계 → 계획 → TDD → 검증 프로세스

```
[아이디어 수신]
      ↓
[brainstorming] → 2-3개 접근법 제안 + 트레이드오프
      ↓ (사용자 승인)
[writing-plans] → bite-sized 태스크 (2-5분 단위)
      ↓
[TDD 구현]     → RED(테스트 작성) → GREEN(최소 구현) → REFACTOR
      ↓
[code review]  → spec 준수 확인 → 코드 품질 확인
      ↓
[verification] → 실행 결과 증거 제시
      ↓
[commit & push]
```

### 계획 문서 헤더 템플릿

```markdown
# [Feature Name] Implementation Plan

**Goal:** [한 문장 목표]
**Architecture:** [2-3문장 접근법]
**Tech Stack:** [핵심 기술/라이브러리]

---
```

---

## 기술 스택 & 선호도

### 프론트엔드
- **React 18+** — 함수형 컴포넌트, Hooks, TypeScript 우선
- **Vue 3** — Composition API, `<script setup>`
- **Next.js 14+** — App Router, Server Components
- TypeScript 항상 사용

### 백엔드
- **Spring Boot 3.x** (Java/Kotlin) — REST API, JPA, Security
- **Node.js / Express** — TypeScript, ESM
- **Python** — FastAPI (API), Django (풀스택)

### 인프라 & 도구
- Docker / Docker Compose
- GitHub Actions (CI/CD)
- MCP (Model Context Protocol) 생태계 적극 활용

### 개발 방법론
| 단계 | 방법론 | 적용 시점 |
|------|--------|----------|
| 초기 개발 | **Waterfall** | Phase 0(기획) ~ Phase 6(배포) |
| 운영/유지보수 | **Kanban** | Phase 7 이후, SLA 기반 |

---

## MCP 생태계

### 핵심 원칙
- 프로젝트당 **최대 10개** MCP 서버 (컨텍스트 예산 보호 — ECC 권장)
- MCP 설정 파일: `~/.claude.json` 또는 프로젝트 `.mcp.json`

### 추천 MCP 서버 카테고리

| 카테고리 | 서버 | 용도 |
|----------|------|------|
| **코드** | filesystem, github | 파일 I/O, PR/이슈 관리 |
| **DB** | postgres, sqlite, mysql | 데이터베이스 직접 쿼리 |
| **검색** | brave-search, context7 | 웹 검색, 최신 문서 조회 |
| **생산성** | slack, notion, google-docs | 팀 커뮤니케이션 |
| **모니터링** | sentry, datadog | 에러 추적, 메트릭 |

### Context7 활용
- LLM용 최신 코드 문서 조회 (`upstash/context7` ⭐48K)
- 프레임워크 문서가 학습 데이터 이후 업데이트된 경우 반드시 활용

### MCP 개발 가이드 위치
- TypeScript SDK: `/home/user/vive-md/vibe-coding/mcp/01-MCP-서버-개발-가이드.md`
- 보안/인증: `/home/user/vive-md/vibe-coding/mcp/02-MCP-보안-인증-가이드.md`
- 실전 패턴: `/home/user/vibe-md/vibe-coding/mcp/03-MCP-실전-패턴-모음.md`
- 서버 카탈로그 (2596개+): `/home/user/vive-md/vibe-coding/mcp/Awesome-MCP-Servers-한국어-가이드.md`

---

## 멀티에이전트 & 훅 패턴

### 멀티에이전트 오케스트레이션 (ECC 패턴)

```
[계획 분해] → [병렬 실행] → [결과 합성] → [품질 게이팅]
```

- DAG 오케스트레이션: 독립적인 태스크는 병렬로
- 체크포인트/평가 루프: 중요 경로에 검증 단계 삽입
- Approval gate: 범위 검증 후 실행

### 동적 팀 어셈블리 패턴 (plugins-for-claude-natives)

작업에 따라 필요한 전문가 에이전트를 동적으로 조합:
- `clarify` — 요구사항 명확화 (구현 전 모호한 부분 제거)
- `doubt` — 응답 검증 (`!rv` 트리거로 팩트체크)
- `team-assemble` — 복잡한 태스크에 전문가 에이전트 팀 구성

### 훅 런타임 제어 (ECC 패턴)

환경변수로 파일 수정 없이 훅 동작 조정:
```bash
# 선택적 훅만 활성화 (컨텍스트 절약)
export ECC_HOOK_PROFILE=selective
# 특정 훅 비활성화
export ECC_DISABLED_HOOKS=hook1,hook2
```

### 세션 메모리 패턴
- 세션 종료 시 패턴/인사이트를 `instinct` 파일로 추출
- 다음 세션에서 학습된 패턴 재활용

---

## 도메인 자동화 명령어 (fireauto 패턴)

반복적인 작업은 도메인별 명령어로 자동화:

| 명령어 | 용도 | 소스 |
|--------|------|------|
| `/planner` | 아이디어 → PRD 자동 생성 | fireauto |
| `/researcher` | Reddit 분석 + 리드 스코어링 | fireauto |
| `/security-guard` | 8개 카테고리 보안 감사 | fireauto |
| `/seo-manager` | 빌드 없이 SEO 스캔 | fireauto |
| `/designer` | DaisyUI v5 UI 자동 생성 | fireauto |
| `/doubt` | 응답 팩트체크 & 가정 검증 | plugins-for-claude-natives |
| `/common-ground` | 프로젝트 가정 검증 | claude-skills |

---

## 안티패턴 테이블

| 안티패턴 | 올바른 방법 |
|----------|-----------|
| 설계 없이 코드 작성 | brainstorming → 승인 → 구현 |
| 테스트 없이 구현 | 실패 테스트 먼저, RED→GREEN |
| "완료"를 추측으로 선언 | 실행 결과 증거 제시 |
| 증상만 수정 | 근본 원인 파악 후 수정 |
| 10개 초과 MCP 활성화 | 프로젝트당 최대 10개 |
| 서브에이전트에 오염된 컨텍스트 | 태스크당 fresh 서브에이전트 |
| 과도한 추상화/헬퍼 생성 | 현재 요구사항에 최소한만 |
| "이건 너무 단순해서 설계 불필요" | 아무리 간단해도 설계 승인 필수 |

---

## 리서치 컨텍스트

현재 진행 중인 연구 분야 (2026):

1. **AI 자동화와 부의 파이프라인**
   - LLM 기반 자동화가 투자 구조에 미치는 영향
   - 수익 파이프라인 자동화 전략
   - AI 인프라 투자 기회 분석

2. **멀티에이전트 오케스트레이션**
   - Claude Code 서브에이전트 패턴 분석
   - 병렬 에이전트 디스패칭 성능 최적화
   - ChatDev, MetaGPT, HyperAgent 패턴 연구

3. **바이브 코딩(Vibe-Coding) 생산성**
   - LLM 기반 개발 워크플로우 최적화
   - 오케스트레이션 패턴 생산성 측정

관련 자료:
- `/home/user/research-notes/AI_자동화_부의파이프라인_투자_20260308/`
- `/home/user/vive-md/docs/research/`

---

## Superpowers 스킬 설치

```bash
# superpowers 스킬을 Codex에 연동
git clone http://127.0.0.1:38233/git/Herewego-1/superpowers ~/.codex/superpowers
mkdir -p ~/.agents/skills
ln -s ~/.codex/superpowers/skills ~/.agents/skills/superpowers
```

설치 후 사용 가능한 스킬 (14개):

| 스킬 | 트리거 시점 |
|------|-----------|
| `brainstorming` | 기능 구현/컴포넌트 생성 전 |
| `writing-plans` | 스펙/요구사항 확보 후, 코딩 전 |
| `executing-plans` | 구현 계획 단계별 실행 |
| `test-driven-development` | 기능/버그픽스 구현 시 |
| `systematic-debugging` | 버그/테스트 실패/예상 밖 동작 발생 시 |
| `verification-before-completion` | 완료/커밋/PR 직전 |
| `subagent-driven-development` | 독립적 태스크가 있는 계획 실행 시 |
| `requesting-code-review` | 구현 완료 후 리뷰 요청 전 |
| `receiving-code-review` | 코드 리뷰 피드백 수신 후 |
| `finishing-a-development-branch` | 브랜치 마무리 (merge/PR) 시 |
| `dispatching-parallel-agents` | 태스크를 병렬 에이전트에 분배 시 |
| `using-git-worktrees` | 여러 기능 병렬 개발 시 |
| `writing-skills` | 새 스킬 작성 시 |
| `using-superpowers` | superpowers 사용법 안내 필요 시 |

---

## 추천 참고 레포지터리

| 레포 | 별점 | 핵심 가치 |
|------|------|----------|
| `affaan-m/everything-claude-code` | ⭐74K | 65+스킬, 멀티에이전트, AgentShield |
| `obra/superpowers` | ⭐81K | Iron Laws 워크플로우 스킬 |
| `anthropics/skills` | ⭐92K | Anthropic 공식 에이전트 스킬 |
| `Jeffallan/claude-skills` | ⭐6.5K | 66개 풀스택 도메인 스킬 |
| `hesreallyhim/awesome-claude-code` | ⭐27K | 200+ 큐레이션 리소스 |
| `upstash/context7` | ⭐48K | LLM용 최신 코드 문서 |
| `max-sixty/worktrunk` | ⭐3.2K | Git worktree 병렬 개발 CLI |
| `johunsang/vive-md` | ⭐225 | 바이브코딩 가이드 & 템플릿 |
