# How the hardened layers were found — the public build log

The best parts of this plane were not designed in isolation. They were
found in public, under fire. This page records the exchanges that became
shipped, sealed, battery-tested code — so the origin of each mechanism is
as auditable as the mechanism itself.

## The CIK exchange → the evidence evaluator (R1–R5)

A practitioner posted the post-mortem of an agent that published a false
"rule" to four repositories: the 120 CIKs that *produced* the rule were
the same 120 that *confirmed* it. His own conclusion — "the check has to
come from a source the hypothesis has not touched" — became the
specification for the publication evaluator:

- R1 independence: confirmations must be row- AND ancestor-disjoint from
  production lineage ("two views of the same bytes" dies on the
  shared-ancestor check);
- R2 scope bounding: a claim must declare its domain, and production must
  cover it (the n=6 rule over the whole population is refused);
- R3 per-claim isolation: a true finding cannot lend credibility to a
  sibling claim;
- R4 the counterexample blocker: "all A are B, except these two" is
  refused — it is the same statement as "not all A are B";
- R5 markers + n at the claimed level: a statistic without a window, or an
  n quoted at the wrong level, is refused.

The battery reproduces his exact failure shapes (the 120==120 loop, the
n=6-over-population rule, the hour-of-day "except two"). See
`governance/ENFORCEMENT_SEMANTICS.md`.

## The linkage critique → reason codes + commitments

A reviewer on the delegation-passport discussion raised two objections:
content-blind receipts still leak *metadata* linkage, and a fail-closed
system must let the operator read *why* it refused. Both became shipped
features:

- per-receipt random salts (cross-receipt correlation via the commitment
  dies), with coarsening knobs stated per guarantee;
- a bounded, documented reason-code vocabulary on health surfaces
  (CLOCK_ANOMALY / ATTESTATION_FAILURE / TAMPERED_STORE /
  ENFORCEMENT_FAULT / RESTRICTED), raw fault detail kept internal, and a
  redaction test matrix — untrusted callers get code-only, operators get
  the full reason, and corrupt flags fall back to the generic code, never
  null.

## The state-binding question → the state-bound ALLOW

A commenter asked whether execution should bind to the exact
assurance-state version that was checked, so a change between validation
and execution fails the action. That is the state-bound ALLOW: every
decision carries the policy-state digest it was evaluated against, and
execution re-verifies against the current state — "state changed" denies,
no silent grandfathering.

## The NOMOS / OECD submission → the computable-authority mapping

An independent OECD consultation submission argued for
"machine-computable authority": sealed authority artifacts, deterministic
deny-by-default evaluation, compilation separate from execution. The
mapping of that thesis to this implementation is published in
`docs/COMPUTABLE_AUTHORITY.md` — including the two things the paper leaves
open: conserved authority budgets, and hostile-round receipts.

## The public red team → six bypasses, zero held

Another builder posted an open "break my prototype" challenge against his
own authorization library. We ran the structured pass: six categories
(authorization, argument, state-poisoning, execution-path, replay,
integration gap) — six bypasses demonstrated, each with start state,
steps, decision, execution, and why-it-bypasses. The lesson that became
our own hardening: an enforcement boundary inside the constrained process
is a suggestion, not a boundary.

## The pattern

Every one of these started as a stranger's critique and ended as a sealed,
battery-tested mechanism. That is the operating doctrine: concede the real
gaps in public, ship them, and return with receipts.
