# The Case for Governed Autonomy

## Agents have outgrown oversight

An autonomous agent can form an intent, act on it, and move on — in less
time than a human takes to read the sentence. Prompt-level policy, code
review, and log files were built for software that waited for humans. Agents
don't wait. The oversight gap is structural, not a matter of effort.

## The three questions

Every agent action should be forced through three questions, in order:

1. **May it happen?** — authority must be *structural*: an unbroken, signed
   delegation path from a human root. Not a prompt that says "be careful."
2. **Does it stay inside?** — the boundary must be *constructive*: outside
   access is impossible by construction, not prevented by convention.
3. **Can we prove it?** — the evidence must be *tamper-evident*: an
   append-only, hash-linked record that any party can verify.

Most agent stacks answer these questions with vibes. CAUTEL answers them
with machinery.

## The asymmetry that matters

> A false refusal costs availability. A false approval costs everything.

Safety systems are usually tuned to avoid false alarms. CAUTEL inverts that:
the default answer to anything unverifiable is **refusal**, recorded as
evidence. That asymmetry is the entire product philosophy.

## What we claim

- Unauthorised action is *refused*, not warned about.
- Evidence that is modified, reordered, or deleted is *detectable*.
- The full state can be *replayed* from the evidence DAG alone.
- Every decision is *mechanical*: same inputs, same verdict, no discretion.

## What we don't claim

- CAUTEL cannot stop a compromised human root — no system can.
- CAUTEL is not a replacement for good delegation design; it is the
  enforcement layer that makes good design binding.
- CAUTEL is not a model-alignment technique; it governs what *runs*, not
  what the model thinks.

## Why now

Autonomous agents are moving from demos to deployments with real
consequences: trading, operations, infrastructure, customer data. The
organisations that adopt them first will be the ones who can answer the
three questions for every action — to regulators, auditors, and their own
boards. Governance is not the tax on autonomy. It is the licence to deploy
it.
