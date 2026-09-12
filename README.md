# CAUTEL — governed autonomy substrate

**Constitutionally governed execution for AI agents — with tamper-evident proof.**

CAUTEL is a governed autonomy substrate: a runtime in which agents can only
execute, delegate, evolve, and produce evidence under cryptographic
authority. Every action answers three questions before it happens:

1. **May it?** — an unbroken, signed delegation path from a human root
2. **Does it stay inside?** — execution within a constructive sandbox boundary
3. **Can we prove it?** — an append-only, tamper-evident evidence chain

And the doctrine that makes it different: **when in doubt, refuse.**
If any rule fails — or compliance cannot be *proven* — CAUTEL fails closed.

---

> **NOTICE** — This repository is a **public showcase** of CAUTEL. The
> software is proprietary, unlicensed for use, and is not distributed here.
> All material is provided for evaluation and information only. See
> [License](License).

## The hard questions, answered on the front page

Evaluators ask sharp questions. The direct answers — threat model,
recovery, rollback, external guarantees, validation scope, and the
operational record — are in **[`docs/FAQ.md`](docs/FAQ.md)**, not buried
in folders. The short version:

- **What is protected?** Application-level attackers, including code
  inside the plane's own sandboxes. **Not** root, and **not** a same-UID
  process (the operator key is owner-readable) — both excluded by
  explicit documentation.
- **Unattended recovery?** No. The key vault unlocks interactively
  (n-of-n shares + hardware factor). Resilience-for-control is a
  deliberate trade-off, stated as such.
- **Rollback protection?** Detection, not magic: the seal-pinned
  append-only ledger, the offline airgap anchor, the prefix-checked
  decision log, and replicated authority state with digest agreement make
  restoring an older snapshot fail closed.
- **Exactly-once into external systems?** Strict internally; at-most-once
  plus a stable `command_id` idempotency key externally. No distributed
  transactions — stated plainly.
- **External validation?** The 13-claim black-box round is real and its
  receipts are published — and it predates several current defenses.
  Validation is self-authored; there is no third-party certification.
- **Operational record?** Single-operator development plane; hours-to-a-day
  continuous runs; no production integrations; no outside operators yet.

## Status, stated plainly

| Claim | Status |
|---|---|
| Enforcement chain (sign → authorize → sequence → exactly-once → admit → state-bound allow) | ✅ shipped, battery-tested |
| Tamper-evident ledger with operator-keyed block auth beyond the seal pin | ✅ shipped, battery-tested |
| Keyed loopback endpoint auth (ledger pull, metrics, verdict, raft RPCs) | ✅ shipped, battery-tested |
| Exactly-once execution, replicated CAS over raft | ✅ shipped, battery-tested |
| Operator key vault — n-of-n shares + hardware factor | ✅ shipped (v2), battery-tested; interactive unlock only |
| Three-guard fleet (fast ring / full ring / airgap) with mutual liveness | ✅ shipped, battery-tested |
| Evidence evaluator with independence-checked publication rules (R1–R5) | ✅ shipped, battery-tested |
| External black-box round | ✅ 13 claims, 12 held fail-closed, 1 fixed before close — receipts published |
| **Current full sweep** | ✅ **28 suites, 425 adversarial checks + 1,000-command isolation drill — green (2026-09-12)** |
| Enterprise integrations (brokers, payments, storage) | ⏳ roadmap — none operational today |
| Federation of authority across organizations | ⏳ designed, not standardized |
| Third-party certification / independent audit | ❌ none — not claimed |
| Outside operators | ❌ none yet — not claimed |

## Repository map

| Section | What it proves |
|---|---|
| [`docs/`](docs/) | What CAUTEL is and why it exists — the front door |
| [`docs/FAQ.md`](docs/FAQ.md) | **The hard questions, answered directly** |
| [`docs/THESIS.md`](docs/THESIS.md) | The case for governed autonomy (thought leadership) |
| [`architecture/`](architecture/) | The three planes, subsystems, and the intent→execution→evidence lifecycle |
| [`specs/`](specs/) | Formal object definitions and governance invariants |
| [`governance/`](governance/) | The Constitution, enforcement semantics, fail-closed doctrine |
| [`validation/`](validation/) | Black-box methodology and result summaries |
| [`docs/DEMO.md`](docs/DEMO.md) | See it in action — recorded demonstrations |
| [`roadmap/`](roadmap/) | Federation model, replay engine, enterprise integrations |

## Proof of substance

- **Formal, not hand-wavy.** The [`specs/`](specs/) define the exact objects —
  AuthorityObject, delegation graph, evidence DAG, trajectory chain — with
  RFC-2119 MUST invariants.
- **Rules with teeth.** [`governance/CONSTITUTION.md`](governance/CONSTITUTION.md)
  is the machine-enforced rule set; [`ENFORCEMENT_SEMANTICS.md`](governance/ENFORCEMENT_SEMANTICS.md)
  shows evaluation is mechanical — no model judgement, no discretion.
- **Validated, not asserted.** [`validation/`](validation/) describes the
  hostile black-box matrix — tamper attempts, replay attacks, crash recovery —
  and the pass standard: *refused and recorded, or provably unchanged.*
  The external round held 12 of 13 claims fail-closed; the one finding was
  fixed before the round closed, re-run clean.
- **Fail-closed as doctrine.** [`governance/FAIL_CLOSED.md`](governance/FAIL_CLOSED.md):
  a false refusal costs availability; a false approval costs everything.

## See it in action

[`docs/DEMO.md`](docs/DEMO.md) — two recorded demonstrations:

1. **The Refusal** — an agent attempts an unauthorised action; CAUTEL refuses,
   records the refusal, and proves the evidence chain remains intact.
2. **A governed workload** — a day-trading agent operating under strict
   constitutional risk rules: capped risk per trade, automatic brackets,
   fail-closed filters, a full evidence journal, and a mandatory flatten
   before close.

## Who this is for

- **Enterprises** deploying autonomous agents and needing auditable control
- **AI-platform teams** wanting enforcement rather than prompt-level policy
- **Investors & partners** evaluating the governance layer of the agent economy
- **Researchers** working on agent safety, delegation, and tamper-evidence

## Talk to us

This showcase is the beginning of a conversation, not the end.

- **Demo request** — a guided walkthrough of the runtime in action
- **Licensing & partnership** — enterprise deployment, integration, federation
- **Feedback & research** — open an [issue](https://github.com/Cautelgovernancesystems-stack/Cautel-goverend-autonomy-substrate/issues)

📧 **paynescrossingpm@gmail.com**

---

*All rights reserved. No license to use the software is granted by this repository.*
