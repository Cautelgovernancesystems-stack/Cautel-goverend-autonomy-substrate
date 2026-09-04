# Trajectory chain — specification

## Definition

The trajectory chain is the view of the evidence DAG restricted to one
agent's **accepted actions over time**: the provable record of what an agent
was authorised to do and did do, in order.

```
TrajectoryEntry {
  agent        : identity
  seq          : monotonically increasing per agent
  intent       : reference to the intent record
  decision     : reference to the decision record
  execution    : reference to the execution record (if any)
}
```

## Invariants

1. Sequence numbers per agent MUST be contiguous (no gaps ⇒ no silent
   deletions).
2. Every trajectory entry MUST resolve to real evidence records.
3. Two entries MUST NOT share a sequence number.
4. Replaying the evidence DAG MUST reproduce the trajectory chain exactly.
