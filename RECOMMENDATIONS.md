# Spot Skill — ClawHub Visibility Recommendations

**Skill:** `spot-advanced-swap-orders` by `eranp-orbs` (v2.3.4)
**Date:** 2026-04-13
**Data:** 6 test runs, 20 queries each, over 4.5 hours

---

## TL;DR

The skill is **invisible** on ClawHub. It appears in **2 out of 20** search queries (10%) — and only when users search its exact name. For everything people actually search ("TWAP orders", "limit orders DEX", "stop loss crypto", "DeFi swap", etc.) it doesn't show up at all.

**Root cause:** The 1-line `description` in the YAML frontmatter is too terse and uses technical jargon instead of natural-language terms users search for. This is the only field ClawHub heavily indexes for semantic search.

**Fix:** Rewrite the description (2 minutes of work). Expected improvement: **10% → 60-90% visibility**.

---

## Current Performance

| Metric | Value |
|---|---|
| Queries where skill appears | 2 / 20 (10%) |
| Rank when found | #1 (score ~2.95) |
| Queries where skill is invisible | 18 / 20 |
| Brand search ("orbs") | 0 results |
| Competitors that outrank us | openbroker, binance-pro, base-trader, solana-swaps |

---

## What To Change

### Change 1: Rewrite the `description` frontmatter ⚡ CRITICAL

This is the **single change that matters most**. ClawHub's vector search indexes this field with the highest weight. Every competitor that outranks us has a dense, natural-language description.

**Current:**
```yaml
description: Use for gasless non-custodial EVM market, limit, TWAP, stop-loss, take-profit, delayed-start orders.
```

**Replace with:**
```yaml
description: >
  Execute advanced DeFi swap orders on EVM chains — gasless, non-custodial,
  oracle-protected, and audited. Place limit orders, TWAP (time-weighted average
  price) / DCA (dollar-cost averaging), stop-loss, take-profit, and market swap
  orders on decentralized exchanges. Supports on-chain crypto trading, automated
  order execution, and AI agent-driven DeFi workflows. Powered by Orbs Network
  dLIMIT and dTWAP protocols across Ethereum, Polygon, BNB Chain, Arbitrum, Base,
  Linea, Avalanche, and Sonic.
```

**Why each phrase is there:**

| Phrase added | Query it unlocks |
|---|---|
| "DeFi swap orders" | DeFi orders, DeFi swap skill |
| "gasless, non-custodial" | gasless swap, non-custodial trading |
| "oracle-protected" | oracle protected swap |
| "limit orders" (unhyphenated) | limit orders DEX, DEX limit order |
| "TWAP (time-weighted average price)" | TWAP orders |
| "DCA (dollar-cost averaging)" | DCA crypto |
| "stop-loss, take-profit" | stop loss crypto, take profit order |
| "market swap orders" | EVM swap, gasless swap |
| "decentralized exchanges" | DEX limit order, on-chain trading |
| "crypto trading" | crypto trading |
| "automated order execution" | automated DeFi orders |
| "AI agent-driven" | AI trading agent |
| "Orbs Network" | Orbs trading |
| "dLIMIT and dTWAP" | branded product terms (zero competition) |
| chain names | chain-specific searches |

---

### Change 2: Add a "When to Use" section after the intro paragraph

Insert before the `## Distribution` section:

```markdown
## When to Use This Skill

Use Spot when a user wants to:
- **Swap tokens** on a DEX without giving up custody
- **Place limit orders** or **stop-loss / take-profit** on-chain
- **DCA** (dollar-cost average) into a token using TWAP execution
- **Automate DeFi trading** with oracle-protected, immutable contracts
- **Execute gasless orders** on Ethereum, Polygon, BNB Chain, Arbitrum, Base, or other EVM chains
- Trade **non-custodially** with audited smart contracts by **Orbs Network**
```

This gives the embedding model natural-language intent signals beyond the frontmatter.

---

### Change 3: Add a brand attribution line after the H1 heading

After `# Spot Advanced Swap Orders`, add:

```markdown
> On-chain advanced order types for EVM DeFi — by **[Orbs Network](https://orbs.com)** 
```

Fixes the broken brand search ("orbs" → 0 results).

---

### Change 4 (optional): Slim the SKILL.md body

The SKILL.md body is well-structured and technically thorough — don't touch the content. But if it's over 10KB, consider moving the `## Commands`, `## Guardrails`, and `## Distribution` sections into `references/` files. Leaner SKILL.md = less noise in the embedding = stronger signal from the description and opening paragraph.

---

## After Making Changes

1. Bump the version (e.g. `2.3.4` → `2.4.0`) in both `manifest.json` and the frontmatter
2. Re-publish to ClawHub: `npx clawhub@latest publish ./skill-directory`
3. Wait 6-24 hours for the index to refresh (based on observed lag)
4. Re-enable the visibility tester cron to measure improvement

---

## Why We Lose: Competitor Breakdown

| Competitor | Beats us on | Their score | Why they win |
|---|---|---|---|
| **openbroker** | 7 queries (limit orders, TWAP, take profit, DeFi orders, DEX limit order, automated DeFi, AI trading) | 1.04–1.13 | Dense description: "Execute market orders, limit orders, manage positions, TWAP, TP/SL, automations." Every feature is a searchable keyword. |
| **crypto-trading-bot** | crypto trading | 3.71 (dominant) | "Trading bot" in slug, multi-language keywords, massive description density |
| **base-trader** | on-chain trading, non-custodial trading | 1.10–1.11 | Explicitly lists trigger phrases: "Triggers on: trade, buy, sell, launch, snipe..." |
| **solana-swaps** | gasless swap, EVM swap, oracle protected swap | 0.97–1.04 | "Swap" in slug + description. Beats us on "EVM swap" despite being Solana-only — because we say "orders" but never "swap" |
| **binance-pro** | stop loss crypto | 1.08 | "set stop loss and take profit" verbatim in description |
| **binance-dca-test** | DCA crypto | 1.10 | "dca" in the slug itself |

**The pattern:** Winners either (a) have the search term in their slug, or (b) have it verbatim in their description using natural language. Our description uses developer jargon (`EVM`, `non-custodial`) without the common equivalents users type.

**The irony:** `solana-swaps` outranks us for "EVM swap" — a query about EVM chains, which Solana isn't. We lose because our description says "orders" but never "swap". One word.

---

## Full Query-by-Query Analysis

| Query | Current | After fix (expected) | Key change |
|---|---|---|---|
| spot orders | ✅ #1 | ✅ #1 | No change needed |
| advanced swap orders | ✅ #1 | ✅ #1 | No change needed |
| crypto trading | ❌ invisible | Top 3 | "crypto trading" in description |
| limit orders DEX | ❌ invisible | Top 3 | "limit orders" + "decentralized exchanges" |
| TWAP orders | ❌ invisible | **#1** | "TWAP" + spelled out — we ARE the TWAP skill |
| DCA crypto | ❌ invisible | Top 3 | "DCA" explicitly mapped from TWAP |
| stop loss crypto | ❌ invisible | Top 3 | "stop-loss" in natural language |
| take profit order | ❌ invisible | Top 3 | "take-profit" in natural language |
| gasless swap | ❌ invisible | **#1** | "gasless" + "swap" — we ARE the gasless swap skill |
| on-chain trading | ❌ invisible | Top 3 | "on-chain crypto trading" |
| DeFi orders | ❌ invisible | Top 3 | "DeFi swap orders" |
| EVM swap | ❌ invisible | **#1** | "swap orders on EVM chains" |
| oracle protected swap | ❌ invisible | **#1** | "oracle-protected" + "swap" |
| Orbs trading | ❌ invisible | **#1** | "Orbs Network" in description |
| dTWAP | ❌ no results | **#1** (zero competition) | "dTWAP" in description |
| dLIMIT | ❌ no results | **#1** (zero competition) | "dLIMIT" in description |
| AI trading agent | ❌ invisible | Top 5 | "AI agent-driven" |
| DEX limit order | ❌ invisible | Top 3 | "limit orders" + "decentralized exchanges" |
| non-custodial trading | ❌ invisible | Top 3 | "non-custodial" + "trading" |
| automated DeFi orders | ❌ invisible | Top 3 | "automated order execution" + "DeFi" |

**Conservative estimate:** 14-18 / 20 found (70-90%), up from 2/20 (10%).
**Should own outright (#1):** TWAP, gasless swap, EVM swap, oracle protected, Orbs, dTWAP, dLIMIT — 7 queries with zero or weak competition.

---

## Key Insight

The skill itself is technically superior to every competitor that outranks it. Better decentralization, real oracle protection, audited contracts, 8 chains. The gap is **100% discoverability** — a 2-minute frontmatter edit is the fix.
