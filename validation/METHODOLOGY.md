# Validation methodology

CAUTEL is validated **black-box**: testers interact only through the public
interfaces, exactly as an attacker would.

## Principles

1. **Hostile by default** — every test assumes the agent is compromised.
2. **Tamper attempts are first-class** — tests try to edit, delete, reorder
   and replay evidence.
3. **Reproducible** — every scenario has a scripted procedure and a recorded
   pass/fail.
4. **Summaries only** — raw logs stay internal; public pages report the
   outcome matrix.

## Standard test classes

| Class | Question |
|---|---|
| A — Unauthorised execution | Does anything run without valid authority? |
| B — Rule tampering | Can the constitution or constraints be altered outside change control? |
| C — Evidence tampering | Can records be changed without breaking verification? |
| D — Sequence attacks | Do duplicate/reordered/replayed messages succeed? |
| E — Crash & recovery | Does state survive plane failure without corruption? |

## Definition of pass

A scenario passes iff the hostile action is **refused and recorded**, or the
system state is **provably unchanged** after recovery — and the evidence
chain still verifies root-to-head.
