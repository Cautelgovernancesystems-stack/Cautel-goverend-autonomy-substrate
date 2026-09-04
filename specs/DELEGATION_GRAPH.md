# Delegation graph — specification

## Definition

The delegation graph is a rooted, directed, signed graph whose nodes are
AuthorityObjects and whose edges are delegations.

```
root: human authority
edge: A → B  iff  B.issued_by == A
```

## Properties

1. **Rooted** — exactly one root: the human authority. Every node MUST have
   a path to the root.
2. **Acyclic** — delegations MUST NOT form cycles.
3. **Monotonic narrowing** — the capability set along any path from the root
   is non-increasing (delegation MAY narrow, never widen).
4. **Time-bounded** — every node has a validity window; the graph is
   evaluated *as of* the intent timestamp.
5. **Verifiable** — any observer can verify any path using only the stored
   signatures.

## Evaluation

An intent's claimed delegation path is **valid** iff every edge verifies,
every window contains the intent timestamp, and the leaf's capability covers
the requested action.
