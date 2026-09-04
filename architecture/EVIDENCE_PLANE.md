# Evidence plane (conceptual)

The evidence plane records everything the other planes decided and did, in
an append-only structure whose integrity can be checked at any time.

```mermaid
flowchart TB
    D1[Decision: intent approved] --> D2[Execution record]
    D2 --> D3[Checkpoint: state hash]
    D3 --> D4[Decision: intent rejected]
```

Properties:

- **Append-only** — records are never modified in place.
- **Tamper-evident** — every record commits to its predecessors; any
  modification, deletion or reordering breaks the chain and is detectable.
- **Replayable** — the full state history can be re-derived from the chain
  and compared (see [`specs/EVIDENCE_DAG.md`](../specs/EVIDENCE_DAG.md)).

```mermaid
flowchart LR
    H[Chain head] -->|verify backward| N1[node]
    N1 -->|verify backward| N2[node]
    N2 -->|...| ROOT[root]
    style ROOT fill:#cfc
```

A chain that fails backward verification ⇒ the system treats its evidence as
untrustworthy ⇒ enforcement fails closed.
