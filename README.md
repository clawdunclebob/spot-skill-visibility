# Spot Skill Visibility Optimization

Automated system to improve the discoverability of the Orbs `spot` (Binance Spot Trading) skill on [ClawHub](https://clawhub.ai).

## Problem
The skill ranks 9th/10 for "spot orders" (score: 0.840) and doesn't appear for common queries like "crypto trading" or "buy bitcoin".

## Root Causes
1. **Generic slug** — `spot` competes with food delivery, Spotify, etc.
2. **API-doc style description** — "Binance Spot request using the Binance API" doesn't match user search intent
3. **24KB SKILL.md** — bloated with parameter tables that dilute the semantic signal
4. **No use-case language** — missing terms users actually search for

## Architecture

Two cron jobs in a feedback loop:

```
┌─────────────────────┐     ┌──────────────────────┐
│  Visibility Tester  │────>│  GitHub Issues        │
│  (every 6 hours)    │     │  [Test Reports]       │
│                     │     └──────────┬───────────┘
│  Runs 20 queries    │               │
│  Records rankings   │               ▼
│  Creates reports    │     ┌──────────────────────┐
└─────────────────────┘     │  Visibility Improver  │
                            │  (daily)              │
         ┌─────────────────>│                       │
         │                  │  Reads test results   │
         │                  │  Picks Todo issues    │
         │                  │  Proposes SKILL.md    │
         │                  │  changes              │
         │                  └──────────┬───────────┘
         │                             │
         │                             ▼
         │                  ┌──────────────────────┐
         └──────────────────│  ClawHub publish      │
                            │  (manual approval)    │
                            └──────────────────────┘
```

## Kanban Board
👉 [Project Board](https://github.com/users/clawdunclebob/projects/5)

### Columns
| Column | Meaning |
|--------|---------|
| **Backlog** | Ideas and future iterations |
| **Todo** | Queued for next cycle |
| **In Progress** | Currently being worked on |
| **Testing** | Visibility test running |
| **Needs Review** | Test results need analysis |
| **Done** | Completed and verified |

## Iterations

### Iteration 0: Baseline
- [ ] Run initial 20-query visibility test
- [ ] Record baseline rankings

### Iteration 1: Quick Wins
- [ ] Rename slug to `binance-spot-trading`
- [ ] Rewrite description for semantic search
- [ ] Add keyword-rich use-case block
- [ ] Re-test and measure improvement

### Iteration 2: Structural
- [ ] Slim SKILL.md (move API tables to references/)
- [ ] Cross-reference with related skills
- [ ] Community promotion
- [ ] Re-test and measure improvement

## Test Queries
```
spot orders, crypto trading, binance trading, buy sell crypto, exchange API,
spot trading bot, binance spot, crypto orders, trading skill, DEX trading,
wallet trading, market orders, limit orders, crypto portfolio, token swap,
binance API, buy bitcoin, sell ethereum, trading automation, crypto agent
```
