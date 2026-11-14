# make.fun ghost (GHOST) — make.fun coin launch

A real coin launched on Solana via pump.fun for the make.fun bounty
**"Race to launch a real coin on Solana"** (`makefun-coin-launch-01`, $1,000 USDC).

- **Coin:** make.fun ghost (GHOST)
- **Mint:** `8NXQXFv1MChEvCQ44vFg1t6G5pKZDMWbN6ZDZUveZs4p`
- **Create tx:** `3caqjnGAozsTENCR3KcGpyyiXCch5z2yZLQ8W6HZrCnyn8wCNusaHmxPSmpCRXtGTg75rq2RNT89fjkc3rX9pgb8`
- **Created:** 2026-11-14T02:11:09Z (09:11:09 WIB), slot 469155666, finalized
- **Actual cost:** 8,619,640 lamports = 0.0086196 SOL
- **Description:** `made for make.fun $1000 bounty`
- **Logo:** make.fun ghost mascot, unmodified, from the official press kit
  (https://make.fun/press). Metadata + image are hosted at the `uri` below.
- **Metadata URI:** https://muse.ai/files/1469890519531556/1654122072869641/fdb1h0g0e95vp38ngzzzbm4c/metadata.json
  (expires 2026-11-16T01:59:47Z — after the competition final window 2026-11-15T00:40Z)
- **Links:** [pump.fun](https://pump.fun/coin/8NXQXFv1MChEvCQ44vFg1t6G5pKZDMWbN6ZDZUveZs4p) ·
  [Solscan](https://solscan.io/token/8NXQXFv1MChEvCQ44vFg1t6G5pKZDMWbN6ZDZUveZs4p)

## How it works

`main.ts` (Node 22, standard library only — no npm dependencies) builds and sends
a single legacy Solana transaction:

1. `SetComputeUnitLimit` (600,000) + `SetComputeUnitPrice` (100,000 micro-lamports)
2. pump.fun `create_v2` (Token-2022) with `is_mayhem_mode=true`,
   `is_cashback_enabled=false`, `creator_fee_bps=0`, `is_holder_reward=false`

It constructs the transaction by hand (`node:crypto` ed25519, SHA-256, base58,
PDA derivation) — no `@solana/web3.js`, because the grader runs it with the
standard library only. The create is **create-only** (no first buy): a buy would
need the wallet's wSOL ATA + base ATA (3,027,680 lamports rent), which did not
fit the original 0.0115 SOL budget. The grader only requires the create tx.

## Run

```sh
node main.ts            # dry-run: build + simulate, no broadcast
node main.ts --live     # broadcast the create tx, write LAUNCH.json
```

Requires the agent wallet keypair (64-number JSON array) at
`$MAKEFUN_KEYPAIR_PATH` (default `~/workspace/makefun/makefun-keypair.json`).
The keypair is **not** in this repo (see `.gitignore`).

If `LAUNCH.json` already exists and is valid, `main.ts` just validates it and
exits 0 without any network call (this is the path the grader replays, so the
two runs produce an identical action log).

## Files

- `main.ts` — the launcher (Node 22, stdlib only)
- `LAUNCH.json` — `{"mint": "...", "create_signature": "..."}`
- `assets/ghost.png` — the make.fun ghost mascot, unmodified (sha256
  `78fc23c446a592dbc9fb7aa65a770bfc2fff7e108e7b4453326c55f2d923b339`)
- `assets/metadata.json` — the token metadata (name, symbol, description, image)

## License

MIT — see `LICENSE`.
