# GitHub Actions MCPs

**Status:** Specialized use case ✔  
**Complexity:** Low for scheduled/event; not suitable for HTTP MCP  
**Cost:** Free (2000 min/mo on free tier)  

---

## Important Limitation

GitHub Actions **cannot serve a persistent HTTP endpoint**. A running Action job is not publicly reachable as an MCP server.

**What Actions CAN do for MCPs:**

```
✔ Trigger MCP builds on push (call Cf-multipart via curl)
✔ Run smoke tests against a deployed MCP after push
✔ Scheduled data refresh (call an MCP tool on a cron)
✔ Auto-commit generated files to GitHub (acts like a GitZip)
✔ Generate MCP_SCHEMA.json from source on every push
```

---

## Useful GitHub Actions Patterns for AFO

### Auto smoke-test on deploy

```yaml
name: MCP Smoke Test
on:
  push:
    branches: [main]
jobs:
  smoke:
    runs-on: ubuntu-latest
    steps:
      - name: Health check
        run: curl -f https://afo-example-mcp.agentfeedoptimization.com/health
      - name: Tools list
        run: |
          curl -s -X POST https://afo-example-mcp.agentfeedoptimization.com/mcp \
            -H 'Content-Type: application/json' \
            -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'
```

### Auto-generate MCP_SCHEMA.json on push

```yaml
name: Generate Schema
on:
  push:
    paths: ['src/index.ts']
jobs:
  schema:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Extract tool schemas
        run: node scripts/extract-schema.js > MCP_SCHEMA.json
      - uses: stefanzweifel/git-auto-commit-action@v5
        with:
          commit_message: 'chore: regenerate MCP_SCHEMA.json'
```

## When to Use
- CI/CD smoke tests after every Worker deploy
- Scheduled data sync jobs that call an existing MCP
- Auto-generating schema/docs from source
- NOT for serving a live MCP endpoint
