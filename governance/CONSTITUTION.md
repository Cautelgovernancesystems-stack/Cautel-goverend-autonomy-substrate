# The CAUTEL constitution

> The constitution is the single source of truth for what may execute.
> If a behaviour is not permitted here, it is forbidden.

## Article 1 — Authority

1.1. All authority originates with the human root. There is no authority
     without a signed, unbroken delegation path from the root.
1.2. Delegation may narrow capabilities but MUST NOT widen them.
1.3. Delegations are time-bounded and revocable; revocation propagates.

## Article 2 — Execution

2.1. Nothing executes except an authorised execution produced from an
     approved intent.
2.2. Execution occurs only inside the execution boundary, with only the
     inputs explicitly injected for that execution.
2.3. Executors hold no ambient credentials, filesystem or network access.

## Article 3 — Evidence

3.1. Every decision and execution MUST be recorded in the evidence DAG.
3.2. Records are append-only and hash-linked to their predecessors.
3.3. Any evidence that fails verification is treated as absent.

## Article 4 — Refusal

4.1. The default answer to any unverifiable condition is **refusal**.
4.2. A refusal is itself recorded as evidence.
4.3. Refusals MUST NOT be retried automatically unless a new, valid intent
     is formed.

## Article 5 — Change control

5.1. The constitution changes only through a signed amendment procedure with
     a recorded vote from authorised governors.
5.2. Amendments are versioned and the enforced version is recorded in the
     evidence chain.
