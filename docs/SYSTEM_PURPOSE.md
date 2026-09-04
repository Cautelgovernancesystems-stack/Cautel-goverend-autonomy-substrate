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

It does **not** assume the hardware root or the human root authority are
compromised.

## The design answer

Separate the planes, make authority structural, make evidence append-only
and tamper-evident, and make refusal the default. Details in
[`architecture/`](../architecture/) and [`specs/`](../specs/).
