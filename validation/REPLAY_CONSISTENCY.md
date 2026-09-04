# Replay consistency summaries

## What replay proves

Replay re-derives the complete system state from the evidence DAG alone. If
replay reproduces state exactly, the chain *is* the truth of the system —
any hidden modification becomes visible as a mismatch.

## Consistency run format (template)

```markdown
- **Date:** YYYY-MM-DD
- **Range replayed:** {first_record} → {head}
- **Records:** {n}
- **State re-derived:** agents, delegations, executions, refusals, checkpoints
- **Mismatches:** 0
- **Chain verification:** root-to-head OK
- **Time to replay:** {duration}
```

## Latest summary (template)

| Run | Records | Mismatches | Verdict |
|---|---|---|---|
| {date} | {n} | 0 | CONSISTENT |
| {date} | {n} | 0 | CONSISTENT |

A non-zero mismatch count is treated as a safety incident and investigated
before any further execution.
