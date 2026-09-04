# See it in action

Two recorded demonstrations. Links are placeholders until recordings are
published — the procedures below are exactly what the recordings show.

## Demo 1 — The Refusal

**What it shows:** an agent attempts an action it is not delegated for;
CAUTEL refuses it, records the refusal in the evidence DAG, and the chain
verifies root-to-head afterwards.

```
1. Agent forms intent outside its delegation (e.g. "write to production").
2. Constitutional plane evaluates authority + invariants.
3. Verdict: REFUSED — nothing executes.
4. Evidence DAG: refusal record appended.
5. Verify chain: root-to-head OK.
```

> `[GIF / asciinema link — place here]`

## Demo 2 — A governed workload

**What it shows:** a day-trading agent operating under constitutional risk
rules — capped risk per trade, automatic stop/limit brackets, fail-closed
filters (volume, volatility, market state), a full evidence journal, and a
mandatory flatten before the session close.

```
[09:30] warm-up: indicators online (VWAP, RVOL, EMA9/21, RSI-9, ATR-14)
[11:10] SIGNAL BUY  rvol=3.48 rsi=66.1  → bracket stop/limit, risk capped at 0.5%
[11:10] evidence: fill recorded
[11:14] trail: stop ratcheted to EMA9
[11:19] evidence: exit recorded (+999.94)
[21:55] FLATTEN: all positions closed before session end
```

> `[GIF / asciinema link — place here]`

## How the recordings are produced

- Terminal capture: `asciinema rec demo.cast` (publish on asciinema.org) or
  `ttyrec`, then convert to GIF with `ttygif` / `agg`.
- Demo 1 runs the hostile validation matrix scenarios from
  [`validation/METHODOLOGY.md`](../validation/METHODOLOGY.md) — show one
  unauthorised attempt end-to-end.
- Demo 2 runs a full synthetic trading session with the evidence journal
  visible, ending in the flatten event.
