---
name: act
description: Encrypt data on Swarm and control who can read it with ACT (Access Control Trie) — per-account access via Ethereum public keys, grantee lists, and add/revoke of access, using bee-js or swarm-cli. Use for private files, paid or subscriber-only content, or team-restricted data where only authorized accounts can decrypt.
user-invocable: true
---

# Access Control (ACT)

Guide a developer through encrypting data on Swarm and controlling who can read it. ACT (Access Control Trie) lets you define per-account read permissions using Ethereum public keys.

## Before Starting (run immediately)

Run these checks now and **narrate each one in a short line** — say what you're checking, run it (don't paste the command), then report the result. Don't pause for confirmation; these are read-only checks.

1. **Say "Checking your Bee node…"**, then run:
   ```bash
   curl -s http://localhost:1633/status | jq .beeMode
   ```
   Reachable → "✓ Node is up (light mode)." | Fails → "✗ No Bee node running." and route to `/setup-bee`.

2. **Say "Checking for a usable postage stamp…"**, then run:
   ```bash
   curl -s http://localhost:1633/stamps | jq '.stamps[] | select(.usable==true) | {batchID, depth, batchTTL}'
   ```
   Found → "✓ Found a usable stamp." and proceed. | None → "✗ No usable stamp." and route to `/stamps`.

## What to Ask

1. **What data are you protecting?** (files, website, app data)
2. **Who should have access?** (specific Ethereum public keys)
3. **swarm-cli or bee-js?**

## Prerequisites

- For swarm-cli: `npm install -g @ethersphere/swarm-cli`
- For bee-js: `npm install @ethersphere/bee-js`

## How ACT Works

- Data is uploaded encrypted — only authorized accounts can decrypt
- Access is controlled via a **grantee list** of Ethereum public keys
- The publisher manages the list: add or revoke access at any time
- Unauthorized download attempts return "not found" — the data is invisible without access
- The publisher always has access

## Via swarm-cli

### Upload with ACT

```bash
swarm-cli upload test.txt --act --stamp <BATCH_ID>
```

First upload — omit `--act-history-address` to create a new history. The response returns:
- Encrypted Swarm reference
- History reference

> **Warning:** Save the history reference securely. Losing it means **permanent, irrecoverable loss** of access to your encrypted data. There is no recovery mechanism.

For subsequent uploads to the same history:

```bash
swarm-cli upload test.txt --act --stamp <BATCH_ID> --act-history-address <HISTORY_REF>
```

### Download with ACT

```bash
swarm-cli download <SWARM_HASH> output.txt \
  --act \
  --act-history-address <HISTORY_REF> \
  --act-publisher <PUBLIC_KEY>
```

- `--act-publisher` — the uploader's public key (needed for decryption)
- `--act-timestamp` — optional, defaults to current time. Use to access a specific version.

**Without the ACT flags, the download will fail with "not found".**

### Create a grantee list

Create a JSON file with the public keys of accounts that should have access:

```json
{
  "grantees": [
    "03ec55e9fb2aefb8600f69142abaad79311516c232b28919d66efb4d41bce15bfa",
    "03fdcab22b455ce08a481d929a4cb9f447752545818eded1ad1785c51581e822c6"
  ]
}
```

```bash
swarm-cli grantee create grantees.json --stamp <BATCH_ID>
```

Returns a grantee reference and history reference — save both.

### Update access (add/revoke)

Create a patch JSON:

```json
{
  "add": ["03fdcab22b455ce08a481d929a4cb9f447752545818eded1ad1785c51581e822c6"],
  "revoke": ["03ec55e9fb2aefb8600f69142abaad79311516c232b28919d66efb4d41bce15bfa"]
}
```

```bash
swarm-cli grantee patch grantees-patch.json \
  --reference <GRANTEE_REF> \
  --history <GRANTEE_HISTORY_REF> \
  --stamp <BATCH_ID>
```

**Note:** Wait at least 1 second between grantee list updates — updating within the same second causes an error.

### View grantee list

```bash
swarm-cli grantee get <GRANTEE_REF>
```

## Via bee-js

### Upload with ACT

```javascript
import { Bee } from '@ethersphere/bee-js'

const bee = new Bee('http://localhost:1633')

// Upload a file with ACT encryption enabled
const result = await bee.uploadFile(batchId, 'Secret data', 'secret.txt', {
  act: true
})

console.log('Encrypted reference:', result.reference.toHex())

// historyAddress is an Optional<Reference> in bee-js 12.x — unwrap it before use.
const historyRef = result.historyAddress.getOrThrow()   // or: result.historyAddress.value
console.log('History address:', historyRef.toHex())
// WARNING: Save both — losing the history address means permanent loss of access to encrypted data
```

### Download with ACT

```javascript
import { Bee } from '@ethersphere/bee-js'

const bee = new Bee('http://localhost:1633')

// The publisher key is the uploader's public key — get it as a PublicKey object and
// pass it directly (do not stringify it). historyRef is the unwrapped Reference from upload.
const { publicKey } = await bee.getNodeAddresses()

const file = await bee.downloadFile(reference, 'secret.txt', {
  actHistoryAddress: historyRef,   // Reference (unwrapped from result.historyAddress)
  actPublisher: publicKey          // PublicKey object from getNodeAddresses()
})

console.log('Decrypted:', file.data.toUtf8())
// Without ACT parameters, this returns "not found"
```

> **swarm-cli quirk:** in testing, `swarm-cli download <ref> --act --act-history-address <hist> --act-publisher <pk>` can return 404 even with correct values, while the equivalent Bee HTTP API call (same headers) returns 200. If the CLI download fails, retry via the API or bee-js.

### Manage grantees

```javascript
import { Bee } from '@ethersphere/bee-js'

const bee = new Bee('http://localhost:1633')

// Create grantee list
const granteeResult = await bee.createGrantees(batchId, [
  '03ec55e9fb2aefb8600f69142abaad79311516c232b28919d66efb4d41bce15bfa'
])
console.log('Grantee ref:', granteeResult)

// Get current grantees
const grantees = await bee.getGrantees(granteeReference)
console.log('Grantees:', grantees)

// Update access — add and revoke
await bee.patchGrantees(batchId, granteeReference, historyReference, {
  add: ['03fdcab22b455ce08a481d929a4cb9f447752545818eded1ad1785c51581e822c6'],
  revoke: ['03ec55e9fb2aefb8600f69142abaad79311516c232b28919d66efb4d41bce15bfa']
})
```

## Use Cases

- **Private file sharing** — share documents with specific accounts
- **Paid content** — grant access after payment
- **Subscriber-only access** — manage a grantee list for subscribers
- **Team collaboration** — restrict project data to team members

## Important Notes

- Grantees are identified by their **Ethereum public keys** (compressed, 33 bytes hex)
- Only the publisher (uploader) can manage the grantee list
- Revoking access prevents future decryption but doesn't delete already-downloaded data
- Invalid history addresses return "not found" errors
- Wait at least 1 second between grantee list updates

## If Something Goes Wrong

| Error | Fix |
|-------|-----|
| "not found" on download | Missing ACT flags, wrong history address, or access revoked |
| "act: invalid history" | Wrong history address — double-check reference |
| "stamp not usable" | Wait 2-3 minutes after buying |
| 1-second update error | Wait at least 1 second between grantee list updates |
| Other errors | Route to `/troubleshoot` |

## Reference

- ACT guide: https://docs.ethswarm.org/docs/develop/act
- ACT concepts: https://docs.ethswarm.org/docs/develop/access-the-swarm/access-control
- bee-js docs: https://bee-js.ethswarm.org/docs/
- swarm-cli: https://github.com/ethersphere/swarm-cli
