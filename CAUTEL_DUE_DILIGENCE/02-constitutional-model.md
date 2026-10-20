# 02 — The Constitutional Model

CAUTEL's constitution is not a document. It is an executable pipeline in
which each stage is a hard gate. The five-stage chain:

```
Agent Identity → Authority → Delegation → Execution → Evidence
```

## Stage 1 — Agent Identity

Every principal (department or agent) holds a **key ring**, not a shared
token. Identity is cryptographic:

- each principal has key ids with rotation-safe resolution
- revoked keys are **permanently** denied (`I14`)
- credentials can expire; expired principals are denied (`I14`)
- task-scoped sessions (`sess-*`) are short-lived OIDC credentials that
  fail closed on expiry, revocation, or principal mismatch
- the enforcement envelope is signed over the exact payload — an
  unsigned or tampered request never reaches the authorisation stage

Proven by: OIDC identity battery **31/31**, loopback auth **24/24**,
chain auth **12/12**, constant-time audit.

## Stage 2 — Authority

Authority **cannot appear**. The only way authority enters the system is
an **operator-signed grant** — a signed, budgeted, scoped creation event.
There is no other path: a compromised department signing a grant gets
`unauthorized`, not authority.

Proven by: engine law (`only the operator may create authority`), the
governance-capture drill (30/30), adversary profiles (8/8).

## Stage 3 — Delegation

Authority moves only through **signed delegations**, and every
delegation is checked at apply time against the live chain:

- child scope must be inside the parent scope (no scope escalation)
- child budget must fit inside the parent's remaining authority
  (conservation — no multiplication, no splitting)
- depth capped; cycles denied; expiry never outlives the parent
- revocation is **transitive**: killing a node kills the whole subtree

Proven by: delegation drills **21/21**, property-based search
(7,774 ops / 10,000 garbage inputs), differential testing, graph hunt.

## Stage 4 — Execution

Execution is not the agent's choice — it is the outcome of the
enforcement chain:

```
signed envelope → clock sanity → authorisation → sequence → idempotency
→ admission control → state-bound ALLOW
```

Any failure at any gate denies **before execution** and records why.
In restricted mode — triggered by any tamper, drift, or anomaly —
nothing executes, including requests with otherwise valid envelopes.

Proven by: 52-suite battery including the 1,000-command isolation load
drill, the probe-guard tarpit, and the fail-closed restricted-mode drill.

## Stage 5 — Evidence

Every allow and every deny is receipted on an append-only, hash-linked,
HMAC-authenticated chain. Executions are sealed: pre-state digest,
ALLOW token, post-state digest, outcome. Point-in-time queries can
reconstruct what the law was and what it said for any past decision.
The chain is pinned by an offline airgap anchor — a rewrite anywhere
diverges at the pinned position.

Proven by: torn-tail 9/9, seal verify, airgap anchor check, ouroboros
(self-verifying sweep) 7/7.

## The doctrine

> When in doubt, refuse. A false refusal costs availability.
> A false approval costs everything.
