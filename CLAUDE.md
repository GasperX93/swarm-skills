# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is the **swarm-skills** repo — AI-powered interactive guides that help developers build on the Swarm decentralized storage network. Currently packaged as Claude Code skills, with support for other AI coding tools (Cursor, GitHub Copilot, Windsurf, Codex) planned. There is no application code or build system. The repo is almost entirely skill definitions (markdown files); the one exception is `scripts/verify-beejs.mjs`, which asserts the bee-js / Bee API facts the skills depend on.

### Verifying skills against the latest bee-js

After bumping the documented bee-js version (or to catch upstream API drift), run:

```bash
npm i @ethersphere/bee-js@12 && node scripts/verify-beejs.mjs
```

It exits non-zero if any documented fact (Utils helper names, capacity numbers, ACT/messaging types) no longer holds.

## Structure

Skills live in `.claude/skills/`. Each skill is a directory containing a `SKILL.md` file that Claude Code discovers automatically.

```
.claude/skills/
  help/SKILL.md             — Entry point: overview + routing
  setup-bee/SKILL.md        — Install and run a Bee node
  stamps/SKILL.md           — List, buy, and manage postage stamps
  upload-download/SKILL.md  — Upload and download data/files
  host-website/SKILL.md     — Deploy a website to Swarm
  build-app/SKILL.md        — Scaffold a dApp or add bee-js
  feed/SKILL.md             — Feeds for dynamic content
  act/SKILL.md              — Access control (encrypted data)
  messaging/SKILL.md        — Real-time messaging (GSOC/PSS)
  troubleshoot/SKILL.md     — Diagnose node and upload issues
```

## Editing Skills

- Each skill is standalone — it should contain everything needed for that topic.
- Skills should always check prerequisites (node running? stamp exists?) and route to the appropriate skill if not.
- When referencing other skills, use the `/skill-name` format.
- Code examples should cover both **bee-js** and **swarm-cli** where applicable.
- Keep commands and code up to date with the latest Bee and bee-js versions.
- Skills last verified against: **Bee 2.8.0**, **bee-js 12.x**, **swarm-cli 3.x**
- Note: bee-js 12.x is tested against Bee **2.7.0**, so `bee.isSupportedExactVersion()` returns `false` against a 2.8.0 node. This is only a version-string difference — bee-js prints no warning and `bee.isSupportedApiVersion()` returns `true` (HTTP API compatible), so it works normally. Verified live against Bee 2.8.0 / API 7.4.1. All stamp helpers (`getStampCost`, `getStampEffectiveBytes`, `getDepthForSize`, `getAmountForDuration`, `getStampDuration`, `getStampTheoreticalBytes`, `getStampUsage`) live under the `Utils` namespace, not as top-level exports.

## Swarm Quick Reference

Swarm is a decentralized peer-to-peer storage network. Files are split into 4KB chunks, distributed across nodes, and retrievable by content hash. Part of Ethereum's ecosystem, production-ready since 2021.

### Developer flow

```
/help → /setup-bee → /stamps → /upload-download or /build-app
                                          ↓
                            /host-website  /feed  /act  /messaging
```

### Two paths

- **No node needed:** Deploy a website via Beeport (beeport.ethswarm.org)
- **Everything else:** Requires a Bee light node at `http://localhost:1633`

### Key tools

- **bee-js** (`@ethersphere/bee-js`) — JavaScript SDK
- **swarm-cli** (`@ethersphere/swarm-cli`) — CLI tool
- **create-swarm-app** — dApp scaffolding
- **swarm-mcp** — MCP server for AI agents

### Effective storage capacity (per stamp depth)

These are realistic capacities, not theoretical maximums:

| Depth | Effective capacity |
|-------|--------------------|
| 19 | ~110 MB |
| 20 | ~680 MB |
| 21 | ~2.6 GB |
| 22 | ~7.7 GB |
| 23 | ~20 GB |
| 24 | ~47 GB |

## Agent Safety Rules

- Never echo private keys, seed phrases, or gift codes to terminal output in plain text
- Never include private keys in code files that could be committed to version control
- Always confirm with the user before executing operations that cost xBZZ (stamp buy, top-up, dilute)
- Never expose port 1633 to the public internet
- Store private keys in environment variables or secure key files, not inline in code
- Use ES module `import` syntax in all code examples (not `require()`)

### Key links

- Docs: https://docs.ethswarm.org
- bee-js: https://bee-js.ethswarm.org/docs/
- API: https://docs.ethswarm.org/api/
- swarm-cli: https://github.com/ethersphere/swarm-cli
- swarm-mcp: https://github.com/ethersphere/swarm-mcp
- Beeport: https://beeport.ethswarm.org
- GitHub: https://github.com/ethersphere
- Discord: https://discord.gg/hyCr9BMX9U
