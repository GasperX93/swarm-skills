---
name: help
description: Swarm skills overview and routing to the right skill
user-invocable: true
---

# Get Started with Swarm

When a developer starts a conversation about building on Swarm, give them a quick overview of what's available and route them to the right skill.

## Show This Overview

```
Welcome! Here's what I can help you with:

🐝 Setup & Infrastructure
  /setup-bee      — Install and run a Bee node (ultra-light → light)
  /stamps      — Buy or manage postage stamps (required for uploads)
  /troubleshoot   — Diagnose node, connectivity, or upload issues

📦 Store & Retrieve
  /upload-download — Upload and download data, files, or directories
  /host-website    — Deploy a website to Swarm (with optional ENS)

🔧 Build
  /build-app       — Scaffold a Swarm dApp or add bee-js to your project
  /feed            — Create updateable content at a fixed address

🔒 Advanced
  /act             — Encrypt data with per-account access control
  /messaging       — Real-time messaging (GSOC or PSS)
```

## Then Ask

"What are you looking to build?" and route them based on their answer:

| They say... | Route to |
|---|---|
| "I'm new" / "getting started" / "first time" | `/setup-bee` |
| "upload" / "store data" / "download" | `/upload-download` (check node first) |
| "deploy a website" / "host a site" | `/host-website` |
| "build an app" / "scaffold" / "dApp" | `/build-app` |
| "feed" / "dynamic content" / "update without changing URL" | `/feed` |
| "stamp" / "storage" / "how much does it cost" | `/stamps` |
| "encrypt" / "private" / "access control" | `/act` |
| "chat" / "messaging" / "real-time" / "notifications" | `/messaging` |
| "not working" / "error" / "can't connect" | `/troubleshoot` |
| "no code" / "just deploy" | Suggest Beeport (beeport.ethswarm.org) — no node needed |

## Quick Path Check

If unclear where they are in their journey, ask:

1. **Do you have a Bee node running?** No → `/setup-bee`
2. **Do you have a postage stamp?** No → `/stamps`
3. **What do you want to build?** → route to the right skill
