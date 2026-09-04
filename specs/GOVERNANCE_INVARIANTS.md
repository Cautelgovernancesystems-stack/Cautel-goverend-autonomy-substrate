# Governance invariants

Invariants are properties CAUTEL guarantees **or refuses to run**. They are
checked by the enforcement engine before and after every execution.

## Safety invariants

| # | Invariant |
|---|---|
| S1 | **No orphan authority** — every action traces to the human root. |
| S2 | **No amplification** — delegation narrows, never widens. |
| S3 | **Boundary containment** — executors receive only injected inputs; no ambient authority. |
| S4 | **Fail closed** — any unverifiable condition ⇒ refusal. |
| S5 | **Flat before close** — trading/economy-facing agents flatten positions before the session close (where applicable). |

## Evidence invariants

| # | Invariant |
|---|---|
| E1 | Every execution has exactly one approving decision ancestor. |
| E2 | The chain verifies from root to head at all times. |
| E3 | Replay reproduces state exactly (deterministic reconstruction). |

## Liveness invariants

| # | Invariant |
|---|---|
| L1 | Approved intents are either executed or recorded as refused — never silently dropped. |
| L2 | Health-check failures surface to operators within a bounded time. |

Each invariant maps to one or more constitutional rules in
[`governance/CONSTITUTION.md`](../governance/CONSTITUTION.md).
