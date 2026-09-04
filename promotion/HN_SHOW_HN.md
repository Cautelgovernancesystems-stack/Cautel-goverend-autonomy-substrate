# Show HN: CAUTEL — a constitution for autonomous agents (docs + specs + validation)

**Title:** Show HN: CAUTEL — fail-closed governance for autonomous agents

**Body:**

Agents act in milliseconds. Oversight takes minutes. That gap is the product.

CAUTEL is a governance substrate for autonomous agents: every action must
answer three questions before it runs —

1. **May it?** — an unbroken, signed delegation path from a human root
2. **Does it stay inside?** — execution in a constructive sandbox boundary
3. **Can we prove it?** — an append-only, tamper-evident evidence DAG

The core doctrine: **when in doubt, refuse.** Any rule violation — or any
condition that can't be *proven* compliant — fails closed. A false refusal
costs availability; a false approval costs everything.

What's in the repo (public by design, conceptual only — the runtime is
proprietary):

- Formal specs: AuthorityObject, delegation graph, evidence DAG, trajectory
  chain — RFC-2119 MUST invariants, not prose
- A 5-article Constitution with mechanical enforcement semantics
- Black-box validation methodology: hostile matrix (unauthorised execution,
  tamper attempts, replay attacks, crash recovery) where the pass standard is
  "refused and recorded, or provably unchanged"
- A fail-closed doctrine page and the case for governed autonomy

The design split that matters: policy lives in prompts today; CAUTEL makes it
structural. I'd genuinely value feedback on the invariant set — what should
governance guarantee that these five articles don't yet cover?

(No signup, no demo gating on the docs — the repo is the whole pitch.)
