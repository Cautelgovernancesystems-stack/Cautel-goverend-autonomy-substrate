# System purpose

## The problem

Modern agent stacks conflate three decisions:

- *Policy* — what is allowed — lives in prompts and code reviews.
- *Mechanism* — how things run — lives in the runtime.
- *Evidence* — what actually happened — lives in logs anyone can edit.

Conflation means: policy is advisory, evidence is weak, and a compromised or
misconfigured agent gets the benefit of the doubt.

## The threat model

CAUTEL assumes:

1. The agent may be **compromised** (prompt injection, model failure, bug).
2. The operator may be **absent** (long-running autonomous operation).
3. The evidence may be **attacked** (log tampering, replay, reordering).
4. The environment may be **hostile** (untrusted inputs, malicious tools).
5. An **application-level attacker** may read and modify plane files on the
   host, and may hold loopback network reach — but **does not hold the
   operator key**. Keyed integrity (operator HMACs on the tamper chain,
   raft RPCs, endpoint requests, and node-state responses) is what fails
   keyless forgery closed.

It does **not** assume protection against:

- the hardware root of trust being compromised;
- **root** or a **same-UID** process on the host — both can read the
  operator key (0600, owner-readable) and therefore forge the MACs; and
- an attacker who controls *all* trust anchors at once (the seal, the
  offline airgap media, and every replicated state node together).

Cryptography protects the representation of authority; it does not create
it. The precise wording of the boundary — what is covered, what is not,
and where the enforcer runs relative to the attacker — is in
[`FAQ.md`](FAQ.md).

## The design answer

Separate the planes, make authority structural, make evidence append-only
and tamper-evident, and make refusal the default. Details in
[`architecture/`](../architecture/) and [`specs/`](../specs/).
