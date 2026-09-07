# Evidence evaluator — the proof half

The mechanical publication evaluator for agent-produced claims. Set
operations only — no prompts, no judgment.

- **Lineage** (`evidence/lineage.py` in the RC): every fact carries its
  source rows, producer, ancestry, and a self-hash — tamper changes the
  id, and the record fails verification.
- **Rules R1–R5**: confirmation independence (row- AND ancestor-disjoint),
  scope bounding, per-claim isolation, the universal-counterexample
  blocker ("except these two" is a refusal), and marker completeness with
  n counted at the level the claim is stated.
- **Receipts**: every evaluation produces a self-hashing receipt recording
  row-sets, intersections, and the decision, landed in the evidence chain.

The semantics doc: [`governance/ENFORCEMENT_SEMANTICS.md`](../governance/ENFORCEMENT_SEMANTICS.md).
The battery (16 checks) reproduces the round-3B community failure shapes:
the 120==120 confirmation loop, shared-ancestor "two views of the same
bytes", the n=6 claim over the whole population, the "except these two"
universal, the wrong-level n, and unmarked legacy claims.
