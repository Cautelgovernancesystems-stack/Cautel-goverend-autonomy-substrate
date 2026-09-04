# Evidence DAG — specification

## Definition

The evidence DAG is the append-only, hash-linked record of every decision,
execution and checkpoint.

```
Record {
  type       : decision | execution | checkpoint
  id         : hash of (type, payload, predecessors)
  payload    : type-specific content
  predecessor: list of record ids (hash links)
  timestamp  : recording time
}
```

## Structure rules

1. **Append-only** — records are never edited or removed; corrections are
   new records referencing the old ones.
2. **Hash-linked** — every record commits to its predecessors; the chain
   head commits transitively to the root.
3. **Causal ordering** — a record's predecessors MUST already exist.
4. **Well-formed** — an execution record MUST have exactly one approving
   decision record among its ancestors.

## Tamper detection

Re-computing the hash chain from the root to the head MUST reproduce the
stored head. Any mismatch (modification, deletion, reordering, back-dating)
is detectable by any verifier.
