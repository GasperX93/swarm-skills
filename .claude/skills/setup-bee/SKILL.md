---
name: setup-bee
description: Install and run a Bee light node for Swarm development on Linux/macOS — system prerequisites (Node.js, curl), Bee and swarm-cli install, funding the node (gift code or xDAI/xBZZ on Gnosis Chain), upgrading ultra-light to light, and buying a first postage stamp. Use when the user has no node, gets connection-refused on localhost:1633, or needs to start or fund Bee.
user-invocable: true
---

# Set Up a Bee Node for Development

Guide a developer through getting a Bee light node running so they can build on Swarm.

## Before Starting (run immediately)

**Run these checks now — do not just show the commands to the user:**

1. Detect platform:
   ```bash
   uname -s
   ```
   - **Linux:** Use the install script directly
   - **Darwin (macOS):** Use the install script (or Homebrew if available)
   - **Other / Windows:** Advise WSL2 first, then the Linux install path

2. Fetch the latest Bee version tag:
   ```bash
   curl -s https://api.github.com/repos/ethersphere/bee/releases/latest | jq -r .tag_name
   ```

Use this tag in the install command below (replace TAG value).

## Node Modes

| Mode | Can download | Can upload | Needs funding | Use case |
|------|-------------|-----------|---------------|----------|
| Ultra-light | Yes | No | No | Exploring the API, downloading data |
| Light | Yes | Yes | Yes (~0.01 xDAI + ~0.2 xBZZ) | Development, uploads, feeds |
| Full | Yes | Yes | Yes + staking | Running infrastructure, PSS subscribe |

## Requirements

- Linux or macOS (Windows: use WSL2)
- Node.js v18+ and npm
- curl or wget
- ~0.01 xDAI + ~0.2 xBZZ on Gnosis Chain (for upgrading to light node)

## Step 0: Install System Prerequisites

A fresh machine usually has none of `curl`, `node`, or `npm`. Check first, then install only what's missing:

```bash
curl --version; node --version; npm --version
```

**Ubuntu / Debian:**

```bash
sudo apt-get update
sudo apt-get install -y curl
# Node.js 20 (recommended) via NodeSource:
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
# (or, for the distro's older Node 18 + npm: sudo apt-get install -y nodejs npm)
```

**macOS (Homebrew):**

```bash
brew install curl node
```

Re-run the check above and confirm `node --version` is v18 or higher before continuing.

## Step 1: Install Bee

The latest Bee release is **v2.8.0**. Fetch the current tag (see "Before Starting") or use it directly:

```bash
curl -s https://raw.githubusercontent.com/ethersphere/bee/master/install.sh | TAG=<LATEST_TAG> bash
```

Or with wget:

```bash
wget -q -O - https://raw.githubusercontent.com/ethersphere/bee/master/install.sh | TAG=<LATEST_TAG> bash
```

> **Non-root Linux users:** the script installs to system paths and fails with "Failed to install bee" without root. Run it through `sudo` while preserving the `TAG` variable:
> ```bash
> curl -s https://raw.githubusercontent.com/ethersphere/bee/master/install.sh | sudo env TAG=<LATEST_TAG> bash
> ```

Verify: `bee version`

Install swarm-cli (v3.x, which bundles bee-js 12.x):

```bash
npm install -g @ethersphere/swarm-cli
```

> **Version note:** Run the latest Bee — **2.8.x**. 2.8 was a breaking change, so **do not run 2.7.x**. This guide targets Bee **2.8.0**, swarm-cli **3.x**, and bee-js **12.x**. bee-js 12.x hasn't yet bumped its tested-version constant past Bee **2.7.0**, so `bee.isSupportedExactVersion()` returns `false` against a 2.8.0 node — that's a cosmetic version-*string* lag in bee-js, not a real incompatibility. bee-js prints **no** warning, and `bee.isSupportedApiVersion()` returns `true` (the HTTP API is compatible), so everything works normally. Inspect the exact numbers with `bee.getVersions()` if needed.

## Step 2: Start in Ultra-Light Mode

No funding needed — lets you explore the API and download data immediately.

```bash
bee start \
  --password YOUR_SECURE_PASSWORD \
  --api-addr 127.0.0.1:1633
```

Verify it's running:

```bash
curl -s http://localhost:1633/status | jq
```

> **Note:** Right after `bee start`, the API may return HTTP **503 "Node is syncing"** for up to ~30 seconds while it initializes. This is normal — wait and retry; it does not mean the node failed.

## Step 3: Fund Your Node

Get your wallet address:

```bash
curl -s localhost:1633/addresses | jq .ethereum
```

Send tokens to this address on **Gnosis Chain** (not Ethereum mainnet):
- **xDAI** — gas for transactions on Gnosis Chain (~0.01 needed)
- **xBZZ** — payment for storage on Swarm (~0.2+ needed, scales with usage)

### Funding options

**Option A — Redeem a gift code (fastest)**

If the developer has a gift code (a private key from Swarm), redeem it directly to the Bee wallet:

```bash
swarm-cli utility redeem <GIFT_CODE_PRIVATE_KEY>
```

This transfers xBZZ and xDAI from the gift wallet to the Bee node wallet automatically. The node's wallet address is detected from the running Bee node.

> **If redeem fails with "Upstream error: 429"**, the default RPC is rate-limited. Point swarm-cli at a different Gnosis RPC:
> ```bash
> swarm-cli utility redeem <GIFT_CODE_PRIVATE_KEY> --json-rpc-url https://rpc.gnosischain.com
> ```

To redeem to a specific wallet instead:

```bash
swarm-cli utility redeem <GIFT_CODE_PRIVATE_KEY> --target <WALLET_ADDRESS>
```

**Option B — Multichain top-up (any chain/token, no bridging)**

→ https://fund.ethswarm.org

**Option C — Manual**

xDAI from Gnosis faucets (https://docs.gnosischain.com/tools/Faucets) or bridge (https://bridge.gnosischain.com/). xBZZ from exchanges (https://www.ethswarm.org/get-bzz).

## Step 4: Upgrade to Light Node

Stop the ultra-light node (Ctrl+C), then restart with swap enabled:

```bash
bee start \
  --password YOUR_SECURE_PASSWORD \
  --swap-enable \
  --api-addr 127.0.0.1:1633 \
  --blockchain-rpc-endpoint https://xdai.fairdatasociety.org
```

> **If the node shuts down immediately with "Upstream error: 429"**, the default RPC (`xdai.fairdatasociety.org`) is rate-limited and Bee does not retry — it exits on the first 429. Restart with a fallback endpoint:
> ```bash
> bee start ... --blockchain-rpc-endpoint https://rpc.gnosischain.com
> ```
> The FDS endpoint is an archive node (better for initial batch-data sync), so prefer it when it's reachable and only fall back to `rpc.gnosischain.com` when you hit 429s.

The node deploys a chequebook and syncs chain data (~5 minutes). Monitor:

```bash
swarm-cli status
```

`swarm-cli status` does **not** print the word "synchronized" — instead the Chainsync section shows the current vs. latest block and their gap:

```
Chainsync
Block: 45,299,125 / 45,299,131 (Δ 6)
```

When the gap (**Δ**) is small — roughly **Δ < 10** — the node is caught up and ready.

## Step 5: Buy a Postage Stamp

Required before any upload. **Depth** sets capacity, **amount** sets duration.

> **Budget check first.** Cost in xBZZ ≈ `amount × 2^depth ÷ 10^16`. A depth-22, 3-month stamp costs **~4.2 xBZZ** — but a standard gift code only provides **~0.5 xBZZ**, so the old `--depth 22` suggestion fails with "You do not have enough BZZ". With a single gift code you can realistically only afford a small (depth-17, ~40 KB) stamp. Buy what your balance covers, or fund more xBZZ first (see Step 3).

### Compute the amount from the live price

The amount-per-day depends on the current network storage price, so don't hardcode it — read it live:

```bash
PRICE=$(curl -s http://localhost:1633/chainstate | jq .currentPrice)
# amount = currentPrice * 17280 (blocks/day) * desired_days
echo $((PRICE * 17280 * 30))   # ≈ amount for 30 days
```

### Budget-friendly stamp (fits a ~0.5 xBZZ gift code)

```bash
swarm-cli stamp buy --depth 17 --amount 9345732487    # ~40 KB, ~1 week, ~0.12 xBZZ
```

Or via API:

```bash
curl -X POST http://localhost:1633/stamps/9345732487/17
```

Save the **Stamp ID** returned.

### Stamp sizing

These are **effective (realistic) capacities** — not theoretical maximums (verify with `Utils.getStampEffectiveBytes(depth)` in bee-js 12.x):

| Depth | Effective capacity | | Duration | Amount (approx, varies with price) |
|-------|-------------------|-|----------|--------|
| 17 | ~40 KB | | 1 week | ~9,345,732,487 |
| 18 | ~6 MB | | 1 month | ~40,053,139,205 |
| 19 | ~110 MB | | 3 months | ~120,159,417,615 |
| 20 | ~680 MB | | 6 months | ~240,318,835,230 |
| 21 | ~2.6 GB | | 1 year | ~480,637,670,460 |
| 22 | ~7.7 GB | | | |

The amounts above are upper-bound estimates from a higher historical price and can overpay by ~40%. Always compute from the live `currentPrice` (above). For the full sizing/cost guide, see `/stamps`.

### Manage stamps later

```bash
swarm-cli stamp list
swarm-cli stamp show <stamp-id>
swarm-cli stamp topup --stamp <stamp-id> --amount <amount>
```

## Step 6: Test It

```bash
# Upload
swarm-cli upload --stdin --stamp <BATCH_ID> <<< "Hello Swarm"

# Download
swarm-cli download <SWARM_HASH>
```

If this works, the node is fully operational.

## Security

Always bind API to localhost (`127.0.0.1:1633`). Never expose port 1633 to the public internet.

## Reference

- Quick start: https://docs.ethswarm.org/docs/bee/installation/quick-start
- Fund your node: https://docs.ethswarm.org/docs/bee/installation/fund-your-node
- Configuration: `bee start --help`
- Bee API: https://docs.ethswarm.org/api/
- swarm-cli: https://github.com/ethersphere/swarm-cli
