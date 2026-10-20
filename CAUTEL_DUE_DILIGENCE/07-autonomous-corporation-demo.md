# 07 — Autonomous Corporation Demo

The question this answers: **can a governed autonomous department
perform useful business work under CAUTEL?** This is a real run of the
real enforcement engine — the same one the battery exercises — on a
fictional but structurally complete organisation, **Nexus** (a Cautel
Governance Systems demonstration company).

## The org chart

```
operator (human root)
├── Procurement Dept (grant: procure/*, budget 5,000)
│   ├── Buyer agent — office line (delegation, budget 1,000)
│   └── Buyer agent — IT line (delegation, budget 1,500)
└── Finance Dept (grant: report/*, budget 1,500)
    └── Analyst agent — Q3 line (delegation, budget 700)
```

Every grant and delegation above is a **signed object** with a budget
and a scope. Nothing else exists in the system.

## The day's transcript (verbatim engine output)

```
=== A GOVERNED DAY AT NEXUS (evidence transcript) ===
  [09:00] operator   grants Procurement authority (5,000)           -> ALLOW
  [09:01] operator   grants Finance authority (1,500)               -> ALLOW
  [09:05] chaos      hires Buyer agent, office line (1,000)         -> ALLOW
  [09:06] chaos      hires Buyer agent, IT line (1,500)             -> ALLOW
  [09:07] console    hires Analyst agent, Q3 line (700)             -> ALLOW
  [10:14] telemetry  buys office supplies (300)                     -> ALLOW
  [11:20] telemetry  buys IT hardware (900)                         -> ALLOW
  [14:02] simagent   files the Q3 finance report (200)              -> ALLOW
  [15:31] telemetry  attempts a 20,000 spend                        -> DENY(budget_exhausted)
  [15:32] telemetry  attempts to hire staff (outside authority)     -> DENY(scope_violation)
  [16:00] operator   terminates the Buyer's office line             -> ALLOW
  [16:05] telemetry  terminated line tries to spend (1)             -> DENY(revoked_authority)
  [16:30] console    compromised Finance mints itself authority     -> DENY(unauthorized)
  [16:45] simagent   forged-signature spend attempt                 -> DENY(bad_signature)
  --- day total: 9 accepted, 5 denied (every one receipted) ---
```

## What to notice

- The legitimate work (three business transactions) executes — the
  corporation is not paralysed by its constitution.
- The overspend, the out-of-scope hire, the terminated agent's stale
  authority, the compromised department's self-grant, and the forged
  signature all die with a **specific denial code**, before execution.
- Every one of the 14 events is receipted — the day is fully
  reconstructable from the evidence chain alone.
- End of day: the independent verifier finds **zero invariant
  violations**, and no new authority entered the corporation.

The demo is itself a battery (`bin/test_corporation_demo.py`, 11/11) in
the governed sweep — it cannot rot silently: any regression in the
engine fails the sweep.
