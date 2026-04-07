# 💣 RugPot Inspector & Recovery

**A fully client-side, open-source tool for inspecting and interacting with the RugPot protocol on Solana.**

> Zero backend. Zero tracking. Zero analytics. Just you, your browser, and the blockchain.

---

## Short Description

**RugPot Inspector & Recovery** is a single-file, self-hosted dashboard that reads RugPot protocol state directly from the Solana blockchain. Inspect rounds, audit on-chain config, view wallet positions, and recover funds — all from your browser with no backend, no accounts, and no third-party dependencies beyond a Solana RPC endpoint.

---

## Features

### 🔍 Inspect
- **Round Scanner** — Scan all rounds, a specific range, or just the latest. Binary search finds the highest round instantly.
- **On-Chain GameConfig** — View live protocol parameters: fee splits, bonding curve coefficients, probability tiers, boost tiers, authorities.
- **Wallet Positions** — Enter any wallet address to see shares, cost basis, claimable reflections, and sell value across all rounds.
- **Vault Health** — Compare actual vault balance against on-chain obligations to verify protocol solvency.

### 💰 Recover
- **Claim Reflections** — Harvest pending reflection rewards from any round.
- **Sell Shares** — Sell positions back to the bonding curve during active rounds.
- **Claim Jackpot** — Claim jackpot winnings if you're the winner.
- **Close Positions** — Reclaim rent from finished rounds after all reflections are claimed.

### 🛠️ Tools
- **Bonding Curve Calculator** — Compute buy cost, sell refund, and price per share at any supply level.
- **CSV/JSON Export** — Export all scanned round and position data for offline analysis.
- **Multi-Explorer Links** — One-click links to Solscan, SolanaFM, or Solana Explorer for any account or transaction.

### 🔒 Trust Model
- **100% client-side** — Runs entirely in your browser. No server, no database, no API calls except to your chosen Solana RPC.
- **Single HTML file** — Download it, open it, inspect the source. That's it.
- **No tracking** — Zero analytics, zero cookies, zero fingerprinting.
- **Self-hostable** — Host it on your own domain, run it from a USB drive, or open it as a local file.
- **Open source** — Every line of code is visible and auditable.

---

## Supported Wallets

- Phantom
- Solflare
- Backpack
- Any wallet-standard compatible Solana wallet

---

## Usage

1. **Download** `index.html` (single file, ~50KB)
2. **Open** it in any modern browser
3. **Configure** your RPC endpoint (defaults to a free public RPC)
4. **Scan** rounds to view protocol state
5. **Connect wallet** (optional) to claim, sell, or close positions

No installation. No dependencies. No build step.

---

## RPC Endpoints

The dashboard works with any Solana RPC endpoint. Built-in presets include:

| Provider | URL | Notes |
|----------|-----|-------|
| PublicNode | `https://solana-rpc.publicnode.com` | Free, CORS-friendly (default) |
| dRPC | `https://solana.drpc.org` | Free, CORS-friendly |
| Ankr | `https://rpc.ankr.com/solana` | Free, CORS-friendly |

For heavy scanning (all rounds), a premium RPC from Helius, QuickNode, or GetBlock is recommended.

---

## Security

- **Content Security Policy** — CSP meta tag restricts scripts to `self` and `esm.sh` only. No other external scripts can execute.
- **XSS protection** — All on-chain data is HTML-escaped before DOM insertion. URL parameters are sanitized to base58-only characters.
- **Scoped transaction approval** — Confirm dialogs use closure-scoped event listeners. Browser extensions cannot programmatically auto-approve transactions.
- **Slippage protection** — Sell transactions enforce a 2% max slippage (min_refund = 98% of estimated value).
- **Preflight simulation** — All transactions are simulated before submission and validated by the RPC before going on-chain.
- **No private key access** — Signing happens entirely in your wallet extension. The dashboard never touches keys.
- **Tabnapping prevention** — All external links use `rel="noopener noreferrer"`.
- **RPC trust warning** — Users are warned that a malicious RPC could return fake data.
- **Source auditable** — Right-click → View Source, or click "View Source" in the header.

---

## Privy Wallet Users (Social Login Recovery)

If you signed up on **rugpot.io** using email, Google, or another social login, your wallet is managed by [Privy](https://privy.io). This Inspector cannot connect to Privy wallets directly, but you can still recover your funds:

### Step 1 — Export your private key from Privy
1. Go to [rugpot.io](https://rugpot.io) and log in with your social account
2. Click your wallet address or profile icon
3. Look for **"Export Wallet"** or **"Wallet Settings"**
4. Follow the prompts to reveal your **private key** (a base58-encoded string)
5. **Copy it securely** — never share it with anyone or paste it in public

### Step 2 — Import into a wallet extension
1. Install [Phantom](https://phantom.app) or [Solflare](https://solflare.com) browser extension
2. Open the extension → **"Add / Connect Wallet"** → **"Import Private Key"**
3. Paste the private key you exported from Privy
4. Your wallet address and all positions will appear

### Step 3 — Use the Inspector
1. Open `index.html` in your browser
2. Click **Connect Wallet** and select Phantom or Solflare
3. Your RugPot positions will load — you can now claim, sell, or close positions

### ⚠️ Security Notes
- **Never share your private key** with anyone, including support staff
- After importing, your key exists in both Privy and your wallet extension — this is fine
- If you no longer trust Privy, you can transfer all assets to a fresh wallet after importing
- Delete the private key from clipboard/notes immediately after importing

### Can't export from rugpot.io?
You can also export your private key directly at [home.privy.io](https://home.privy.io) — log in with the same email or social account you used on rugpot.io, and export your embedded wallet from there.



---

## Technical Details

- **Program ID**: `552HLD8APrtVRHkRvgkKiZw48gsLdiTXC3SS5kDLd2ka`
- **Deserialization**: Zero-copy for `RoundState`, Borsh for `PlayerPosition` and `GameConfig`
- **PDA Seeds**: `round`, `player`, `vault`, `game_config`
- **Bonding Curve**: `price(s) = A·s + B` (scaled by 1e6)

---
