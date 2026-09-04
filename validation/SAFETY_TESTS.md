# Safety test results (summaries)

## Hostile matrix

The full matrix runs on every release candidate. Each cell is one hostile
scenario; the outcome is one of:

- **REFUSED** — attack blocked and recorded
- **DEGRADED** — system entered fail-closed degraded mode, no execution
- **PROVABLY UNCHANGED** — after recovery, replay reproduces the exact
  pre-attack state

## Summary table (template)

| Class | Scenarios | Blocked | Degraded | Escaped |
|---|---|---|---|---|
| A — unauthorised execution | {n} | {n} | 0 | 0 |
| B — rule tampering | {n} | {n} | {n} | 0 |
| C — evidence tampering | {n} | {n} | — | 0 |
| D — sequence attacks | {n} | {n} | — | 0 |
| E — crash & recovery | {n} | — | {n} | 0 |

**Escapes = 0** across all classes on the latest release candidate
({version}, {date}).

> Fill `{n}` from your actual suite-run logs (e.g. `cautel_attacker_output`
> summaries). Keep raw logs internal.
