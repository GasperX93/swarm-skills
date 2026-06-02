# Postage Stamps — Reference

Detailed reference for stamp management beyond the buy/size happy path in `SKILL.md`. Load this when the user needs the bee-js utility helpers, wants to manage an existing stamp (top up, dilute, inspect), or needs to check content retrievability.

## bee-js Utility Functions

All stamp helpers live under the `Utils` namespace in bee-js 12.x — they are **not** top-level exports. (Several were renamed from earlier versions; the old names below no longer exist.)

```javascript
import { Utils, Size, Duration } from '@ethersphere/bee-js'

Utils.getDepthForSize(Size.fromMegabytes(100))        // Size → minimum depth (was getDepthForCapacity)
Utils.getStampEffectiveBytes(depth)                   // depth → effective storable bytes
Utils.getStampTheoreticalBytes(depth)                 // depth → max theoretical bytes (was getStampMaximumCapacityBytes)
Utils.getStampCost(depth, amount)                     // depth + amount → cost (BZZ object)
Utils.getStampUsage(utilization, depth, bucketDepth)  // how full a stamp is
Utils.getStampDuration(amount, pricePerBlock, blockTime)   // amount → Duration (was getStampTtlSeconds)
Utils.getAmountForDuration(Duration.fromDays(30), pricePerBlock, blockTime)  // Duration → amount (was getAmountForTtl)
```

`pricePerBlock` is the network's `currentPrice` (from `GET /chainstate`); `blockTime` is `5` seconds on Gnosis Chain.

## Manage Stamps

### List all stamps

```bash
swarm-cli stamp list
```

Or via API:

```bash
curl -s http://localhost:1633/stamps | jq
```

### Check a specific stamp

```bash
swarm-cli stamp show <stamp-id>
```

### Top up (extend duration)

```bash
swarm-cli stamp topup --stamp <stamp-id> --amount <additional-amount>
```

Via API:

```bash
curl -X PATCH "http://localhost:1633/stamps/topup/<batchID>/<amount>"
```

Via bee-js:

```javascript
await bee.topUpBatch(batchId, '40053139205')
```

### Dilute (increase capacity)

Increase depth to store more data. Dilution alone decreases TTL — combine with topup to maintain duration.

```bash
swarm-cli stamp dilute --depth <new-depth> --stamp <stamp-id>
```

Via API:

```bash
curl -s -X PATCH http://localhost:1633/stamps/dilute/<batchID>/<newDepth>
```

Via bee-js:

```javascript
await bee.diluteBatch(batchId, 24)
```

## Check Content Retrievability

```bash
curl "http://localhost:1633/stewardship/<reference>"
```

Returns `isRetrievable: true/false`. If false and content is pinned locally:

```bash
curl -X PUT "http://localhost:1633/stewardship/<reference>"
```

## Reference

- Stamp batches: https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch
- bee-js docs: https://bee-js.ethswarm.org/docs/
- Bee API: https://docs.ethswarm.org/api/
- swarm-cli: https://github.com/ethersphere/swarm-cli
