# MCP Distribution Playbook

> **Author:** Alice (AFO Build Agent)  
> **Date:** 2026-05-27  
> **Status:** Living document — update as new runtimes are validated

This repo documents every viable way to host, deploy, and distribute AFO-style MCP tools. The goal is to have **multiple distribution paths** so Jared and any agent can choose the right runtime for the job — or offer users multiple install/connect options.

---

## Quick Reference

| Runtime | Cold Start | Cost | Deploy Complexity | Best For |
|---|---|---|---|---|
| [Cloudflare Workers](docs/01-cloudflare-workers.md) | ~0ms | Free → $5/mo | Medium (Cf-multipart automates) | Production AFO MCPs |
| [Deno Deploy](docs/02-deno-deploy.md) | ~50ms | Free tier | Low (GitHub URL deploy) | Fast prototypes, TypeScript-native |
| [Railway](docs/03-railway.md) | ~500ms | $5/mo | Low (GitHub connect) | Persistent servers, Node/Bun MCPs |
| [Render](docs/04-render.md) | Cold ~30s | Free tier | Low (GitHub connect) | Always-on with paid tier |
| [Fly.io](docs/05-fly-io.md) | ~100ms | Free tier | Medium (flyctl CLI) | Multi-region, Docker-based MCPs |
| [GitHub Actions](docs/06-github-actions.md) | N/A | Free tier | Low | Scheduled/event MCPs, not HTTP |
| [Local / npx](docs/07-local-npx.md) | Instant | Free | None | Dev + personal use |
| [Docker / Self-hosted](docs/08-docker-selfhosted.md) | Varies | Infra cost | High | Enterprise, private MCPs |

---

## AFO Primary Path

The default AFO distribution path remains:

```
Alice commits source to GitHub
  → Claude/ChatGPT deploys via cloudflare-multipart-mcp
  → Jared adds custom domain
  → Live at https://<worker>.agentfeedoptimization.com/mcp
```

See [docs/01-cloudflare-workers.md](docs/01-cloudflare-workers.md) for the full AFO Cloudflare pattern.

---

## Contents

```
docs/
  01-cloudflare-workers.md     — Primary AFO path
  02-deno-deploy.md            — GitHub URL → live edge function
  03-railway.md                — GitHub repo → persistent server
  04-render.md                 — GitHub repo → web service
  05-fly-io.md                 — Docker → multi-region
  06-github-actions.md         — Event/schedule MCPs
  07-local-npx.md              — Dev + personal npx install
  08-docker-selfhosted.md      — Enterprise / private
  09-distribution-matrix.md    — Decision guide: which runtime to choose
  10-mcp-install-snippets.md   — Ready-to-paste MCP connect configs
```
