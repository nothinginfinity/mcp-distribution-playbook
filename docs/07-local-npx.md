# Local / npx Distribution

**Status:** Dev + personal use ✔  
**Complexity:** None for user  
**Cost:** Free  

---

## Why This Matters

For open-source or developer-facing MCPs, you can publish to npm and let users run your MCP locally with a single command:

```bash
npx afo-example-mcp
```

This is how many popular MCPs are distributed today (e.g. `@modelcontextprotocol/server-filesystem`).

---

## Package Structure

```
afo-example-mcp/
├── package.json      ← "bin": { "afo-example-mcp": "./bin/start.js" }
├── bin/
│   └── start.js         ← #!/usr/bin/env node + starts HTTP server
└── src/
    └── index.js
```

## package.json

```json
{
  "name": "afo-example-mcp",
  "version": "0.1.0",
  "type": "module",
  "bin": {
    "afo-example-mcp": "./bin/start.js"
  },
  "scripts": {
    "start": "node bin/start.js"
  }
}
```

## MCP Connect Config (local)

```json
{
  "mcpServers": {
    "afo-example-mcp": {
      "command": "npx",
      "args": ["-y", "afo-example-mcp"],
      "env": {
        "MY_SECRET": "user-provides-this"
      }
    }
  }
}
```

## Strengths
- Zero hosting cost
- Users control their own secrets (never leave their machine)
- Works offline
- npm publish = instant distribution to anyone

## Weaknesses
- User must have Node.js installed
- No shared state across users
- Not suitable for cloud-hosted AI agents (needs a URL)

## When to Use
- Open-source MCPs for developers
- Personal productivity MCPs with local secrets
- MCPs that read/write local files (filesystem, git, etc.)
- Distribution to Claude Desktop, Cursor, Windsurf users
