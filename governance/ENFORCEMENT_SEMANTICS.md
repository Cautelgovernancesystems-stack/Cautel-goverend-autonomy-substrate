# Enforcement semantics

How the enforcement engine evaluates an intent, in order:

1. **Parse** — the intent MUST match the intent schema. Malformed ⇒ refuse.
2. **Time** — the intent timestamp MUST be within the validity window of
   every AuthorityObject on its delegation path.
3. **Authority** — the delegation path MUST verify and terminate at the
   human root.
4. **Capability** — the requested action MUST be covered by the leaf
   capability.
5. **Invariants** — every governance invariant applicable to the action class
   MUST hold in the current state.
6. **Boundary preconditions** — the execution plane MUST confirm the boundary
   is healthy and ready.
7. **Record** — the decision (approve/refuse) MUST be appended to the
   evidence DAG *before* any execution proceeds.

Evaluation is **mechanical**: no model judgement, no heuristics, no
discretion. Rules are total functions of (intent, state) → {approve, refuse}.
