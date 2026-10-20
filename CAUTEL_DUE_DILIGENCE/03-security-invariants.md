# 03 — Security Invariants

CAUTEL's invariant set is explicit, enumerated, and each invariant is
enforced by a mechanism and proven by a battery. Two layers:

## The six constitutional guarantees (the headline)

| # | Guarantee | Enforcement | Proof |
|---|---|---|---|
| G1 | Authority cannot be created | operator-only genesis (`unauthorized` otherwise) | capture drill 30/30 |
| G2 | Authority must be granted | no other entry path exists | engine law + drills 21/21 |
| G3 | Authority cannot increase during delegation | conservation arithmetic at apply | property-based 7/7, differential 3/3 |
| G4 | Revoked authority cannot execute | revocation is transitive; stale spends die | drills, adversary profiles 8/8 |
| G5 | Every execution produces evidence | sealed transitions on the tamper chain | torn-tail 9/9, ouroboros 7/7 |
| G6 | Every denial produces evidence | denials are receipted decisions | every battery asserts receipt codes |

## The enforcement invariants (I1–I15, verbatim)

1. **I1** — a request without a verifiable signed envelope never executes
2. **I2** — one command identity commits exactly once
3. **I3** — commands execute in per-source order; gaps rejected, then resync
4. **I4** — unauthorized principal/action denied BEFORE execution (no effect)
5. **I5** — an unwritable or corrupted ledger blocks execution
6. **I6** — external policy edits are detected → restricted
7. **I7** — modified executors are no longer attested → task refused
8. **I8** — crash at any moment never re-executes and never loses committed state
9. **I9** — rate budgets, in-flight caps, priority reserves are deterministic
10. **I10** — telemetry signed/sequenced; gated classes fail closed without it
11. **I11** — time manipulation (rollback/jump) fails closed
12. **I12** — simultaneous faults → deterministic outcomes
13. **I13** — no alternative path bypasses governance
14. **I14** — revoked keys / expired principals permanently denied
15. **I15** — the audit chain is tamper-evident end-to-end

## The authority invariants (delegation layer)

- **Conservation (I1-delegation)** — for every operator grant, spent +
  live remaining never exceeds the granted budget; authority is never
  created or duplicated (kills multiplication/splitting attacks).
- **Scope containment** — a child delegation never exceeds its parent's
  scope (no scope escalation).
- **Expiry monotonicity** — no delegation outlives its parent.
- **Depth cap / cycle-free** — the delegation tree is bounded and acyclic.
- **Traceability** — every accepted spend's chain reaches a live operator
  grant.
- **One-shot ids** — spend, renew, reduce and expire ids are single-use.
- **Transitive revocation** — killing a node kills the whole subtree.

Each layer is checked twice: once by the enforcement engine, and again
by an independent verifier that re-folds the entire recorded manifest
and re-derives every decision — a recorded decision that the law would
not produce is a violation.
