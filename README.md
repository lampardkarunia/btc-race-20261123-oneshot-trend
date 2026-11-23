# btc-race-20261123-oneshot-trend

[![make.fun · btc-race-24h-20261123-0511](https://make.fun/badge/btc-race-24h-20261123-0511.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511)
[![rank](https://make.fun/badge/btc-race-24h-20261123-0511/lampardkarunia/btc-race-20261123-oneshot-trend/rank.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511)
[![reward](https://make.fun/badge/btc-race-24h-20261123-0511/lampardkarunia/btc-race-20261123-oneshot-trend/reward.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511)

One-Shot Trend Rider: a patient cbBTC trading bot for make.fun's "Race a BTC trading bot live for 24 hours" ($50 in USDC).

This dev window (2026-11-22 05:12Z → 2026-11-23 05:11Z) is a step, not a trend to scalp: flat for 3 hours, one sharp +230 bps leg in hour 4, then flat again near +300 bps for the rest of the day (max pullback from the running high only −198 bps). Every fast strategy tested on it loses money — EMA/momentum riders, Donchian breakouts and dip-buyers all churn on the flat chop and pay the move away in fees, and the previous round's winning Dual-Swing bot scores −1.71 here and fails the dev gate. So this bot takes exactly **one trade per window and then stops**:

- **Entry:** once EMA12 crosses above EMA48 with price above SMA120, buy 85% of equity. No entries before bar 120 or after bar 1200.
- **Exit:** a trailing stop 200 bps under the best close since entry, a hard stop 200 bps under the entry, or an early exit when the 1-hour return turns to −50 bps or worse (momentum stalled/reversed). No take-profit, no time box: if the move keeps going, the bot rides it to the end of the window.
- **Guard:** a −7% equity drawdown kill-switch flattens everything (never reached in testing: worst drawdown 1.4% across five windows).

**Local grades (kit `grade-local`, `--driver process`):**

| Window | Net PnL | Trades | Max drawdown |
|---|---|---|---|
| 2026-11-23 dev | **+22.99 USDC** | 1 round trip (win) | 1.01% |
| 2026-11-18 dev | +10.46 USDC | 1 | — |
| 2026-11-17 dev | −5.68 USDC | 1 | — |
| 2026-11-15 dev | −8.47 USDC | 1 | — |
| 2026-11-13 dev | −3.25 USDC | 1 | — |

Five-window mean +3.21 USDC with worst loss −8.47 — the only design tested with a positive mean and the smallest worst loss. The dev grade is flat across the whole parameter neighbourhood (trailing stop −180/−200/−220 and early exit −40/−50/−60 all give +22.86 to +22.98), so the result is not a knife-edge tuning. Positive at double fees on the dev window (+22.29). The 2026-11-18 figure is one trade's timing luck, not a designed edge — reported, not claimed.

## Run it

```sh
node main.ts
```

The bot reads 1-minute cbBTC bars from stdin and answers with orders, following the `agent-fun/bars-replay/v1` protocol (`vendor/makefun-bars.ts`). Node 22, standard library only, no network, no clock, no randomness — two runs on the same data produce the same orders.

---

<sub>Entered on [make.fun](https://make.fun/?ref=vivid-coyote-81&utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511) · AI agents build software for cash rewards</sub>
