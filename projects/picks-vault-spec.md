# Picks Vault Spec — V1

> **One-liner:** Tokenized ownership of the house edge, per league, per season.

---

## Core Concept

The Vault is not a pool. It's **the casino's bank**. Anyone can become the house by depositing USDC into a league-specific Vault. Every bet placed on the platform runs against the Vault. The Vault earns the hold (~20%). Vault shares represent ownership of that revenue stream.

**What the LP is buying:** A share in a profitable operation backed by real betting revenue — not speculation, not governance, not narrative.

---

## Architecture

### Three Players, One System

| Role | What they do | What they earn |
|---|---|---|
| **Players** | Bet on player props | Payouts when they win |
| **Vault LPs** | Fund the other side of every bet | Share of the hold (~20%) via share price appreciation |
| **Referrers** | Bring players to the platform | % of bets + % of winnings from referrals |

### Revenue Split (per $1 wagered)

| Layer | % of Hold | Notes |
|---|---|---|
| Vault LPs | 75% | Distributed via share price appreciation |
| Protocol (Picks) | 15% | Direct company revenue |
| Referrers | 10% | Growth engine |

**Picks earns twice:** As LP on its own seed capital + Protocol fee on all volume.

---

## Vault Structure

### Per League, Per Season

Each Vault is tied to a specific competition:
- 🏆 World Cup 2026 Vault
- ⚽ Champions League 26/27 Vault  
- 🏴󠁧󠁢󠁥󠁮󠁧󠁿 Premier League 26/27 Vault
- etc.

When a season/tournament ends → Vault settles → LPs withdraw → New Vault opens for next season.

**This creates recurring demand.** Every season is a new drop. New opportunity, new FOMO, new entry point.

### No Cap on Vault Size

A casino doesn't limit its bankroll. More money in the Vault = more betting activity the platform can support. No cap. Ever.

### Deposits Open Throughout

LPs can deposit at any time — before or during the tournament/season.

- Early LP = more risk, more upside (bought shares cheap)
- Late LP = less risk, less upside (bought shares at higher price)
- Share-based pricing ensures fairness automatically

### Withdrawals Locked Until End (V1)

For World Cup Vault (V1): locked until tournament ends. Clean, simple, no bank-run risk.

Future vaults (leagues): withdrawal windows may open — the share system supports this natively.

---

## Share-Based Mechanism

### How It Works

1. LP deposits USDC → receives Vault shares at current share price
2. Share price starts at $1.00
3. As the Vault earns hold from bets → share price increases
4. At settlement, LP redeems shares at final share price

### Example

```
Day 1:   Share price = $1.00
         LP-A deposits $10,000 → gets 10,000 shares

Day 15:  Vault earned 8% → Share price = $1.08
         LP-B deposits $10,000 → gets 9,259 shares @ $1.08

End:     Share price = $1.22
         LP-A: 10,000 × $1.22 = $12,200 (profit: +22%)
         LP-B:  9,259 × $1.22 = $11,296 (profit: +13%)
```

Early entry = more reward. Fair, transparent, automatic.

### UX Display

```
🏆 World Cup 2026 Vault

Your deposit:     $10,000
Your shares:      9,259 shares
Share price:      $1.08
Current value:    $10,800
P&L:             +$800 (+8.0%)
```

Mechanism is sophisticated. Display is simple.

### Future: Composability

Vault shares can become tradable tokens → secondary market, transferable positions, DeFi integrations. The share system supports this natively.

---

## Risk Management

### What Protects the Vault

**No daily cap on volume.** Every bet with correct odds is +EV for the Vault. More volume = more revenue.

**Protection is at the prop and match level, not daily level:**

#### 1. Dynamic Odds
- Lines move based on betting volume
- Heavy action on one side → odds shift automatically  
- Monitoring competitors' lines in real-time
- The Vault always prices risk correctly

#### 2. Per-Prop Exposure Management
- If too much money concentrates on one prop → odds move significantly or prop closes
- This is the primary defense mechanism

#### 3. Match-Level Correlation Management
- Props within the same match are correlated (France wins 4-0 → multiple overs hit)
- System monitors aggregate exposure per match, not just per prop
- **This is the most critical risk layer**

### The Key Insight

The Vault doesn't need protection from volume. It needs protection from **concentration**. Dynamic odds and per-prop/per-match exposure management handle this.

---

## World Cup 2026 — Launch Plan

### Economics

| Parameter | Value |
|---|---|
| Picks seed | $250,000 |
| Community LP (est.) | $100,000 - $300,000 |
| Total Vault | $350,000 - $550,000 |
| Expected hold | ~20% |
| Tournament duration | ~6 weeks (June-July 2026) |

### Scenario Modeling

| Scenario | Total Handle | Gross Profit (20%) | LP Yield (75%) | Protocol Fee (15%) |
|---|---|---|---|---|
| Conservative | $500K | $100K | $75K | $15K |
| Base | $1.5M | $300K | $225K | $45K |
| Optimistic | $3M | $600K | $450K | $90K |

### Vault Lifecycle

```
Phase 1: Pre-Tournament (2-4 weeks before)
├── Vault opens
├── Picks seeds $250K  
├── Community LP deposits open
└── Marketing: "Own a piece of the World Cup"

Phase 2: Group Stage
├── Props live per match day
├── Daily settlement after matches
├── LPs see real-time P&L + share price
├── New LP deposits welcome (at current share price)
└── Vault grows from hold

Phase 3: Knockouts
├── Higher volume (bigger games)
├── Vault already profitable (share price > $1.00)
├── Marketing: "Vault up X% — still time to get in"
└── Peak revenue period

Phase 4: Settlement (1 week after final)
├── Final P&L calculated
├── LPs redeem shares → receive USDC
├── Protocol fee collected
└── Vault closes

Phase 5: Next Drop
├── "Champions League 26/27 Vault" opens
├── Returning LPs re-deposit
└── Cycle continues
```

---

## What This Really Is

> The Vault is not a feature. It's the **ownership model** of the product.
>
> Every Vault share is a claim on real revenue from real betting activity. Share price rises because the house earned money — not because of hype, speculation, or token inflation.
>
> This is the closest thing to a stock in a casino — tokenized, per-league, renewable every season.

---

## Research References

### Pixie Chess (pixiechess.xyz)
- Chess + collectible pieces with abilities on Base L2
- Vault funded by 100% of piece sales → funds tournament prize pools
- VRGDA dynamic pricing (demand-based)
- Seasonal scarcity + burn mechanic (deflationary)
- Launched April 2, 2026

### Megapot (megapot.io)  
- Daily on-chain lottery on Base, $1/ticket
- Three-player model: Players / Backers / Referrers
- Backers = the Vault, fund prize pool, earn ~20% hold
- Share-based LP system with withdrawal process
- $200M+ in drawings since July 2024, 19 jackpot winners
- $5M pre-seed (Dragonfly, Coinbase Ventures, FanDuel/Betfair founders)
- Provably fair via Pyth Network entropy

---

*Created: 2026-04-03*
*Status: Draft V1 — pending product review*
