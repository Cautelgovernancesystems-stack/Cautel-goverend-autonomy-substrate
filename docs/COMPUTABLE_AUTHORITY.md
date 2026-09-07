# Computable Authority — the policy twin, already running

NOMOS Protocol's OECD submission ("Computable Authority: A Runtime
Reference Architecture for Machine-Executable Law and Governance",
August 2026) proposes that autonomous software requires more than
machine-readable law — it requires **machine-computable authority**: a
sealed authority artifact, a deterministic deny-by-default runtime, and
the separation of compilation (governed, reviewed, sealed) from execution
(deterministic evaluation of proposed actions).

That thesis is the architecture CAUTEL has been running. This document
maps the paper to the implementation, so the conversation can cite a
working system rather than a proposal.

## The mapping

| NOMOS thesis | CAUTEL mechanism |
|---|---|
| "The model can propose an action. It cannot authorize itself to perform it." | the enforcement chain — every action signed, authorized, budgeted, receipted before execution |
| Compilation ≠ Execution: probabilistic systems may construct authority; deterministic systems enforce it | governed compilation (attest-policy, attest-runtime, operator-signed seal) vs the deterministic enforcement chain |
| Authority as a first-class object: issuer, jurisdiction, scope, delegation, version, effective period, constraints, evidence requirements | delegation objects + policy: scope, budget, expiry, revocation, versioned grants |
| "A consequential machine action should be attributable to the exact version of the computational authority that governed it" | the state-bound ALLOW: every decision carries the policy state digest it was evaluated against; execution re-verifies (TOCTOU-closed) |
| ESCALATE as an explicit state where authority is absent or unresolved | `restricted` mode (whole-plane fail-closed on tamper, drift, clock anomalies), `undelegated`, `budget_exhausted` |
| "Cryptography does not create authority. It protects the representation after authority has been established" | operator-signed attestations and seals over policy, secrets, and the artifact manifest |
| Evidence boundaries and independent verification | tamper-chain receipts, self-hashing evidence lineage, and the publication evaluator (mechanical independence rules R1–R5) |
| Humans establish, approve, delegate, revise, withdraw authority; machines enforce at speed | the delegation law — the human's lever is the grant, not the approval button |

## What the paper leaves open that CAUTEL closes

1. **Conserved authority.** The paper's delegation narrows; CAUTEL's
   delegation is a *budget that runs out* — one-shot spend ids,
   conservation checked on every op, revocation that stays dead. A sliver
   of authority usable infinitely many times is a liability; a spend-down
   balance is the difference.
2. **Hostile verification.** The paper argues the architecture; CAUTEL
   paid an external attacker to break it: round-3B, 13 claims, 12 held
   fail-closed, 1 real finding fixed before the round closed, re-run
   clean.

## The closing line

> The OECD submission describes the architecture. CAUTEL is the running
> implementation — with the two pieces the paper leaves open: conserved
> authority budgets, and receipts from an external attack round.
