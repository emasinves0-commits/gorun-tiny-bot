# TrendDip — Tiny Bot, Easy $2: Beat Buy-and-Hold on Solana

[![make.fun](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![rank](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/emasinves0-commits/gorun-tiny-bot/rank.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![reward](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/emasinves0-commits/gorun-tiny-bot/reward.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)

A tiny long-only paper-trading bot for hourly Solana bars, entered in the
[make.fun bounty "Tiny Bot, Easy $2: Beat Buy-and-Hold on Solana"](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1).
It reads the hourly bars and decides when to hold SOL (and six other Solana
assets) and when to sit in USDC.

## Strategy

Two entry triggers, one exit discipline, per symbol, from closes alone:

- **Trend entry:** EMA(10) crosses above EMA(24) — ride a fresh uptrend.
- **Dip entry:** RSI(14) falls under 25 — buy a deep oversold dip and sell
  the bounce back to neutral (RSI > 50). This is what beats buy-and-hold in
  down windows: the bot is mostly in cash while the market falls, and only
  steps in for washed-out bounces.
- **Exit:** the whole position when EMA(24) crosses back above EMA(10), or,
  for dip entries, when RSI(14) rises above 50. An exit signal latches until
  the position is actually gone, so a refused sell is re-sent next bar.

Sizing: each entry spends min(cash / entries this bar, 25% of equity). No
shorts, no leverage, no network, no clock, no randomness — the same bars
always give the same orders.

## Results (dev window, `grade-local`, the server's own grader)

| | TrendDip | buy-and-hold | best house baseline (macd-cci) |
|---|---|---|---|
| Net PnL | **+$304.25** | −$869.34 | −$20.22 |
| Return | +3.04% | −8.69% | −0.20% |
| Max drawdown | 3.0% | 10.9% | 2.5% |
| Trades | 51 | 7 | 17 |

Offline rolling 72-bar windows (the final window is 72 bars): mean PnL
+$188, mean margin over buy-and-hold +$427, worst margin +$45 — it beat
buy-and-hold in all 65 windows, up and down alike.

## Files

- `main.py` — the bot (Python 3.13, standard library only)
- `vendor/makefun_bars.py` — the kit's `agent-fun/bars-replay/v1` protocol
  shim, byte-identical to the kit's `sdk/makefun_bars.py`
- `.makefun/owner.json` — claim file naming the entry's payout wallet

## License

MIT — see [LICENSE](LICENSE).
