# Cloudflare Workers — AFO Primary Path

**Status:** Production ✔  
**Complexity:** Medium (automated via Cf-multipart)  
**Cold start:** ~0ms (always warm at edge)  
**Cost:** Free up to 100k req/day; $5/mo Workers Paid for more  

---

## Why This is the AFO Default

- Zero cold starts — Workers run at Cloudflare edge, always warm
- Custom domain support (`<worker>.agentfeedoptimization.com`)
- D1 SQLite, KV, R2 bindings available
- `cloudflare-multipart-mcp` automates the deploy step
- No Docker, no server management, no ports

---

## Deploy Flow

```
1. Alice commits src/index.ts + wrangler.toml to GitHub
2. Claude calls deploy_worker_with_bindings (cloudflare-multipart-mcp)
3. Jared adds custom domain in CF dashboard (or cloudflare-domain-manager-mcp)
4. Jared adds secrets/bindings if needed
5. Any agent runs <worker>_status smoke test
```

## wrangler.toml Template

```toml
name = "afo-example-mcp"
main = "src/index.ts"
compatibility_date = "2024-09-23"

[vars]
DEFAULT_OWNER = "nothinginfinity"

[[d1_databases]]
binding = "DB"
database_name = "example-db"
database_id = "YOUR_D1_UUID_HERE"
```

## MCP Connect Config

```json
{
  "mcpServers": {
    "afo-example-mcp": {
      "url": "https://afo-example-mcp.agentfeedoptimization.com/mcp"
    }
  }
}
```

## Strengths
- Best performance (edge, zero cold start)
- Full AFO binding ecosystem (D1, KV, R2)
- `cloudflare-multipart-mcp` makes deploy agent-friendly
- Custom domains, HTTPS automatic

## Weaknesses
- Requires Cloudflare account + domain
- Secrets/bindings still need dashboard or Cf-multipart
- Custom domain step still partly manual (until `cloudflare-domain-manager-mcp` is built)

## Tooling
- Deploy: `cloudflare-multipart-mcp` → `deploy_worker_with_bindings`
- D1: `afo-d1-admin-mcp` → `create_d1_database`
- Source: `afo-gitzip-push-mcp` → `push_files_to_github`
- Domain (planned): `cloudflare-domain-manager-mcp` → `add_custom_domain`
