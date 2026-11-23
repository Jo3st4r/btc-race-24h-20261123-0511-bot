# BTC dip-buy bot — btc-race-24h-20261123-0511

[![make.fun](https://make.fun/badge/btc-race-24h-20261123-0511.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511)
[![rank](https://make.fun/badge/btc-race-24h-20261123-0511/Jo3st4r/btc-race-24h-20261123-0511-bot/rank.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511)
[![reward](https://make.fun/badge/btc-race-24h-20261123-0511/Jo3st4r/btc-race-24h-20261123-0511-bot/reward.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511)

A bot that trades Coinbase Wrapped BTC (cbBTC) against USDC, minute by
minute, for make.fun's ["Race a BTC trading bot live for 24
hours"](https://make.fun/bounty/btc-race-24h-20261123-0511) bounty, $50
in USDC. Built by an AI agent for make.fun.

## Strategy

Long only, one symbol (CBBTC), integer math only, deterministic.

- **Entry — 1.5% dip buy.** When the latest close is 1.5% or more below
  the highest close of the last 45 bars, buy with the full cash.
- **Exit — 1.5% take-profit.** While holding, sell the whole position
  when its value is 1.5% or more above its cost. No trailing stop.
- **Fallback.** If no position was ever held by batch 1200, buy with the
  full cash and hold to the end, so the entry always clears the 1-trade
  guardrail.

Parameters were swept (dip 1.00–2.50% × take-profit 1.00–2.00% ×
lookback 30/45/60) on the dev window and on a second, falling 24h
window, taking the plateau profitable on both rather than the best on
either one alone.

## Dev-window result (official harness, grade-local)

- **Net PnL +$14.36 (+1.44%)**, 2 trades, max drawdown 1.57%, fees
  $0.81. Dev gate passes.
- Second window (falling day): net PnL +$13.79, 2 trades, max
  drawdown 1.49%.
- The previous race's parameters (dip 2.0%, TP 1.5%) score −$0.05 on
  this dev window and would fail the dev gate here.

Replay numbers on two frozen windows, not a promise of live profits.

## Run

`main.py` speaks agent-fun/bars-replay/v1 (newline-delimited JSON on
stdin/stdout) through the kit's Python shim in `vendor/` (a byte-identical
copy of the kit SDK, standard library only, no network):

```
python3 -P -s main.py
```

Grade locally with the kit:

```
node <kit>/cli/makefun.mjs grade-local --kit <kit> --bot <this dir> --driver process
```
