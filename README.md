<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:00B4D8,100:8B5CF6&height=200&section=header&text=Polymarket%20Trading%20System&fontSize=42&fontColor=ffffff&fontAlignY=38&desc=v1.7.2%20Final%20·%20Multi-Asset%20·%20Kelly%20Criterion%20·%20Edge%20Validated&descAlignY=58&descSize=16&descColor=c7d2fe" />

<img width="1920" height="1080" alt="Ekran görüntüsü 2026-04-16 230443" src="https://github.com/user-attachments/assets/1b8a98c7-1ab9-40c7-8a6b-7a25b32446a8" />


<br/>

[![Node](https://img.shields.io/badge/Node.js-18%2B-339933?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Status](https://img.shields.io/badge/Status-v1.7.2_Final-00B4D8?style=for-the-badge&logo=checkmarx&logoColor=white)](.)
[![Mode](https://img.shields.io/badge/Mode-PAPER_|_LIVE-F59E0B?style=for-the-badge&logo=bitcoin&logoColor=white)](.)
[![License](https://img.shields.io/badge/License-MIT-8B5CF6?style=for-the-badge&logo=opensourceinitiative&logoColor=white)](./LICENSE)
[![Tests](https://img.shields.io/badge/Tests-46%2F46_Passing-10B981?style=for-the-badge&logo=jest&logoColor=white)](.)

<br/>

[![BTC](https://img.shields.io/badge/BTC-F7931A?style=flat-square&logo=bitcoin&logoColor=white)](.)
[![ETH](https://img.shields.io/badge/ETH-627EEA?style=flat-square&logo=ethereum&logoColor=white)](.)
[![SOL](https://img.shields.io/badge/SOL-9945FF?style=flat-square&logoColor=white)](.)
[![DOGE](https://img.shields.io/badge/DOGE-C2A633?style=flat-square&logoColor=white)](.)
[![XRP](https://img.shields.io/badge/XRP-00AAE4?style=flat-square&logoColor=white)](.)

<br/><br/>

> **Probability > Price = Opportunity**
>
> *Architecture > Hype · Replay > Guessing · Edge Validation > Blind Trading*

<br/>

</div>

---

<div align="center">

## ⚡ What Is This?

</div>

A production-grade automated trading framework for **Polymarket binary prediction markets**, driven by crypto price movements. Not a script — a full decision infrastructure with calibrated probability estimation, Kelly-optimal position sizing, multi-layer risk protection, and a live web dashboard.

```
Spot Price + Orderbook  →  Decision Engine  →  Kelly Gate  →  Risk Gate  →  Order
                                ↓
                        Replay & Analysis  →  Edge Verdict
```

---

<div align="center">

## 🗺️ System Architecture

</div>

```
┌─────────────────────────────────────────────────────────────────────┐
│                         DATA LAYER                                   │
│   Binance / OKX / Coinbase ──▶ Spot Intelligence                    │
│   Polymarket CLOB ───────────▶ Orderbook (WebSocket)                │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│                       DECISION ENGINE                                │
│   Direction Signal ──▶ Persistence ──▶ Calibration ──▶ EV + Kelly  │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│                         RISK GATES                                   │
│   Drawdown Cap · Loss Streak · Cooldown · Phase Filter · Idempotency│
└──────────────────────────────┬──────────────────────────────────────┘
                               │
               ┌───────────────┼───────────────┐
               ▼               ▼               ▼
          NO_TRADE          PAPER            LIVE
        Signal Log      Paper Journal    Order Verify
                               │               │
                               └───────┬───────┘
                                       ▼
                               Trade Journal ──▶ Replay Report ──▶ Dashboard
```

---

<div align="center">

## ✨ Features

</div>

<table>
<tr>
<td width="50%" valign="top">

### 🧠 Decision Engine
Mathematical trade decisions powered by `pSide`, `Edge`, Expected Value, and Kelly Criterion. Market intelligence scores are **diagnostic only** — they never inflate the `pSide` used in EV/Kelly.

### 📡 Live Data Feeds
Chainlink spot + Binance / OKX / Coinbase + Polymarket CLOB WebSocket. Orderbooks older than **15 seconds** are automatically blocked.

### 💰 Fractional Kelly Stake
Dynamic position sizing proportional to portfolio equity — not a fixed lot. In AUTO mode order size comes directly from `kelly.stakeUsd`.

### 🎯 Probability Calibration
Bucket-based **smoothed win rate** calibration from real historical trade data. Honest heuristic fallback when sample is insufficient.

</td>
<td width="50%" valign="top">

### 🛡️ Multi-Layer Risk
Drawdown cap, loss streak lock, idempotency guard, cooldown, late-phase filter, bad-hour filter — **seven independent layers**.

### 🔬 Real vs Sim PnL
Eliminates fake-profit illusions. `realPnlUsd` and `simulatedPnlUsd` are tracked and displayed separately.

### 🗂️ Multi-Asset Isolation
BTC, ETH, SOL, DOGE, XRP each run with **independent state**, calibration tables, and idempotency stores. No cross-contamination.

### 📊 Replay & Edge Validation
Answers the question: *does this system actually produce edge?*
Verdict: `EDGE_PRESENT` / `NO_PROVEN_EDGE`.

</td>
</tr>
</table>

---

<div align="center">

## 🚀 Quick Start

</div>

```bash
# Clone
git clone https://github.com/<user>/polymarket-system.git
cd polymarket-system

# Install
npm install

# Configure
cp .env.example .env

# Generate Polymarket API credentials
npm run clob:creds

# Verify everything works
npm test

# Launch
npm run supervised
```

```
Dashboard → http://localhost:3000
```

---

<div align="center">

## 🔑 API Key Generation

</div>

Polymarket CLOB uses **two-layer authentication**:

| Layer | Description |
|---|---|
| **L1 — EOA** | EVM private key signing via `ethers.Wallet` |
| **L2 — API Key** | `POLY_API_KEY` / `POLY_API_SECRET` / `POLY_API_PASSPHRASE` |

```bash
npm run clob:creds
# Outputs ready-to-paste .env values
```

```env
POLY_SIGNER=0x...
POLY_API_KEY=...
POLY_API_SECRET=...
POLY_API_PASSPHRASE=...
```

> **Note:** L1 auth always uses `signatureType=0`. Even with Magic/proxy accounts, key derivation is signed by the EOA.

| `signatureType` | Usage |
|---|---|
| `0` | Direct EOA — `POLY_FUNDER` not needed |
| `1` | Magic / email proxy — `POLY_FUNDER` required |

---

<div align="center">

## ⚙️ Environment Variables

</div>

### 🔴 Required

```env
PRIVATE_KEY=               # EVM wallet private key — never share or commit
POLY_API_KEY=              # CLOB L2 API key
POLY_API_SECRET=           # CLOB L2 secret
POLY_API_PASSPHRASE=       # CLOB L2 passphrase
ADMIN_API_KEY=             # Dashboard admin password (≥32 chars)
POLY_SIGNATURE_TYPE=1      # 0 = EOA  |  1 = Magic/proxy
POLY_FUNDER=               # Required only when SIGNATURE_TYPE=1
```

### 🟡 Kelly & Decision Tuning

```env
# ── Position Sizing ────────────────────────────────────────
KELLY_MIN_PROBABILITY=0.56          # Minimum win probability
KELLY_MIN_EDGE=0.045                # Minimum edge (pSide - ask)
KELLY_MIN_EV_USD_PER_DOLLAR=0.035   # Minimum EV per dollar
KELLY_FRACTION=0.10                 # Fractional Kelly multiplier
KELLY_MAX_FRACTION=0.015            # Max portfolio risk per trade
KELLY_MIN_STAKE_USD=1               # Minimum order size ($)
KELLY_MAX_STAKE_USD=15              # Maximum order size ($)

# ── Decision Thresholds ────────────────────────────────────
DECISION_CONF_THRESHOLD=0.55        # Minimum confidence score
DECISION_MIN_EDGE=0.025             # Minimum edge floor
DECISION_PERSIST_TICKS=6            # Ticks for directional stability
DECISION_LATE_PHASE_MAX_SECS_LEFT=75
DECISION_BAD_HOURS_UTC=0,1,2,3      # UTC hours to block trading
DECISION_COOLDOWN_MS=7000           # Minimum ms between trades
```

### 🔵 Security & Rate Limiting

```env
ADMIN_IP_ALLOWLIST=127.0.0.1,::1
API_RATE_WINDOW_MS=60000
API_RATE_MAX_REQ=100
MAX_ORDER_USD=50
IDEMPOTENCY_TTL_MS=120000
```

---

<div align="center">

## 🧠 Decision Engine Deep Dive

</div>

`src/engine/decisionEngine.js` — `evaluateDecision(state)`

### Pipeline

```
┌─────────────────┐   ┌────────────────┐   ┌─────────────┐   ┌──────────────┐
│ Data Validation │──▶│Direction Signal│──▶│ Persistence │──▶│ Price & Edge │
└─────────────────┘   └────────────────┘   └─────────────┘   └──────┬───────┘
                                                                      │
┌─────────────────┐   ┌────────────────┐   ┌─────────────┐          │
│  FINAL DECISION │◀──│  Confidence    │◀──│  Risk Gates │◀─────────┘
└─────────────────┘   └────────────────┘   └──────┬──────┘
                                                   │
                                           ┌───────┴──────┐
                                           │  Kelly Gate  │
                                           └──────────────┘
```

### Core Formulas

```
deltaBps  =  ((spot - ptb) / ptb) × 10000

pUp       =  0.5 + 0.5 × clamp(0.6 × deltaRaw + 0.4 × bookRaw, −1, 1)
pSide     =  rawDir == UP ? pUp : (1 − pUp)
edge      =  pSide − chosenAsk
```

### Confidence Score Composition

```
conf  =  rawConf           × 0.68
      +  momentumAlignment × 0.08
      +  ptbPressure       × 0.07
      +  bookClarity       × 0.06
      +  askValue          × 0.05
      +  entryQuality      × 0.04
      ±  bookDir bonus / penalty
      −  lossPenalty       (capped at MAX_LOSS_PENALTY)
```

### Dynamic Thresholds by Phase

| Condition | `confThreshold` | `minEdge` |
|---|---|---|
| EARLY phase, cadence not armed | `+ 0.08` | `+ 0.02` |
| Cadence armed (idle N rounds) | `− 0.05` | `− 0.008` |
| Normal | base | base |

### 🚫 Block Codes

| Code | Trigger |
|---|---|
| `MISSING_PTB` | PriceToBeat unavailable |
| `MISSING_SPOT` | Spot price unavailable |
| `MISSING_ASK` | YES or NO ask unavailable |
| `STALE_ORDERBOOK` | Orderbook > 15 seconds old |
| `ROUND_LOCKED` | Trade already placed this round |
| `LATE_PHASE` | ≤ 75 seconds remaining |
| `BAD_HOUR` | Blocked UTC hour (default: 0–3) |
| `FLAT_DELTA` | Spot equals PTB, no direction |
| `EARLY_BOOK_MISMATCH` | Book conflicts with signal direction |
| `PERSISTENCE_PENDING` | Direction not stable enough |
| `ASK_OUT_OF_RANGE` | Ask outside allowed band |
| `EDGE_TOO_SMALL` | `pSide − ask < minEdge` |
| `CONF_TOO_LOW` | Confidence below threshold |
| `LOSS_STREAK_LOCK` | Consecutive loss limit exceeded |
| `DAILY_LOSS_CAP` | 24h drawdown limit exceeded |
| `COOLDOWN_ACTIVE` | Inter-trade cooldown not elapsed |
| `ENGINE_ERROR` | Unexpected error |

---

<div align="center">

## 📐 Kelly Criterion & Stake Sizing

</div>

`src/engine/kellyDecision.js`

### Formulas

```
winProfitPerDollar  =  (1 − ask) / ask
evPerDollar         =  p × winProfit − (1 − p)
fullKelly           =  (b × p − q) / b          [b = winProfit, q = 1−p]
fracKelly           =  fullKelly × KELLY_FRACTION
stakeFraction       =  clamp(fracKelly, minFraction, maxFraction)
stakeUsd            =  clamp(equity × stakeFraction, minStake, maxStake)
```

### Live Example

```
pSide = 0.62   ask = 0.50

winProfit  =  (1 − 0.50) / 0.50   =  1.00
EV/$       =  0.62 × 1.00 − 0.38  =  0.24   ✅
fullKelly  =  (1.00 × 0.62 − 0.38) / 1.00  =  0.24
fracKelly  =  0.24 × 0.10  =  0.024

equity = $1,000
stake  = $1,000 × 0.024 = $24  →  cap applied  →  $15 ✅
```

### Gate — All Four Must Pass

| Gate | Condition | Default |
|---|---|---|
| Probability | `p ≥ KELLY_MIN_PROBABILITY` | `0.56` |
| Edge | `edge ≥ KELLY_MIN_EDGE` | `0.045` |
| EV | `evPerDollar ≥ KELLY_MIN_EV_USD_PER_DOLLAR` | `0.035` |
| Kelly sign | `fullKelly > 0` | required |

> In AUTO mode, order size is sourced from `decision.components.kelly.stakeUsd`. `autoUsdMax` is a hard ceiling only.

---

<div align="center">

## 🎯 Probability Calibration

</div>

`src/engine/calibratedProb.js`

Raw heuristic `pUp` is calibrated against **bucket-level win rates** learned from historical trades.

### Bucket Dimensions

| Dimension | Bands |
|---|---|
| `askBand` | `<0.45` / `0.45–0.55` / `0.55–0.70` / `≥0.70` |
| `deltaBpsBand` | 7 bands: `<−50` → `≥50` |
| `imbalanceBand` | `<0.40` / `0.40–0.48` / `0.48–0.52` / `0.52–0.60` / `≥0.60` |
| `phase` | `EARLY` / `MID` / `LATE` |
| `hour (UTC)` | 0–23 |

### Smoothed Win Rate

```
smoothedWinRate  =  (wins + α × prior) / (trades + α)

α = 2   |   prior = 0.5   |   minSamples = 5
```

### Decision Tree

| Condition | Result |
|---|---|
| `DECISION_CALIBRATE_PROBS=0` | Raw heuristic |
| No bucket found | Raw heuristic (no bucket) |
| `trades < minSamples` | Raw heuristic (low sample) |
| Sufficient data | `smoothedWinRate` ✅ |

---

<div align="center">

## 🛡️ Risk Management

</div>

```
┌──────────────────────────────────────────────────────────────┐
│  LAYER 1 │ Loss Streak Lock     │ AUTO → MAN on streak limit  │
├──────────────────────────────────────────────────────────────┤
│  LAYER 2 │ Daily Drawdown Cap   │ Block when PnL24h too low   │
├──────────────────────────────────────────────────────────────┤
│  LAYER 3 │ Idempotency Guard    │ Same key = HTTP 409         │
├──────────────────────────────────────────────────────────────┤
│  LAYER 4 │ Cooldown             │ Min ms between trades       │
├──────────────────────────────────────────────────────────────┤
│  LAYER 5 │ Late Phase Block     │ ≤75s left = no entry        │
├──────────────────────────────────────────────────────────────┤
│  LAYER 6 │ Bad UTC Hours        │ Default: 0, 1, 2, 3         │
├──────────────────────────────────────────────────────────────┤
│  LAYER 7 │ One Trade / Round    │ No second order same slug   │
└──────────────────────────────────────────────────────────────┘
```

**Idempotency Guard detail:**
- Every buy request requires an `idempotency-key` header
- Duplicate key → `HTTP 409`, order never fires twice
- Keys auto-purge after TTL (default 120s)
- File-based lock (`idempotency_store.json.lock`) prevents race conditions

---

<div align="center">

## 🌐 REST API

</div>

### Authentication

```http
x-admin-key: <ADMIN_API_KEY>
# or
Authorization: Bearer <ADMIN_API_KEY>
```

### Public

| Method | Path | Description |
|---|---|---|
| `GET` | `/api/health` | System health |
| `GET` | `/api/ready` | Readiness check |
| `GET` | `/api/version` | Version info |
| `GET` | `/` | Dashboard UI |

### Admin Protected

| Method | Path | Description |
|---|---|---|
| `GET` | `/api/state` | Full system state |
| `GET` | `/api/signals` | Recent SignalTrace records |
| `GET` | `/api/trade-outcomes` | Real / paper trade results |
| `GET` | `/api/decision/evaluate` | Live decision simulation |
| `GET` | `/api/decision/config` | Active decision config |
| `PATCH` | `/api/decision/config` | Update parameters at runtime |
| `POST` | `/api/exec/live/buy` | Place a live order |
| `POST` | `/api/exec/paper/buy` | Place a paper order |
| `GET` | `/api/replay/report` | Edge / Kelly replay report |
| `GET` | `/api/analytics/*` | Advanced analytics |

```bash
# Simulate a decision right now
curl -H "x-admin-key: $ADMIN_API_KEY" \
  http://localhost:3000/api/decision/evaluate

# Tune parameters live, no restart
curl -X PATCH \
  -H "x-admin-key: $ADMIN_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"confThreshold": 0.60, "minEdge": 0.03}' \
  http://localhost:3000/api/decision/config
```

---

<div align="center">

## 📊 Dashboard

`http://localhost:3000`

</div>

| Panel | Content |
|---|---|
| **Round Sidebar** | Active slug, PriceToBeat vs Spot delta, phase, time remaining |
| **Orderbook** | YES/NO ask, imbalance, freshness status |
| **Decision Box** | Latest decision, confidence, Kelly stake, block reason |
| **Trade Outcomes** | WIN/LOSS/OPEN, real PnL — REAL vs SIM clearly separated |
| **Signal History** | All SignalTrace records with block reason breakdown |
| **Strategy Health** | Streak counter, drawdown, equity snapshot |
| **Replay Stats** | Edge bucket performance, win rate, verdict |
| **Alerts** | Audit log and critical system events |

> `v1.7.2` — Signal-only rows removed from Trade Outcomes. This panel is now execution-only.

---

<div align="center">

## 🗂️ Multi-Asset & Isolation

</div>

```
data/
├── BTC/
│   ├── decision_config.json
│   ├── events.db                    ← SQLite, round events
│   ├── trade_history.json
│   ├── round_history.json
│   ├── idempotency_store.json
│   ├── trade_execution_journal.json
│   └── exec_state.json
├── ETH/   SOL/   DOGE/   XRP/
│   └── (same structure, fully isolated)
```

**Isolation rules enforced at runtime:**

- Asset prefix validated on every trade record (`btc-`, `eth-`, …)
- Asset/slug mismatch → rejected at executor level
- Idempotency keys scoped to `asset + slug`
- Each asset calibrates from its own trade history only
- A BTC process **cannot** open an XRP-slug order

> BTC being profitable does **not** imply XRP has edge. Every asset is judged by its own statistics.

```bash
# Single-asset replay
ASSET=BTC npm run replay:report
```

---

<div align="center">

## 🔬 Replay & Analysis

</div>

```bash
npm run replay:report            # All assets
ASSET=ETH npm run replay:report  # Single asset

# Via API
curl -H "x-admin-key: $ADMIN_API_KEY" \
  http://localhost:3000/api/replay/report
```

| Field | Description |
|---|---|
| `totalSignals` | Total signal evaluations |
| `actionable` | BUY_UP / BUY_DOWN signals |
| `blocked` | Block count + breakdown by reason |
| `closedTrades` | Closed / WIN / LOSS / Win Rate |
| `realPnlUsd` | PnL from verified fills |
| `simulatedPnlUsd` | Simulation PnL for comparison |
| `edgeBuckets` | Performance segmented by edge range |
| `kellyStats` | Average stake, gate pass rate |
| `skippedOutcomes` | What happened in rounds where signals were skipped |
| `verdict` | `INSUFFICIENT_SAMPLE` / `EDGE_PRESENT_BUT_KEEP_MONITORING` / `NO_PROVEN_EDGE` |

```bash
npm run replay:metrics     # Detailed metrics
npm run replay:risk        # Risk profile
npm run replay:decisions   # Decision simulation
npm run replay:optimize    # Optimization report
npm run analyze:segments   # Trade segments
npm run policy:learn       # Policy learning
```

---

<div align="center">

## 🧪 Tests

</div>

```bash
npm test               # All unit tests
npm run test:live      # Live connection tests (API key required)
npm run test:all       # Unit + live
```

> Tests run sequentially with Node.js built-in runner (`--test-concurrency=1`)

<details>
<summary><b>📋 Full test coverage (click to expand)</b></summary>

<br/>

| File | Coverage |
|---|---|
| `decisionEngine.test.js` | Decision engine core scenarios |
| `decisionFinalCore.test.js` | Final decision core |
| `decisionHardening.test.js` | Edge cases and harsh conditions |
| `decisionCadence.test.js` | Cadence and relax logic |
| `autoKellyStake.test.js` | AUTO order size sourced from Kelly |
| `calibratedProb.test.js` | Calibration bucket tests |
| `calibratorReal.test.js` | Real calibrator integration |
| `advancedRisk.test.js` | Advanced risk calculations |
| `riskProfile.test.js` | Risk profile validation |
| `tradeService.test.js` | Trade service operations |
| `tradeServiceDuplicateGuard.test.js` | Duplicate trade protection |
| `tradeServiceSlugGuard.test.js` | Slug mismatch block |
| `tradeExecutorPersistentGuard.test.js` | Persistent idempotency guard |
| `idempotencyMiddleware.test.js` | Idempotency middleware |
| `orderbookPressure.test.js` | Orderbook pressure metrics |
| `liveSafety.test.js` | Live execution safety |
| `signalTrace.test.js` | SignalTrace construction |
| `roundResolver.test.js` | Round resolution |
| `runtimeSchema.test.js` | Runtime schema validation |
| `tradeSegmentSources.test.js` | Trade segment sources |
| `btcFeedFreshness.test.js` | BTC feed freshness check |
| `tradeSchema.test.js` | Trade schema validation |

</details>

---

<div align="center">

## 🔒 Security

</div>

### Pre-Publish Checklist

- [ ] `.env` in `.gitignore` — `PRIVATE_KEY` is **never** committed
- [ ] `.env.example` in repo with no real credentials
- [ ] `ADMIN_API_KEY` strong and unique (≥ 32 characters)
- [ ] `ADMIN_IP_ALLOWLIST` configured (empty = IP filter disabled)
- [ ] Service behind TLS-terminating reverse proxy
- [ ] `MAX_ORDER_USD` set to prevent runaway orders
- [ ] Polymarket API keys rotated periodically
- [ ] `npm run security:smoke` and `npm run clob:smoke` pass

### Protected Route Prefixes

```
/api/live/*     /api/paper/*    /api/exec/*
/api/risk/*     /api/decision/* /api/log*
/api/auth_ping
```

### Minimum `.gitignore`

```gitignore
.env
node_modules/
*.db
*.log
trade_execution_journal.json
```

---

<div align="center">

## 📦 npm Scripts

</div>

<details>
<summary><b>▶ Running</b></summary>

| Script | Description |
|---|---|
| `npm start` | Production |
| `npm run dev` | Watch mode |
| `npm run supervised` | Crash recovery + supervisor |

</details>

<details>
<summary><b>🧪 Testing</b></summary>

| Script | Description |
|---|---|
| `npm test` | All unit tests |
| `npm run test:unit` | Unit tests only |
| `npm run test:live` | Live connection tests |
| `npm run test:all` | Unit + live |

</details>

<details>
<summary><b>🔧 Tools</b></summary>

| Script | Description |
|---|---|
| `npm run env:check` | Environment variable check |
| `npm run clob:creds` | Generate API credentials |
| `npm run clob:headers` | L2 header test |
| `npm run clob:smoke` | CLOB connection smoke test |
| `npm run security:smoke` | Security endpoint check |

</details>

<details>
<summary><b>📊 Analysis</b></summary>

| Script | Description |
|---|---|
| `npm run replay:report` | Replay report for all assets |
| `npm run replay:metrics` | Detailed metric summary |
| `npm run replay:risk` | Risk profile analysis |
| `npm run replay:decisions` | Decision replay simulation |
| `npm run replay:optimize` | Optimization report |
| `npm run analyze:segments` | Trade segment analysis |
| `npm run policy:learn` | Policy learning script |

</details>

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:8B5CF6,100:00B4D8&height=120&section=footer" />

<br/>

**Do not trust signals. Measure them.**

<br/>

*This project is for educational and research purposes only. Not financial advice.*
*Always validate with PAPER mode and replay reports before using real capital.*

<br/>

[![Star](https://img.shields.io/github/stars/umutengez06-eng/Polymarket-Trading-System?style=social)](https://github.com/umutengez06-eng/Polymarket-Trading-System)

</div><img width="1920" height="1080" alt="Ekran görüntüsü 2026-04-16 230443" src="https://github.com/user-attachments/assets/ddefc954-a013-4a3c-b033-bd6e25e3c26f" />
