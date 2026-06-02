---
name: stamps
description: List, buy, size, top up, and dilute postage stamps (postage batches) — required before any Swarm upload. Covers depth vs. capacity, amount vs. duration/TTL, mutable vs. immutable, live-price cost estimation, and the bee-js Utils stamp helpers. Use when the user needs a stamp, asks about storage cost/capacity/expiry, or hits a 'no usable stamp' error.
user-invocable: true
---

# Manage Postage Stamps

Guide a developer through listing, buying, sizing, topping up, and managing postage stamps. Stamps are required before any upload to Swarm.

## Step 1: List Existing Stamps (DO THIS FIRST)

**Immediately run this command** before doing anything else — do not just show it to the user:

```bash
curl -s http://localhost:1633/stamps | jq '.stamps[] | {batchID, depth, usable, batchTTL, immutableFlag}'
```

If the command fails (connection refused, etc.), the node is not running — route to `/setup-bee`.

Present the results as a table with: batch ID (shortened), depth, effective capacity, type (mutable/immutable from `immutableFlag`), and approximate TTL in days (batchTTL / 86400).

If they have a usable stamp with enough capacity and TTL, ask if they want to reuse it instead of buying a new one. They can also top up or dilute an existing stamp (see [REFERENCE.md](REFERENCE.md)).

## What to Ask

1. **How much data?** Offer the size presets below.
2. **How long should it persist?** Offer the duration presets below.
3. **Will you update/overwrite the data?** (mutable vs immutable)

**If unsure, recommend:** depth 20 + 3 months (~680 MB effective, amount ~120,159,417,615) — a safe starting point for development and testing.

## How Stamps Work

- **Depth** controls capacity (how much data you can upload)
- **Amount** controls duration (how long it stays on Swarm)
- Both are set at purchase time. Amount can be topped up later. Depth can be increased (diluted) later.
- Stamps cost xBZZ. The node must be funded before buying.

### Mutable vs Immutable

- **Immutable (default):** Becomes unusable once capacity fills. Good for static content.
- **Mutable:** Older chunks get overwritten when full. Good for feeds and updateable content. Set `immutable` header to `false` when buying via API.

## Storage Presets

These are **effective (realistic) capacities** — not theoretical maximums. Actual capacity is lower than `2^depth * 4KB` because chunks distribute unevenly across 2^16 buckets.

### Size options

Effective capacities (verify with `Utils.getStampEffectiveBytes(depth)`):

| Size | Depth |
|------|-------|
| ~40 KB | 17 |
| ~6 MB | 18 |
| 110 MB | 19 |
| 680 MB | 20 |
| 2.6 GB | 21 |
| 7.7 GB | 22 |

> **Low depths are tiny:** depth 17 holds only ~40 KB (not ~7 MB — that figure is actually depth 18). Below depth 19, effective capacity is a small fraction of the `2^depth × 4KB` theoretical size because chunks spread unevenly across 2^16 buckets.

### Duration options

| Duration |
|----------|
| 15 days |
| 1 month |
| 3 months |
| 6 months |
| 1 year |

### Calculating amount from duration

Amount depends on the current network storage price:

```
amount = currentPrice * 17280 * days
```

Where `17280` = blocks per day (5-second blocks on Gnosis Chain).

**Quick reference** — these are **upper-bound estimates** based on a higher historical price (~77,261 PLUR/chunk/block) and can overpay by ~40% versus the current price. Treat them as ceilings and always compute from the live `currentPrice` above:

| Duration | Amount (upper-bound estimate) |
|----------|---------------------|
| 15 days | ~20,026,569,615 |
| 1 month | ~40,053,139,205 |
| 3 months | ~120,159,417,615 |
| 6 months | ~240,318,835,230 |
| 1 year | ~480,637,670,460 |

To compute the actual amount from the live price:

```bash
PRICE=$(curl -s http://localhost:1633/chainstate | jq .currentPrice)
echo $((PRICE * 17280 * 30))   # amount for 30 days
```

**Minimum amount** must cover 24 hours of storage.

## Buy a Stamp

### Before buying — estimate cost and check balance

**Always do this before purchasing — do not skip:**

1. Check wallet balance:
   ```bash
   curl -s http://localhost:1633/wallet | jq '{bzzBalance, nativeTokenBalance}'
   ```

2. Calculate cost with `Utils.getStampCost(depth, amount)` (returns a BZZ object), or this formula:
   ```
   cost in PLUR = amount * (2 ^ depth)
   cost in BZZ  = cost in PLUR / 10^16
   ```

3. Present the estimate to the user: depth, effective capacity, estimated TTL, estimated cost in BZZ, and current balance. **Ask for confirmation before proceeding.**

If balance is insufficient, suggest funding via `/setup-bee` or reusing an existing stamp.

### Via swarm-cli

```bash
swarm-cli stamp buy --depth 22 --amount 120159417615
```

Returns estimated cost, capacity, TTL, and the stamp ID. Save the stamp ID.

### Via API

```bash
curl -s -X POST http://localhost:1633/stamps/<amount>/<depth>
```

Example (~7.7 GB effective, ~3 months):

```bash
curl -s -X POST http://localhost:1633/stamps/120159417615/22
```

For mutable stamps:

```bash
curl -s -X POST http://localhost:1633/stamps/120159417615/22 -H "immutable: false"
```

Returns `batchID` and `txHash`.

### Via bee-js (recommended)

```javascript
import { Bee, Size, Duration } from '@ethersphere/bee-js'

const bee = new Bee('http://localhost:1633')
const batchId = await bee.buyStorage(Size.fromGigabytes(7.7), Duration.fromDays(90))
console.log('Stamp ID:', batchId)
```

Low-level alternative (if you need exact depth/amount control):

```javascript
const batchId = await bee.createPostageBatch('120159417615', 22)
```

**Important:** Wait several minutes after purchase for the stamp to propagate on the network before using it.

## TTL Warning

TTL is estimated based on current storage price, which fluctuates as Swarm adoption changes. Always maintain a buffer for important data. Content with expired stamps cannot be re-uploaded unless pinned locally.

## Managing an Existing Stamp

For deeper operations, see **[REFERENCE.md](REFERENCE.md)**:

- **bee-js utility functions** — `Utils.getStampCost`, `getStampEffectiveBytes`, `getDepthForSize`, `getAmountForDuration`, etc.
- **Manage stamps** — list, inspect (`stamp show`), top up (extend duration), dilute (increase capacity)
- **Check content retrievability** — `/stewardship/<reference>`

## Reference

- Stamp batches: https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch
- bee-js docs: https://bee-js.ethswarm.org/docs/
- Bee API: https://docs.ethswarm.org/api/
- swarm-cli: https://github.com/ethersphere/swarm-cli
