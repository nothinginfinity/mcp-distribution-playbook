# Render

**Status:** Viable alternative ✔  
**Complexity:** Low — connect GitHub repo, auto-deploy  
**Cold start:** Free tier sleeps after 15min inactivity (~30s wake); paid is instant  
**Cost:** Free tier (sleeps); $7/mo for always-on  
**URL:** https://render.com  

---

## Deploy Flow

```
1. Alice commits src/ + package.json (or Dockerfile) to GitHub
2. Go to render.com → New Web Service → Connect GitHub repo
3. Set start command: node src/index.js
4. Add env vars in Render dashboard
5. Render provides: https://<project>.onrender.com
6. MCP URL: https://<project>.onrender.com/mcp
```

## Strengths
- Very easy GitHub → live service flow
- Free tier available
- Auto-deploy on push
- Supports Node, Python, Go, Ruby, Dockerfile

## Weaknesses
- Free tier has cold start (~30s) after inactivity
- $7/mo for always-on
- Not edge-distributed

## When to Use
- Sharing open-source MCPs with users who want a free self-deploy option
- MCPs that need full Node.js ecosystem
- Demos and prototypes that don’t need zero cold start
