# MCP Distribution Decision Guide

Use this matrix to pick the right runtime for a given MCP.

---

## Decision Tree

```
Is this an AFO production tool?
  YES → Cloudflare Workers (primary path)
  NO → continue...

Does it need D1 / KV / R2?
  YES → Cloudflare Workers
  NO → continue...

Is this a prototype or proof of concept?
  YES + TypeScript only → Deno Deploy
  YES + Node.js needed → Railway or Render
  NO → continue...

Do you want others to self-host it easily?
  YES + developers → npx / local
  YES + non-technical → Deno Deploy (one-click fork)
  NO → continue...

Does it need multi-region or custom Docker?
  YES → Fly.io
  NO → continue...

Is it a scheduled job or CI task?
  YES → GitHub Actions (not a live HTTP MCP)

Is it private / enterprise?
  YES → Docker / self-hosted
```

---

## Feature Matrix

| Feature | CF Workers | Deno Deploy | Railway | Render | Fly.io | npx | Docker |
|---|---|---|---|---|---|---|---|
| Zero cold start | ✔ | ≈ | ✔ paid | ✔ paid | ✔ | — | ✔ |
| Free tier | ✔ | ✔ | Trial | ✔ (sleeps) | ✔ | ✔ | ✔ |
| D1 / SQLite | ✔ native | Turso | Any | Any | Any | Any | Any |
| Custom domain | ✔ | Pro | ✔ | ✔ | ✔ | — | ✔ |
| Deploy from GitHub | ✔ (Cf-multipart) | ✔ URL | ✔ | ✔ | Dockerfile | ✔ npm | ✔ |
| Agent-automated deploy | ✔ (Cf-multipart) | Partial | — | — | — | — | — |
| Private / VPN | ✔ | — | ✔ | ✔ | ✔ | ✔ | ✔ |
| Multi-region | ✔ | ✔ | ✔ | — | ✔ | — | Manual |
| Node npm packages | ≈ (compat layer) | ≈ | ✔ | ✔ | ✔ | ✔ | ✔ |
| GPU / large memory | — | — | ✔ | ✔ | ✔ | ✔ | ✔ |

---

## MCP Audience Matrix

| Audience | Recommended Path |
|---|---|
| AFO internal agents | Cloudflare Workers |
| Other developers (open-source) | npx + Deno Deploy (fork button) |
| Non-technical users | Hosted URL (CF or Deno) |
| Enterprise / private | Docker self-hosted or Fly.io private |
| Prototype / experiment | Deno Deploy or Railway |
| Claude Desktop / Cursor users | npx local |
