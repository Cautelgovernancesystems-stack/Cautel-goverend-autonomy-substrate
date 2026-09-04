# Fail-closed doctrine

CAUTEL's single most important rule:

> **When in doubt, refuse. When refused, record. When recorded, verify.**

## Triggers for refusal

- Any invariant violation, before, during or after execution
- Any delegation path that fails signature or window verification
- Any evidence chain segment that fails hash verification
- Any plane that fails a health check or cannot prove it is the right plane
- Any missing or stale attestation
- Any malformed, replayed or reordered message

## Consequences

1. The intent is refused — nothing executes.
2. The refusal is appended to the evidence DAG (refusals are evidence too).
3. Affected planes enter a **degraded** state: no new executions until the
   cause is verified and recorded as resolved.
4. Operators are alerted; the system does **not** self-heal silently.

Fail-closed is deliberately asymmetric: a false refusal costs availability;
a false approval costs everything.
