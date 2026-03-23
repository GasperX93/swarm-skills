# Swarm Skills

Claude Code skills for building on the Swarm decentralized storage network. Drop this into your project and your AI assistant knows how to build on Swarm.

## Setup

Copy the `.claude/` folder into your project root:

```bash
cp -r .claude/ /path/to/your-project/
```

Then use any skill by typing its name in Claude Code.

## Available Skills

### Setup & Infrastructure

| Skill | What it does |
|---|---|
| `/help` | Quick overview and routing — start here |
| `/setup-bee` | Install and run a Bee node (ultra-light → fund → light) |
| `/stamps` | List, buy, or manage postage stamps (required for uploads) |
| `/troubleshoot` | Diagnose node, connectivity, or upload issues |

### Store & Retrieve

| Skill | What it does |
|---|---|
| `/upload-download` | Upload and download data, files, or directories |
| `/host-website` | Deploy a website to Swarm (with optional ENS) |

### Build

| Skill | What it does |
|---|---|
| `/build-app` | Scaffold a Swarm dApp or add bee-js to your project |
| `/feed` | Create updateable content at a fixed address |

### Advanced

| Skill | What it does |
|---|---|
| `/act` | Encrypt data with per-account access control |
| `/messaging` | Real-time messaging (GSOC or PSS) |

## Developer Flow

```
/help → /setup-bee → /stamps → /upload-download or /build-app
                                          ↓
                            /host-website  /feed  /act  /messaging
```

## Links

- Swarm Docs: https://docs.ethswarm.org
- bee-js: https://bee-js.ethswarm.org/docs/
- swarm-cli: https://github.com/ethersphere/swarm-cli
