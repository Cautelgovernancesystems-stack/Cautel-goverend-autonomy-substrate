# Reddit r/algotrading post — "what I learned building a governed trading agent"

**Title:** I gave my trading bot a constitution. Here's what the guardrails taught me.

**Body:**

Been building a day-trading agent (IG API + Lightstreamer, then a public
crypto feed) and the interesting part wasn't the indicators — it was the
guardrails. Some findings after watching it run live on a small account:

1. **Risk caps as code, not intention.** 0.5–1% equity per trade, ATR bracket
   stops, mandatory flatten before close. When the account was too small for
   the minimum order size, the engine refused every signal and *told me why*
   (margin floor, not settings).
2. **The spread gate is brutally honest.** On 5-minute crypto bars, the
   2×ATR% spread rule rejected setups I was sure were good — because the
   spread genuinely ate the edge. Moving to 15-minute bars fixed it. The rule
   was right; my timeframe was wrong.
3. **Data plumbing is half the battle.** Broker historical-data allowance
   exhaustion took the bot down more times than any strategy bug. Built:
   local bar cache, delta ingestion, rate-limit cooldowns, and a fallback
   public data feed. The bot now survives broker outages without losing state.
4. **Evidence first.** Every signal/fill/exit is journaled with a P&L ledger —
   win rate, expectancy, profit factor, drawdown. No trade happens that isn't
   measurable afterward.

Not selling anything — the governance layer (specs, invariants, fail-closed
doctrine) is documented publicly, the trading engine is a demo workload for
it. Happy to answer questions about the architecture or the specific
guardrail implementations.

Full disclosure: this is a system under active development; nothing here is a
profit guarantee and it's all been validated on paper/virtual funds first.
