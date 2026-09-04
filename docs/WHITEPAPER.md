# CAUTEL — whitepaper (draft skeleton)

> Fill each section with conceptual argument. No implementation detail.

1. **Abstract** — four sentences: problem, approach, guarantees, scope.
2. **Introduction** — why agent governance is a *systems* problem, not a
   prompt-engineering problem.
3. **Threat model** — the four assumptions in
   [SYSTEM_PURPOSE.md](SYSTEM_PURPOSE.md), stated formally.
4. **Design principles** —
   - Structural authority over advisory policy
   - Fail-closed as the default
   - Append-only, tamper-evident evidence
   - Plane separation with sealed messaging
5. **The constitutional plane** — rules, delegation, enforcement semantics
   (link to [`governance/`](../governance/)).
6. **The execution boundary** — what "inside" means and why the boundary is
   constructive, not conventional (link to
   [`architecture/EXECUTION_BOUNDARY.md`](../architecture/EXECUTION_BOUNDARY.md)).
7. **The evidence plane** — DAG structure, tamper detection, replay
   (link to [`specs/EVIDENCE_DAG.md`](../specs/EVIDENCE_DAG.md)).
8. **Formal guarantees** — the invariants, and exactly what they do *not*
   cover (link to [`specs/GOVERNANCE_INVARIANTS.md`](../specs/GOVERNANCE_INVARIANTS.md)).
9. **Validation** — black-box methodology and summary results
   (link to [`validation/`](../validation/)).
10. **Limitations & future work** — federation, replay improvements
    (link to [`roadmap/`](../roadmap/)).
