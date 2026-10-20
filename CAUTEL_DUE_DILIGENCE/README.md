# CAUTEL — Due Diligence Package

**Cautel Governance Systems** · product: CAUTEL · status: 54 suites / 640 checks, 0 failures (2026-10-20)

This package exists so a serious evaluator can prove the claims in 15
minutes without reading source code. Every number here is produced by a
battery that ran in the last full sweep; every denial below is a real
engine output, not a description of one.

## Read it in 15 minutes

| # | Document | What it proves |
|---|---|---|
| 01 | [executive-summary](01-executive-summary.md) | What CAUTEL is, in one page |
| 02 | [constitutional-model](02-constitutional-model.md) | The six-line constitutional model + the five-stage chain |
| 03 | [security-invariants](03-security-invariants.md) | The 15 invariants, each with its enforcement mechanism |
| 04 | [adversarial-testing](04-adversarial-testing.md) | Attack catalogue: expected → observed → evidence → result |
| 05 | [resilience-testing](05-resilience-testing.md) | Power loss, kill -9, partitions, replay, reconstruction |
| 06 | [test-matrix](06-test-matrix.md) | 54 suites / 640 checks, 0 failures — the green board |
| 07 | [autonomous-corporation-demo](07-autonomous-corporation-demo.md) | A governed day at Nexus: a department doing real work |
| 08 | [architecture](08-architecture.md) | Trust model, threat model, deployment, boundaries |
| 09 | [roadmap](09-roadmap.md) | What is next, and what is honestly not yet |

## The green board

```
54 test suites          PASS
640 checks              PASS  (0 failures)
Authority escalation    DENY  (evidence receipted)
Overspend               DENY  (budget conserved)
Out-of-scope action     DENY  (scope enforced)
Revoked authority       DENY  (transitive kill)
Forged signature        DENY  (key rings)
Forged policy update    DENY  (restricted mode trips)
Power-loss recovery     PASS  (kill -9 drill 13/13)
Network partition       PASS  (12/12)
Replay reconstruction   PASS  (torn-tail 9/9)
Evidence chain integrity PASS (seal + airgap anchor)
```

## What this package deliberately does not do

- It contains **no valuation** and no revenue projections. One enterprise
  customer proving demand is worth more than any number we could print.
- It does not claim Byzantine-fault-tolerant consensus. A colluding
  replication majority can **censor** (availability); it can never
  **fabricate** authority (safety) — authority is signature-bound.
- It does not claim protection against root, or against the holder of
  the operator key. Those are documented boundaries, not hidden ones.
