# Swarm Skills for Claude Code

Turn [Claude Code](https://claude.ai/code) into a Swarm development assistant. Copy one folder into your project, type `/help`, and get hands-on guidance for storing data, hosting websites, building dApps, and more on the [Swarm](https://ethswarm.org) decentralized storage network.

## What This Is

Swarm Skills is a set of interactive guides (Claude Code skills) that live inside your project. Each skill checks your environment, runs real commands against your Bee node, and walks you through the task step by step. No separate docs to read — just type the skill name and go.

## Quick Start

### 1. Clone into your project

```bash
git clone https://github.com/GasperX93/swarm-skills.git
cp -r swarm-skills/.claude/ /path/to/your-project/
```

Or add directly to an existing project:

```bash
cd your-project
curl -sL https://github.com/GasperX93/swarm-skills/archive/main.tar.gz | tar xz --strip-components=1 "swarm-skills-main/.claude"
```

### 2. Open Claude Code in your project

```bash
cd your-project
claude
```

### 3. Type `/help`

You'll see all available skills and get routed to the right one based on what you want to do.

## Available Skills

### Setup & Infrastructure

| Skill | What it does |
|---|---|
| `/help` | Overview of all skills — start here |
| `/setup-bee` | Install and run a Bee node (ultra-light, fund it, upgrade to light) |
| `/stamps` | List, buy, top up, dilute, and manage postage stamps |
| `/troubleshoot` | Diagnose node, connectivity, and upload issues |

### Store & Retrieve

| Skill | What it does |
|---|---|
| `/upload-download` | Upload and download data, files, or directories |
| `/host-website` | Deploy a static website to Swarm with optional ENS domain |

### Build

| Skill | What it does |
|---|---|
| `/build-app` | Scaffold a Swarm dApp or add bee-js to an existing project |
| `/feed` | Create updateable content at a fixed address (feeds) |

### Advanced

| Skill | What it does |
|---|---|
| `/act` | Encrypt data with per-account access control (ACT) |
| `/messaging` | Real-time messaging via GSOC or PSS |

## Developer Flow

```
/help → /setup-bee → /stamps → /upload-download or /build-app
                                        |
                          /host-website  /feed  /act  /messaging
```

**No node needed?** Deploy a website through [Beeport](https://beeport.ethswarm.org) — no Bee node required.

## What the Skills Actually Do

These aren't static docs. Each skill:

- **Checks prerequisites** — is your node running? Do you have a stamp? If not, it routes you to the right skill first.
- **Runs real commands** — queries your Bee node API, lists your stamps, uploads your files.
- **Covers multiple tools** — examples for both [bee-js](https://github.com/ethersphere/bee-js) (JavaScript SDK) and [swarm-cli](https://github.com/ethersphere/swarm-cli) (CLI tool).
- **Handles errors** — if something breaks, `/troubleshoot` walks through diagnostics step by step.

## Requirements

- [Claude Code](https://claude.ai/code) (Claude's CLI tool)
- [Node.js](https://nodejs.org) 18+
- For most skills: a running [Bee](https://docs.ethswarm.org/docs/bee/installation/install) light node at `http://localhost:1633`

## About Swarm

Swarm is a decentralized peer-to-peer storage network and part of the Ethereum ecosystem. Files are split into 4 KB chunks, distributed across nodes, and retrievable by content hash. Production-ready since 2021.

- Docs: https://docs.ethswarm.org
- bee-js SDK: https://bee-js.ethswarm.org/docs/
- Bee API reference: https://docs.ethswarm.org/api/
- swarm-cli: https://github.com/ethersphere/swarm-cli
- Beeport (no-node deploys): https://beeport.ethswarm.org
- Discord: https://discord.gg/hyCr9BMX9U

## Contributing

Each skill is a standalone markdown file in `.claude/skills/`. To edit or add a skill:

1. Skills should be self-contained — include everything needed for that topic.
2. Always check prerequisites and route to the right skill if something is missing.
3. Cover both bee-js and swarm-cli where applicable.
4. Reference other skills using `/skill-name` format.

## License

MIT
