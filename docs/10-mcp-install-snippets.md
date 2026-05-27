# MCP Connect Config Snippets

Ready-to-paste configs for connecting MCPs in Claude, ChatGPT, Cursor, Windsurf, and other MCP-compatible clients.

---

## HTTP-based MCP (Cloudflare / Deno / Railway / Render / Fly.io)

```json
{
  "mcpServers": {
    "my-mcp": {
      "url": "https://my-mcp.agentfeedoptimization.com/mcp"
    }
  }
}
```

---

## Local / npx MCP

```json
{
  "mcpServers": {
    "my-mcp": {
      "command": "npx",
      "args": ["-y", "my-mcp-package"],
      "env": {
        "MY_SECRET": "your-value-here"
      }
    }
  }
}
```

---

## Docker / Self-hosted (local port)

```json
{
  "mcpServers": {
    "my-mcp": {
      "url": "http://localhost:3000/mcp"
    }
  }
}
```

---

## Claude Desktop (claude_desktop_config.json location)

```
macOS: ~/Library/Application Support/Claude/claude_desktop_config.json
Windows: %APPDATA%\Claude\claude_desktop_config.json
```

## Cursor (.cursor/mcp.json)

```json
{
  "mcpServers": {
    "my-mcp": {
      "url": "https://my-mcp.example.com/mcp"
    }
  }
}
```

## Windsurf (~/.codeium/windsurf/mcp_config.json)

```json
{
  "mcpServers": {
    "my-mcp": {
      "serverUrl": "https://my-mcp.example.com/mcp"
    }
  }
}
```

---

## Health Check Command (verify any MCP)

```bash
curl https://your-mcp-url.com/health
```

## Tools List Command (verify MCP surface)

```bash
curl -s -X POST https://your-mcp-url.com/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}' \
  | jq .result.tools[].name
```
