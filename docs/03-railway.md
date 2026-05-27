# Railway

**Status:** Viable alternative ✔  
**Complexity:** Low — connect GitHub repo, auto-deploy on push  
**Cold start:** ~500ms (persistent server, so rare)  
**Cost:** Free trial; $5/mo Hobby plan for persistent servers  
**URL:** https://railway.app  

---

## Why Use This

- GitHub repo → live server in minutes
- Persistent Node.js / Bun / Deno process — no Worker constraints
- Auto-deploy on push to `main`
- Good for MCPs that need long-running connections, WebSockets, or large memory
- Supports any language/runtime via Dockerfile or Nixpacks

---

## Deploy Flow

```
1. Alice commits src/ + package.json to GitHub
2. Go to railway.app → New Project → Deploy from GitHub repo
3. Railway auto-detects Node/Bun/Deno and builds
4. Set env vars in Railway dashboard (secrets, tokens)
5. Railway provides: https://<project>.up.railway.app
6. MCP URL: https://<project>.up.railway.app/mcp
```

---

## Minimal package.json for Node MCP

```json
{
  "name": "afo-example-mcp",
  "version": "0.1.0",
  "type": "module",
  "scripts": {
    "start": "node src/index.js"
  }
}
```

## Minimal Node HTTP server skeleton (AFO-compatible)

```js
import { createServer } from 'http';

const NAME = 'afo-example-mcp';
const VERSION = '0.1.0';
const PORT = process.env.PORT || 3000;

const CORS = {
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Methods': 'GET,POST,OPTIONS',
  'Access-Control-Allow-Headers': 'Content-Type, Authorization',
  'Cache-Control': 'no-store'
};

createServer(async (req, res) => {
  const url = new URL(req.url, `http://localhost`);
  Object.entries(CORS).forEach(([k, v]) => res.setHeader(k, v));

  if (req.method === 'OPTIONS') { res.writeHead(204); res.end(); return; }

  if (url.pathname === '/health') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ status: 'ok', worker: NAME, version: VERSION, bindings: {} }));
    return;
  }

  if (url.pathname === '/mcp' && req.method === 'POST') {
    // ... JSON-RPC handler
  }

  res.writeHead(404); res.end(NAME + ': not found');
}).listen(PORT);
```

## Strengths
- Persistent server — no Worker size/CPU limits
- Any Node/Bun package available (Prisma, pg, redis, etc.)
- Auto-deploy on GitHub push
- Good free trial

## Weaknesses
- $5/mo for always-on (free tier sleeps)
- Slightly heavier than CF Workers for simple MCPs
- No built-in edge network

## When to Use
- MCPs that use npm packages not available in Workers
- Long-running jobs or WebSocket connections
- Node.js-native integrations (Prisma, Bull, etc.)
