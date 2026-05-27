# Docker / Self-Hosted

**Status:** Enterprise / advanced ✔  
**Complexity:** High  
**Cost:** Your own infra  

---

## Why Use This

- Full control over runtime, networking, and data
- Private MCPs that never touch external cloud
- Enterprise deployments behind a VPN or firewall
- Custom hardware (GPU MCPs, large memory, etc.)

---

## Dockerfile (AFO-compatible)

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY src/ ./src/
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s CMD curl -f http://localhost:3000/health || exit 1
CMD ["node", "src/index.js"]
```

## docker-compose.yml

```yaml
version: '3.8'
services:
  mcp:
    build: .
    ports:
      - "3000:3000"
    environment:
      - GITHUB_TOKEN=${GITHUB_TOKEN}
      - MY_SECRET=${MY_SECRET}
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 3s
      retries: 3
```

## MCP Connect Config

```json
{
  "mcpServers": {
    "afo-example-mcp": {
      "url": "http://localhost:3000/mcp"
    }
  }
}
```

## When to Use
- Private enterprise MCPs
- MCPs with GPU or special hardware needs
- Air-gapped / regulated environments
- High-volume MCPs where cloud cost matters
