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
   step, and every enforcement decision is appended to an evidence chain
   that detects and resists tampering.
4. **Violations fail closed.** The constitutional engine continuously checks
   the rules. On any violation — or any inability to *prove* compliance — the
   system refuses execution.

These four properties are summarised as: **may it happen, did it stay inside,
can we prove it, and stop if not.**

## Current build state

The plane ships with: enterprise identity (OIDC + task-scoped sessions),
an MCP enforcement gateway, the evidence evaluator with publication rules
R1–R5 and confidential commitments, an operator key vault under n-of-n
dual control with a hardware-bound second factor, layered runtime seals
with hash-chained startup, bounded reason-code health surfaces, a
three-guard watch fleet (fast ring / full ring / airgap) with mutual
liveness, keyed loopback endpoint auth, operator-keyed block auth on the
tamper chain beyond the seal pin, exactly-once execution (atomic mark,
replicated CAS over raft), and a watchdog clock-drift tripwire.

Verification as of 2026-09-12: **28 battery suites, 425 adversarial
checks, plus the 1,000-command V18 isolation load drill and the CLI
anchors — all green.** External validation: a 13-claim black-box round,
12 held fail-closed, 1 found and fixed before close — receipts published
in [`validation/`](../validation/).

## The boundary, stated plainly

CAUTEL protects against application-level attackers — including code
running inside its own sandboxes — but **not** against root, and not
against a same-UID process (the operator key is owner-readable). There is
no unattended recovery (the vault unlocks interactively), exactly-once
does not extend past non-cooperative external services, and the system
has not yet been operated by anyone outside its development environment.
The full set of answers — recovery, rollback, external guarantees,
validation scope, and the operational record — is in
[`FAQ.md`](FAQ.md).

See the roadmap for the full shipped/planned split.
