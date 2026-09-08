# Overview

CAUTEL is a **constitutionally governed execution plane**. It wraps an AI
agent (or any automation) so that:

1. **Authority is explicit.** A human root authority delegates capabilities
   through a signed delegation graph. No action executes without a provable
   path of authority.
2. **Execution is bounded.** Approved intents run inside an isolated
   execution boundary; anything outside the boundary is impossible by
   construction, not by convention.
3. **Evidence is tamper-evident.** Every accepted intent, every execution
   step, and every enforcement decision is appended to an evidence DAG that
   detects and resists tampering.
4. **Violations fail closed.** The constitutional engine continuously checks
   the rules. On any violation — or any inability to *prove* compliance — the
   system refuses execution.

These four properties are summarised as: **may it happen, did it stay inside,
can we prove it, and stop if not.**

## Current build state

The plane ships with: enterprise identity (OIDC + task-scoped sessions),
an MCP enforcement gateway, the evidence evaluator with publication rules
R1–R5 and confidential commitments, an operator key vault under m-of-n
dual control, layered runtime seals with hash-chained startup, and
bounded reason-code health surfaces. External validation: 13-claim
black-box round, 12 held fail-closed, 1 found and fixed before close.
See the roadmap for the full shipped/planned split.
