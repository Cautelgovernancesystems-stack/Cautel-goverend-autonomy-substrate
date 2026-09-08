# CAUTEL — Whitepaper

## Abstract

Autonomous agents are moving from generating text to taking consequential
actions — moving money, modifying systems, acting on behalf of
organizations. The central problem is no longer "can the model follow
instructions" but *"under whose authority does this machine act, and what
enforces that boundary?"* CAUTEL answers it with structural authority
rather than advisory policy: every action is signed, authorized against a
versioned policy, budgeted, and receipted before it executes, and anything
unprovable is refused. This paper states the threat model, the design
principles, the three planes, the formal guarantees, and the validation
evidence — including an external black-box attack round in which 12 of 13
claims held fail-closed and the one finding was fixed before the round
closed.

## 1. Introduction — governance is a systems problem

Prompt engineering cannot solve governance. A guardrail written as an
instruction lives in the same channel an attacker can rewrite; a policy in
a document the agent reads is advisory by construction. The moment an
agent executes a side effect, the question becomes architectural: is there
a boundary the agent cannot talk its way past? That boundary is what
CAUTEL builds. The model proposes; the plane authorizes; the ledger
receipts. The three are different components, and the security property
lives in their separation.

## 2. Threat model

Four assumptions, stated formally:

1. **The model is an untrusted text generator.** It may be prompt-injected,
   hallucinating, or maliciously driven. No property of the system may
   depend on the model's intent.
2. **The agent's runtime is a hostile environment.** Code running alongside
   the agent — including injected code — may attempt to bypass, skip, or
   re-route enforcement.
3. **A same-host writer may modify any file on disk.** It does not hold the
   operator's signing key. Integrity, therefore, must fail closed when
   files change without the key, not merely detect the change.
4. **Human adjudication cannot scale to machine speed.** Humans set the
   boundary; machines enforce it; humans intervene only where authority is
   genuinely insufficient.

What the model explicitly does *not* protect against: an adversary who
holds the operator key. Cryptography protects the representation of
authority; it does not create it.

## 3. Design principles

1. **Structural authority over advisory policy.** Authorization is a
   deterministic evaluation in a separate process, not instructions the
   model reads.
2. **Fail-closed as the default.** Any unprovable condition — drift,
   tamper, clock anomaly, missing evidence — refuses the action, and
   whole-plane restricted mode refuses everything.
3. **Append-only, tamper-evident evidence.** Every decision lands in a
   hash-linked chain; receipts are self-hashing; the record cannot be
   quietly edited.
4. **Plane separation with sealed integrity.** Compilation of authority
   (governed, reviewed, signed) is separate from its execution
   (deterministic evaluation), and every enforcement layer is
   independently sealed and verified before the plane boots.

## 4. The constitutional plane

The law: a versioned policy, a key ring, and governed identity. Rules are
least-privilege-linted; changes to the law are signed, attested, and
sealed — a policy edit without the operator's signature is drift and the
plane refuses. Authority is delegated, not merely granted: a delegation is
a *conserved budget* with scope, expiry, and revocation — it runs out, and
once revoked it stays dead. See `governance/ENFORCEMENT_SEMANTICS.md`.

## 5. The execution boundary

"Inside" is defined by construction, not convention: the side effect has
exactly one path, through an executor that refuses without an allow. The
agent never holds raw credentials. A decision is bound to the exact policy
version and state it was evaluated against, and execution re-verifies —
the world cannot change between check and act. See
`architecture/EXECUTION_BOUNDARY.md`.

## 6. The evidence plane

Every decision produces a self-hashing receipt in an append-only chain:
who acted, under what rule, at what policy version, against what state,
consuming what budget. Publication of agent-produced claims is itself
governed: confirmations must be row- and ancestor-disjoint from the
claims' production lineage, n must be counted at the level the claim is
stated, counterexamples are blockers, and unmarked claims fail closed.
Receipts prove without leaking: sensitive values are replaced with salted
commitments whose openings stay with the data owner. See
`evidence/README.md` and `governance/ENFORCEMENT_SEMANTICS.md`.

## 7. Formal guarantees

The invariants, and exactly what they do not cover:

- **Guaranteed:** no unsigned action executes; no action executes outside
  its delegation; no delegation overspends its budget; no revoked
  authority resurrects; no decision cites a policy other than the one it
  was evaluated under; no tampered layer boots; every refusal and every
  allow is receipted.
- **Not covered:** correctness of the model's reasoning; truth of inputs
  the system has not itself verified; the actions of whoever holds the
  operator key. The plane proves *authorization*, not *wisdom*.

## 8. Validation

Black-box methodology: a frozen build, operator-signed fixtures, a fixed
claim list, receipts for every attempt, and a rule that findings become
invariants rather than shipping mid-round. The external round ran 13
claims across authorization, budget, revocation, finalization, evidence,
and identity surfaces: **12 held fail-closed, 1 real finding (unsigned
revert) found and fixed before the round closed, re-run clean.** Internal
batteries: 444 checks across 21 suites, all green. See `validation/`.

## 9. Limitations & future work

Federation of authority across organizations (portable artifacts, roots of
trust) is designed but not yet standardized; replay forensics at scale;
and the hardware-factor vault (device-bound unlock) is specified but not
yet shipped. See `roadmap/`.
