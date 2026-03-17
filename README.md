# Where's My Money? — Bitcoin Ordinals UTXO Inspector

A single-file, no-install Bitcoin wallet scanner that shows you exactly what is inside every UTXO — inscriptions, runes, stamps, rare sats, and more — so you never accidentally destroy an Ordinal by spending it as plain BTC.

**Live tool:** open `index.html` in your browser. No server required (serve locally for full CORS support).

---

## ⚠️ Use UniSat for Merge / Send / Split

**Just use UniSat.** It is the only wallet that fully supports the PSBT signing this tool needs.

Xverse and OP_NET can scan your UTXOs, but cannot sign transactions — so Merge, Send, and Split are disabled for them. If you currently use Xverse or OP_NET, import your seed phrase or WIF private key into UniSat to use all features.

---

## Why this tool exists

Most Bitcoin wallets show you a balance. This tool shows you what is *inside* that balance — which UTXOs are safe to spend, which ones carry inscriptions, which ones are fat (holding more BTC than necessary), and exactly how much value is locked up and why.

---

## Features

### Scanning & Inspection
- **Multi-address support** — scan Taproot (`bc1p`), Native SegWit (`bc1q`), and Nested SegWit (`3…`) addresses
- **All inscriptions per UTXO** — shows every inscription on a UTXO, not just the first one
- **Sat rarity** — detects uncommon, rare, epic, legendary, and mythic sats
- **Satributes** — pizza sats, nakamoto sats, block 9, palindromes, vintage, and more
- **Token detection** — Runes, Stamps (SRC-20), Atomicals, and cBRC-20 per UTXO
- **Ordinal size badges** — Normal / Fat (5k–20k sats) / Whale (20k+ sats)
- **UTXO Age** — shows how many days ago each UTXO was created
- **Dust Alert** — flags UTXOs where the network fee to spend them exceeds their value
- **Safe to Spend checker** — green ✓ or red ⚠ indicator on every UTXO in the inspector panel

### Portfolio Overview
- **Total portfolio value** in USD and BTC (live price from mempool.space)
- **Locked in Ordinals** — USD value of sats stuck in inscribed UTXOs
- **Lost Value Meter** — visual bar showing what percentage of your total value is trapped in Fat/Whale Ordinals, recoverable via Split

### Transactions
All transactions are built as PSBTs and signed by your wallet — private keys never leave the wallet.

| Feature | What it does |
|---|---|
| **Merge** | Combine multiple junk inscription UTXOs into one clean output, recovering locked sats |
| **Send** | Send any selection of UTXOs to any address |
| **Split** | Separate a single inscription onto a 546 sat output and return excess sats as change |

- Correct fee calculation for all address types (P2TR, P2WPKH, P2SH)
- Service fee: 500–3,000 sats depending on number of UTXOs
- Supported wallets: **UniSat** (Xverse and OP_NET are scan-only — see note at top)

### Tools & Utilities
- **Historical Fee Chart** — 24h or 1-week fee rate history so you pick the best time to merge
- **Merge Calculator** — shows vB saved per future transaction and break-even point after merging
- **Inscription Floor Price** — fetches collection floor price via Satflow when available
- **Mempool Monitor** — toast notification that polls every 30 seconds until your transaction confirms
- **Tax Export** — CSV with address, date acquired, days held, current USD value, and token info
- **Standard CSV Export** — all UTXOs with inscription IDs, rarity, tokens, and USD value
- **Filter bar** — filter by: All / Clean / Inscribed / Runes / Fat / Whale / Dust Risk
- **Sort** — by value or vout, ascending or descending
- **BTC price** — live from mempool.space, shown in header and per UTXO

### Wallet Connection
Connecting a wallet is **optional for scanning** — just paste your address to inspect.
A wallet connection is required for Merge, Send, and Split.

---

## How to use

### Scanning (no wallet needed)
1. Open `index.html` in your browser
2. Paste a `bc1p`, `bc1q`, or `3…` Bitcoin address
3. Click **Analyze**
4. The tool fetches UTXOs from mempool.space and inscriptions from ordinals.com
5. Click any row to open the inspector panel for that UTXO

### Merging junk inscriptions
1. Connect **UniSat** (Xverse and OP_NET are scan-only)
2. Check the boxes next to UTXOs you want to merge (or click **Select Junk**)
3. Click **Merge Selected**
4. Review the transaction preview and confirm
5. Sign in your wallet

### Splitting a Fat Ordinal
1. Click on any inscribed UTXO with more than 1,800 sats
2. Click **✂ Split** in the inspector panel
3. Enter the inscription destination address and your change address
4. Confirm the fee preview and click **Sign & Split**

---

## Data sources

| Data | Source |
|---|---|
| UTXOs | [mempool.space](https://mempool.space) |
| Inscriptions & previews | [ordinals.com](https://ordinals.com) |
| Sat rarity & metadata | [ordinals.com](https://ordinals.com) |
| Runes | [mempool.space](https://mempool.space) |
| Stamps | [stampchain.io](https://stampchain.io) |
| Atomicals | [atomicals.xyz](https://atomicals.xyz) |
| BTC price & fee rates | [mempool.space](https://mempool.space) |
| Floor prices | [satflow.com](https://satflow.com) |

---

## Running locally

For full functionality (ordinals.com CORS), serve the file via a local server:

```bash
npx serve .
```

Then open `http://localhost:3000` in your browser.

Opening `index.html` directly via `file://` works for scanning but may hit CORS limits on some inscription previews.

---

## Technical notes

- **Single file** — everything in one `index.html`, no dependencies, no build step
- **No backend** — all API calls go directly to public APIs from the browser
- **PSBT v0** — transactions are built manually in pure JavaScript using the PSBT binary format
- **Address types** — correct vbyte calculations per address type:
  - P2TR input: 57.5 vB, output: 43 vB
  - P2WPKH input: 68 vB, output: 31 vB
  - P2SH input: 91 vB, output: 32 vB
- **Ordinal theory** — first-in-first-out sat tracking, inscriptions detected at their actual sat offset within the UTXO (not always offset 0); gap outputs are automatically calculated when needed

---

## FAQ

See the built-in **FAQ** button in the tool for answers to common questions:
- Why do I have 7 inscriptions in one UTXO?
- What happens if I send an Ordinal to an exchange?
- What is a Fat Ordinal?
- Why does a Fat Ordinal stay fat after sending?
- What are Bitcoin Stamps?
- Why is the minimum inscription output 546 sats?
- And more…

---

## Built with vibecoding

This tool was built entirely through conversation with [Claude](https://claude.ai) — no manual coding. Every feature, bug fix, and improvement was described in plain language and implemented through dialogue.

---

## License

MIT — do whatever you want with it.
