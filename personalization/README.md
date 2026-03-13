# AI 개인화 설정 모음

superpowers, vive-md, research-notes 레포지터리의 유용한 내용을 종합해
Claude Code, Claude.ai, Perplexity에 적용할 수 있는 개인화 설정 파일들입니다.

---

## 파일 구성

| 파일 | 적용 대상 | 설명 |
|------|-----------|------|
| `global-claude.md` | Claude Code (`~/.claude/CLAUDE.md`) | Claude Code 모든 세션에 자동 적용 |
| `claude-ai-instructions.md` | Claude.ai 웹사이트 | 커스텀 인스트럭션 붙여넣기용 |
| `perplexity-ai-profile.md` | Perplexity.ai | AI 프로필 설정 붙여넣기용 |
| `codex-agents.md` | **Codex CLI** (`~/.codex/AGENTS.md`) | Codex 글로벌 설정 (7개 레포 교차분석) |
| `chatgpt-plus-instructions.md` | **ChatGPT Plus** (Custom Instructions) | 두 개 필드용 압축 버전 |

---

## 빠른 설정 가이드

### 1. Claude Code (로컬 CLI)

```bash
# 자동 설치 스크립트 실행
bash /home/user/superpowers/scripts/install-global-claude.sh

# 또는 수동으로
mkdir -p ~/.claude
cp /home/user/vive-md/personalization/global-claude.md ~/.claude/CLAUDE.md
```

설치 후 모든 Claude Code 세션에서 자동으로 한국어 응답, superpowers 철학,
기술 스택 컨텍스트가 적용됩니다.

### 2. Claude.ai 웹사이트

1. [claude.ai/settings](https://claude.ai/settings) 접속
2. **Custom Instructions** 탭
3. `claude-ai-instructions.md` 파일 내 "붙여넣기용 텍스트" 복사 후 붙여넣기
4. Save

### 5. ChatGPT Plus

1. ChatGPT → 우측 상단 프로필 → **Settings → Personalization → Custom Instructions**
2. `chatgpt-plus-instructions.md` 파일의 **필드 1** 내용 → "What would you like ChatGPT to know about you?"
3. **필드 2** 내용 → "How would you like ChatGPT to respond?"
4. **Save**

### 4. Codex CLI

```bash
mkdir -p ~/.codex
cp /home/user/vive-md/personalization/codex-agents.md ~/.codex/AGENTS.md
```

설치 후 모든 Codex 세션에서 자동으로 한국어 응답, superpowers Iron Laws,
기술 스택 컨텍스트, MCP 가이드가 적용됩니다.

### 3. Perplexity.ai

1. [perplexity.ai/settings](https://perplexity.ai/settings) 접속
2. **AI Profile** 또는 **Customize** 섹션
3. `perplexity-ai-profile.md` 파일 내 "붙여넣기용 텍스트" 복사 후 붙여넣기
4. Save

---

## 설정 업데이트

각 파일을 편집한 후 Claude Code 설정을 다시 적용하려면:

```bash
cp /home/user/vive-md/personalization/global-claude.md ~/.claude/CLAUDE.md
```

Claude.ai, Perplexity는 해당 설정 페이지에서 수동으로 업데이트합니다.

---

## 소스 레포지터리

- **superpowers** (`/home/user/superpowers`): 워크플로우 스킬 라이브러리 (v4.3.1)
- **vive-md** (`/home/user/vive-md`): 기술 스택 가이드, MCP 문서, 방법론 템플릿
- **research-notes** (`/home/user/research-notes`): AI 자동화 및 투자 리서치
