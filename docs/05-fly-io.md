# Fly.io

**Status:** Viable alternative ✔  
**Complexity:** Medium — requires flyctl CLI + Dockerfile  
**Cold start:** ~100ms (machines wake fast)  
**Cost:** Free tier (3 shared VMs); $1.94/mo per always-on micro VM  
**URL:** https://fly.io  

---

## Why Use This

- Multi-region deployment (run your MCP close to users worldwide)
- Docker-based — any language, any runtime
- Persistent volumes available
- Good for enterprise or private MCPs that need isolation

---

## Deploy Flow

```
1. Alice commits Dockerfile + src/ to GitHub
2. Install flyctl: curl -L https://fly.io/install.sh | sh
3. fly launch (creates fly.toml)
4. fly deploy
5. Fly provides: https://<app>.fly.dev
6. MCP URL: https://<app>.fly.dev/mcp
```

## Minimal Dockerfile for AFO MCP

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY src/ ./src/
EXPOSE 3000
CMD ["node", "src/index.js"]
```

## Strengths
- True multi-region edge deployment
- Any Docker image — maximum flexibility
- Persistent volumes for stateful MCPs
- Great for private/enterprise MCPs

## Weaknesses
- Requires Docker knowledge
- flyctl CLI setup
- More complex than Railway/Render for simple MCPs

## When to Use
- Enterprise or private MCPs needing isolation
- Multi-region requirements
- MCPs with unusual runtime needs (Python ML, Rust, Go)
