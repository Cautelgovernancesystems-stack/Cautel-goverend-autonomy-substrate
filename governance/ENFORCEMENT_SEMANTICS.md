# Enforcement Semantics — the evidence half

The mechanical evaluator for publication of agent-produced claims. No
judgment, no prompts: every rule is a set operation over lineage records.

## Lineage

Every fact carries a lineage record (`evidence/lineage.py`): its source
rows, its producer, its ancestry (parent lineage ids), and a self-hash —
the record's id IS the hash of its content, so tamper changes the id and
the record fails verification. Ancestry links make "two views of the same
bytes" detectable: two confirmations descending from the same parent are
not independent, no matter how different their row-sets look.

## The publication rules (`evidence/evaluator.py`)

A claim is a record: `{claim_id, kind, domain_rows, production,
confirmations, n, unit, window, counterexamples}`. Publication of a claim
requires ALL of:

- **R1 independence** — every confirmation's input lineage is row-disjoint
  AND ancestor-disjoint from the claim's production lineage. "Did two
  things agree" is replaced by "do these two share a row or an ancestor."
  Refusal: `confirmation N not independent`, with the shared rows and the
  shared ancestor recorded in the receipt.

- **R2 scope bounding** — the claim declares its domain rows, and its
  production lineage must cover exactly that domain. A claim about all
  filings produced from six rows is refused unless it scopes itself to
  those six (`scope_unsupported`).

- **R3 per-claim isolation** — a multi-claim publication is refused if ANY
  claim fails. A true finding cannot lend its credibility to a sibling
  claim in the same release.

- **R4 counterexample blocker** — a universal claim ("all A are B") with
  any counterexample inside its own production sample is refused
  (`universal_claim_with_counterexamples`). "All A are B, except these
  two" is the same statement as "not all A are B"; only one survives
  being copied into an issue title.

- **R5 marker completeness + n-level** — the claim must carry its n, unit,
  and (for statistics) window at the level the claim is stated, and n must
  equal the production row count at that level. Missing markers
  (`unmarked_claim` — the old unmarked population) or a mismatched n
  (`n_mismatch` — quoting the filings scanned instead of the rule's own n)
  refuse the claim. An agent cannot quietly promote a claim while
  summarising it: restating the n is the feature, not the overhead.

## Receipts

Every evaluation produces a self-hashing receipt recording the production
lineage id, the domain, each confirmation's independence verdict, every
intersection, and the decision. The receipt lands in the tamper chain
(`evidence`, decision `publication_allowed`/`publication_refused`), so
"who allowed this claim to publish, under which rule" is answerable the
same way as every other action on the plane.

## Battery

`evidence/test/provenance.py` (16 checks) reproduces the failure shapes
this rule set exists to stop: the 120==120 confirmation loop, shared
ancestry behind different row-sets, the n=6 claim over the whole
population, the "except these two" universal, the wrong-level n, the
windowless statistic, unmarked legacy claims, tampered lineage, and
deterministic receipts.
