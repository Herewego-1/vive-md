---
name: mcp-server-assistant
description: >
  MCP(Model Context Protocol) 서버를 설계, 개발, 설정, 보안 강화할 때 사용한다.
  "MCP 서버 만들어줘", "MCP 연결 방법", "MCP 도구 추가" 같은 요청에 반드시 실행한다.
  TypeScript SDK, Python FastMCP, OAuth 2.0, 실전 패턴을 모두 포함한다.
---

# MCP 서버 어시스턴트

## 개요

vive-md MCP 지식센터(6개 문서, ~9,700줄)를 기반으로 한 MCP 서버 전문 도우미.
설계부터 보안까지 전 과정을 지원한다.

---

## MCP 핵심 개념 빠른 참조

### 아키텍처
```
Claude (Client/Host)
    ↓ MCP Protocol (JSON-RPC 2.0)
MCP Server
    ↓
External Systems (DB, API, File System, etc.)
```

### Transport 방식
| 방식 | 사용 상황 |
|------|----------|
| stdio | 로컬 프로세스, Claude Desktop |
| HTTP + SSE | 원격 서버, 웹 배포 |
| Streamable HTTP | 최신 방식, 권장 |

### 3가지 Primitive
- **Tools**: 액션 실행 (함수 호출)
- **Resources**: 데이터 읽기 (파일, DB 조회)
- **Prompts**: 재사용 가능한 프롬프트 템플릿

---

## 워크플로우

### 1단계: 요구사항 파악

사용자에게 확인할 것:
- [ ] 어떤 외부 시스템과 연결? (DB, GitHub, Slack, 파일시스템 등)
- [ ] 로컬 서버인가 원격 서버인가?
- [ ] TypeScript vs Python 선호?
- [ ] 인증이 필요한가?

### 2단계: 기술 스택 선택

**TypeScript SDK** 선택 기준:
- Node.js 환경, npm 생태계 활용
- 타입 안전성 필요
- 기존 JS/TS 프로젝트 통합

**Python FastMCP** 선택 기준:
- Python 생태계 (pandas, scipy 등) 활용
- 데이터 분석/AI 연동
- 빠른 프로토타이핑

### 3단계: 서버 구현

**TypeScript 기본 구조:**
```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server(
  { name: "my-server", version: "1.0.0" },
  { capabilities: { tools: {}, resources: {} } }
);

// Tool 등록
server.setRequestHandler(ListToolsRequestSchema, async () => ({
  tools: [{ name: "my-tool", description: "...", inputSchema: { ... } }]
}));

server.setRequestHandler(CallToolRequestSchema, async (request) => {
  // 구현
});

const transport = new StdioServerTransport();
await server.connect(transport);
```

**Python FastMCP 기본 구조:**
```python
from fastmcp import FastMCP

mcp = FastMCP("my-server")

@mcp.tool()
def my_tool(param: str) -> str:
    """도구 설명"""
    return f"결과: {param}"

if __name__ == "__main__":
    mcp.run()
```

### 4단계: 보안 체크리스트

<HARD-GATE>
배포 전 반드시 확인:
- [ ] 입력값 검증 (SQL Injection, Path Traversal 방지)
- [ ] 민감 정보 환경변수 처리 (코드에 하드코딩 금지)
- [ ] Rate Limiting 적용
- [ ] OAuth 2.0 / API Key 인증 구현 (원격 서버)
- [ ] HTTPS 강제 (원격 서버)
- [ ] 최소 권한 원칙 (필요한 접근만 허용)
</HARD-GATE>

---

## 실전 패턴

### DB 연결 패턴 (PostgreSQL)
```typescript
import { Pool } from "pg";

const pool = new Pool({ connectionString: process.env.DATABASE_URL });

server.setRequestHandler(CallToolRequestSchema, async (request) => {
  if (request.params.name === "query_db") {
    const { sql, params } = request.params.arguments;
    // 파라미터화 쿼리 필수 (SQL Injection 방지)
    const result = await pool.query(sql, params);
    return { content: [{ type: "text", text: JSON.stringify(result.rows) }] };
  }
});
```

### GitHub API 연동 패턴
```typescript
import { Octokit } from "@octokit/rest";

const octokit = new Octokit({ auth: process.env.GITHUB_TOKEN });
// PR 목록, 이슈, 코드 검색 등 구현
```

---

## Claude Desktop 설정 (`~/.claude/claude_desktop_config.json`)

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["/path/to/server/build/index.js"],
      "env": {
        "API_KEY": "your-key"
      }
    }
  }
}
```

---

## 인기 MCP 서버 카탈로그 (즉시 사용 가능)

| 카테고리 | 서버 | 설명 |
|---------|------|------|
| 파일시스템 | `@modelcontextprotocol/server-filesystem` | 로컬 파일 읽기/쓰기 |
| GitHub | `@modelcontextprotocol/server-github` | PR, 이슈, 코드 관리 |
| 데이터베이스 | `@modelcontextprotocol/server-postgres` | PostgreSQL 쿼리 |
| 웹 | `@modelcontextprotocol/server-puppeteer` | 웹 브라우저 자동화 |
| 검색 | `@modelcontextprotocol/server-brave-search` | Brave 웹 검색 |
| 슬랙 | `@modelcontextprotocol/server-slack` | 슬랙 메시지/채널 |

전체 2596개+ 서버 목록: `vive-md/vibe-coding/mcp/Awesome-MCP-Servers-한국어-가이드.md`
