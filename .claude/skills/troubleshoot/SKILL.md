---
name: troubleshoot
description: Diagnose and fix common Bee node, connectivity, and upload issues
user-invocable: true
---

# Troubleshoot Bee Node

Guide a developer through diagnosing and fixing common Bee node issues. **Run these checks in order** — each step depends on the previous one passing. Stop at the first failure, fix it, then continue.

## Triage Tree

```
(1) Is the bee process running?
    └─ No → start bee / check port conflict
    └─ Yes ↓
(2) Is the API reachable at localhost:1633?
    └─ No → check firewall / port binding
    └─ Yes ↓
(3) Does the node have peers / is it synced?
    └─ No → wait / check RPC endpoint
    └─ Yes ↓
(4) Is the wallet funded?
    └─ No → route to /setup-bee funding
    └─ Yes ↓
(5) Does a valid, non-expired stamp exist?
    └─ No → route to /stamps
    └─ Yes ↓
(6) Is the upload/download itself failing?
    └─ Yes → check error code table below
    └─ No → problem is elsewhere
```

## Step 1: Is the node running?

```bash
curl -s http://localhost:1633/status | jq
```

If this fails, the node isn't running or the API port is wrong.

**Fixes:**
- Start the node: `bee start --password YOUR_PASSWORD --api-addr 127.0.0.1:1633`
- Check if another process uses port 1633: `lsof -i :1633`
- Check Bee logs for startup errors

## Step 2: Is the node connected to peers?

```bash
swarm-cli status
```

Or:

```bash
curl -s http://localhost:1633/topology | jq '{connected, population, depth}'
```

If connected peers = 0:
- Node may still be initializing — wait a few minutes
- Check internet connectivity
- Check firewall isn't blocking port 1634 (p2p port)
- See connectivity section below

## Step 3: Is the chain synced?

```bash
swarm-cli status
```

Watch the "Chainsync" section. It shows current vs. latest block and a gap, e.g. `Block: 45,299,125 / 45,299,131 (Δ 6)` — it never prints "synchronized". A small gap (roughly **Δ < 10**) means the node is caught up. A large/growing gap means it's still syncing (~5 minutes typical) or stuck.

If chain sync is stuck:
- **RPC rate-limited (429):** the default `https://xdai.fairdatasociety.org` can return HTTP 429 "Too Many Requests"; Bee does not retry and exits on the first 429 at startup. Restart with a fallback: `bee start ... --blockchain-rpc-endpoint https://rpc.gnosischain.com`. For swarm-cli commands, add `--json-rpc-url https://rpc.gnosischain.com`.
- Check the RPC endpoint is reachable: `curl -s https://xdai.fairdatasociety.org`
- Try an alternative RPC: `https://rpc.gnosischain.com` or `https://gnosis-rpc.publicnode.com`

## Step 4: Is the wallet funded?

```bash
curl -s localhost:1633/addresses | jq .ethereum
```

Check balances:

```bash
curl -s http://localhost:1633/wallet | jq
```

Or:

```bash
swarm-cli status
```

If xDAI or xBZZ is zero, fund the wallet — see `/setup-bee` for funding options.

**Note:** If the node was left unfunded too long after first start, it may have shut itself down. Fund and restart.

## Step 5: Are stamps usable?

```bash
swarm-cli stamp list
```

Or:

```bash
curl -s http://localhost:1633/stamps | jq '.stamps[] | {batchID, usable, batchTTL, depth}'
```

Common stamp issues:
- **No stamps** → buy one: see `/stamps`
- **`usable: false`** → stamp is still propagating (wait a few minutes after purchase) or has expired
- **`batchTTL: 0`** → stamp expired, buy a new one
- **Stamp full** (immutable) → buy a new one or dilute: `swarm-cli stamp dilute --depth <new-depth> --stamp <id>`

## Step 6: Upload failing?

**"stamp not usable"** → stamp hasn't propagated yet. Wait 2-3 minutes after buying.

**"insufficient funds"** → wallet needs more xBZZ. Fund via `/setup-bee`.

**Ultra-light node** → can't upload. Upgrade to light node (restart with `--swap-enable` and `--blockchain-rpc-endpoint`).

**"act history not found"** → wrong history address for ACT upload. Double-check the history reference.

## Step 7: Download failing?

**"not found"** → content may have expired (stamp TTL ran out), reference is wrong, or content was uploaded with ACT and you're missing the ACT flags.

**Check retrievability:**

```bash
curl "http://localhost:1633/stewardship/<reference>"
```

Returns `isRetrievable: true/false`.

## Connectivity Issues

If the node has zero or few peers, check network connectivity.

### Check your public IP

```bash
curl icanhazip.com --ipv4
```

### Test p2p port (1634) reachability

```bash
nc -zv <YOUR_PUBLIC_IP> 1634
```

### Common fixes

| Problem | Fix |
|---------|-----|
| Behind NAT/router | Set up port forwarding: public 1634 → local IP:1634. Start with `--nat-addr <PUBLIC_IP>:1634` |
| Firewall blocking | Allow TCP+UDP on port 1634 |
| Docker | Ensure port 1634 is mapped (`-p 1634:1634`) |
| Only outgoing works | Bee can function with outgoing only, but performance is reduced |

### Test connectivity step by step

```bash
# 1. Localhost
nc -zv 127.0.0.1 1634

# 2. Local network (your private IP)
nc -zv 192.168.x.x 1634

# 3. Public IP
nc -zv <PUBLIC_IP> 1634
```

If step 1 fails → OS firewall or port conflict.
If step 2 fails → machine firewall.
If step 3 fails → router/ISP firewall or port forwarding missing.

## Quick Health Check (all at once)

Run these in parallel to get a full picture:

```bash
curl -s http://localhost:1633/status | jq
curl -s http://localhost:1633/topology | jq '{connected, population, depth}'
curl -s http://localhost:1633/wallet | jq
curl -s http://localhost:1633/stamps | jq '.stamps[] | {batchID, usable, batchTTL}'
swarm-cli status
```

## Common Bee API Error Codes

Observed on Bee 2.7.x–2.8.x:

| Code | Meaning | Fix |
|------|---------|-----|
| 400 | Bad request / missing required header (e.g. no `swarm-postage-batch-id`) → "invalid header params: want required" | Include and correctly format the stamp header and other required params |
| 404 | Content not found **or** stamp batch not found ("batch with id not found") | Content expired, wrong reference, missing ACT flags, **or** an invalid/unknown stamp ID |
| 422 | Unprocessable entity | Check parameter types (e.g., batch ID format) |
| 500 | Internal server error | Check Bee logs, restart node |
| 503 | Node not ready (still syncing) | Wait for chain sync to complete (up to ~30s right after start) |

> **Note:** A missing/insufficient stamp does **not** return 402 in current Bee — a missing stamp header returns **400** and an invalid stamp ID returns **404**. (402 may appear on older Bee versions or specific scenarios; treat stamp problems as 400/404.)

## Security Reminder

- API port (1633) should **never** be exposed to the public internet — always bind to `127.0.0.1`
- Only the p2p port (1634) should be publicly accessible

## Reference

- Connectivity guide: https://docs.ethswarm.org/docs/bee/installation/connectivity
- Bee API: https://docs.ethswarm.org/api/
- swarm-cli: https://github.com/ethersphere/swarm-cli
- Discord support: https://discord.gg/hyCr9BMX9U
