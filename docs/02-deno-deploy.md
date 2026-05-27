# Deno Deploy

**Status:** Validated alternative ✔  
**Complexity:** Low — paste GitHub URL, instant deploy  
**Cold start:** ~50ms  
**Cost:** Free tier (1M req/mo); Pro $20/mo  
**URL:** https://deno.com/deploy  

---

## Why Use This

- Deploy directly from a GitHub file URL — no CLI, no dashboard setup
- TypeScript-native — no build step, no `tsc`
- Deno runtime is close to Cloudflare Workers API surface
- AFO Worker skeleton runs with minimal changes
- Great for fast prototypes and sharing MCPs with others

---

## Deploy Flow

```
1. Alice commits src/index.ts to GitHub
2. Go to dash.deno.com → New Project → Deploy from GitHub URL
3. Paste: https://raw.githubusercontent.com/nothinginfinity/<repo>/main/src/index.ts
4. Deno Deploy gives you: https://<project>.deno.dev
5. MCP URL: https://<project>.deno.dev/mcp
```

Or use the Deno Deploy GitHub integration to auto-deploy on push.

---

## Code Differences from CF Workers

The AFO skeleton is ~95% compatible. Key differences:

```ts
// Cloudflare Workers
export default {
  async fetch(req: Request, env: Env): Promise<Response> { ... }
};

// Deno Deploy
Deno.serve(async (req: Request): Promise<Response> => {
  // env vars via Deno.env.get('MY_SECRET')
  const mySecret = Deno.env.get('GITHUB_TOKEN') ?? '';
  // ... same handler logic
});
```

Bindings become `Deno.env.get()` calls. No D1 (use Turso, PlanetScale, or Neon for SQLite/Postgres).

---

## MCP Connect Config

```json
{
  "mcpServers": {
    "my-mcp": {
      "url": "https://my-project.deno.dev/mcp"
    }
  }
}
```

## Strengths
- Fastest path from GitHub source to live MCP
- No Cloudflare account needed
- TypeScript-native, no build toolchain
- Free tier is generous
- Easy to share with others (just give them the `.deno.dev` URL)

## Weaknesses
- No D1 (use external DB like Turso for SQLite)
- Slightly different env/binding API than CF Workers
- Custom domains require Pro tier or DNS setup
- Less AFO tooling automation vs Cloudflare path

## When to Use
- Prototyping a new MCP before committing to CF
- Sharing an MCP with someone who doesn’t have Cloudflare
- TypeScript-only tools with no D1 dependency
- Open-source MCPs you want anyone to fork + deploy in one click
