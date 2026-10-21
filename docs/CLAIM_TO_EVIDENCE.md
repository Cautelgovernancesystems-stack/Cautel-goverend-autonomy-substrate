# Claim-to-Evidence Matrix

Every major security claim, the mechanism that enforces it, the test
that challenges it, and the honest limitation. If a claim has no row
here, it is not claimed.

| # | Claim | Mechanism | Evidence (battery) | Limitation |
|---|---|---|---|---|
| 1 | Authority cannot appear — it must be granted | operator-signed genesis; any other signer denied `unauthorized` | governance-capture drill 30/30, delegation drills 21/21 | the holder of the operator signing key IS the root (out of model) |
| 2 | Delegation cannot create or amplify authority | conservation arithmetic at apply; multiplication/splitting denied | property-based 7/7, differential 3/3, adversary profiles 8/8 | float equality tolerance 1e-6 (documented) |
| 3 | Two valid authorities cannot merge into a larger one | every authority line bounded to its own grant | policy-conflicts C7 | none known |
| 4 | Scope escalation is denied | scope containment at delegation; delimiter-aware matching at authorisation | harden P6, policy-conflicts C5/C6/C8 | scope model is exact-or-wildcard (no hierarchy prefixes) — documented |
| 5 | Revoked authority cannot execute, on any replica, at any timing | transitive revocation at apply; deterministic law on the ordered log | revocation-timing 8/8 | a censoring replication majority can delay the revocation's *commit* (availability, no BFT) — never its effect once committed |
| 6 | Replay is refused | exactly-once command identity (I2); one-shot ids | claim race 11/11, drills | exactly-once is internal; external side effects are at-most-once + stable `command_id` idempotency key |
| 7 | Evidence cannot be silently removed | every allow/deny receipted; torn tails quarantined and refused | torn-tail 9/9, audit-window 11/11 | transport-level bytes that never form a request cannot be receipted (physical-layer fact) |
| 8 | Audit artifacts cannot be altered undetectably | operator-HMAC token over the package manifest; boundary state digests; live-chain prefix check | audit-window W4–W6 | verification requires the operator key + the live chain |
| 9 | Policy cannot change on a running system without a key | operator-signed policy attestation; drift → restricted | policy attestation A1–A10 | root/same-UID processes are out of model |
| 10 | Federation cannot inflate authority | foreign artifacts verified against published roots, then admitted as budget-identical local mirrors | federation 11/11 | v1 roots are out-of-band shared HMAC secrets; PKI roots on the roadmap |
| 11 | Restricted mode fails closed, even for the operator's governed clear | secrets withheld while restricted; audited clear + clean re-attestation is the only exit | governance-capture G5a–G5c | OS-level file access (clear_restricted) is an audited, operator-only recovery act |
| 12 | The enforcement law is deterministic on every replica | pure state machine: no wall clock, no RNG, no I/O in apply_op | governance-capture G3, revocation-timing R7 | none known |
| 13 | Crash at any moment never re-executes and never loses committed state | idempotent claim/commit states, WAL stores, sealed transitions | kill-9 drill 13/13, ouroboros 7/7 | single-machine scope today |
| 14 | The core invariants hold mathematically | Z3 SMT proofs: default-deny, signature, budget, revocation all UNSAT | z3 proof 6/6 | SMT checks the specified properties, not the whole implementation |
| 15 | The release is reproducible and tamper-evident between builds | two independent builds byte-identical; sealed manifest; interpreter pin | reproducible 3/3 (4,606 files), interpreter pin 7/7 | speculative-execution (Rowhammer-class) attacks are detected, not prevented |
| 16 | The plane has no economic surface to vampire | no fees/gas/prices/stakes/tokens/markets in the decision path; scale-invariant enforcement | economic decoupling 7/7 | none known |

## Reading the evidence

- The batteries above run in the governed full sweep: **57 suites /
  669 checks / 0 failures** (2026-10-21). Per-suite final lines are
  reproduced verbatim in
  [`CAUTEL_DUE_DILIGENCE/06-test-matrix.md`](../CAUTEL_DUE_DILIGENCE/06-test-matrix.md).
- Each limitation column is a boundary we state rather than hide; the
  same boundaries appear in
  [`CAUTEL_DUE_DILIGENCE/08-architecture.md`](../CAUTEL_DUE_DILIGENCE/08-architecture.md).
- Findings against any row belong in [`FINDINGS.md`](../FINDINGS.md).
