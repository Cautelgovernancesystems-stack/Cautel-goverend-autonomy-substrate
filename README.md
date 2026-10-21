# CAUTEL — The Constitutional Runtime for Governed Autonomy

**Cautel Governance Systems**

> AI agents can decide what they want to do. They should not be able
> to grant themselves the authority to do it.

CAUTEL is a governance runtime for autonomous systems. It places
explicit authority enforcement between agent intent and execution,
with bounded delegation, mechanically evaluated policy, and mandatory
evidence for every decision.

Most agent architectures centre on a think-and-act loop. CAUTEL
introduces a governed execution lifecycle:

```
Think → Request Authority → Governance Review → Evidence Generated → Execute
```

## The constitutional model

1. **Authority cannot appear.** It must be granted.
2. **Authority can be delegated.** Only within the original grant.
3. **Delegation is constrained.** Scope and budget are checked at every step.
4. **Execution is bounded.** Out-of-bounds actions are denied before execution.
5. **Evidence is mandatory.** Every allow and every deny produces a receipt.
6. **When proof is impossible, CAUTEL fails closed.**

Workflow engines determine *what should happen next*. CAUTEL determines
*whether the actor has authority to make it happen*. LangGraph, CrewAI
and AutoGen address agent orchestration; CAUTEL targets a different
architectural responsibility: enforcing delegated authority at the
execution boundary.

> Cloud gave us compute. Kubernetes gave us orchestration.
> CAUTEL aims to provide governance for autonomous systems.

| | Human world | Autonomous world |
|---|---|---|
| Worker | Employee | Agent |
| Container | Human corporation | CAUTEL corporation |
| Contract | Employment agreement | Signed delegation of authority |
| Budget | Department budget | Delegation budget |
| Audit | Accounting records | Tamper-evident evidence chain |

## See it work

A governed day at **Nexus**, a demonstration company run entirely under
the enforcement engine — operator → departments → agents, real signed
authority lines, real budgets. The day ends **9 allowed, 5 denied,
every event receipted**: the overspend dies (`budget_exhausted`), the
out-of-scope hire dies (`scope_violation`), the terminated agent's
authority is dead (`revoked_authority`), the compromised department
cannot mint authority (`unauthorized`).

Full transcript and what each outcome proves:
[`CAUTEL_DUE_DILIGENCE/07-autonomous-corporation-demo.md`](CAUTEL_DUE_DILIGENCE/07-autonomous-corporation-demo.md)

The standing attack surface — the constitutional claims we believe hold,
each mapped to its battery — is published as a challenge:
[`CHALLENGE.md`](CHALLENGE.md). Weaknesses found are numbered, fixed and
retested in [`FINDINGS.md`](FINDINGS.md).

## Project status

- **Latest recorded validation:** 57 suites, 669 checks, zero failures
  (2026-10-21). Scope: adversarial governance tests, recovery and
  partition testing, evidence integrity, and formal property checks —
  see the [due-diligence package](CAUTEL_DUE_DILIGENCE/README.md).
- **External evaluation:** a 13-claim black-box round has been
  completed; 12 claims held fail-closed, one finding was fixed before
  the round closed.
- **Production integrations:** none operational.
- **Independent certification:** none claimed.

---

> **NOTICE** — Repository status: public documentation and evaluation
> materials only. The CAUTEL implementation is proprietary and is not
> included in this repository. No licence to use, modify, or
> redistribute the software is granted. See [License](License).

## Start here

- **[FAQ](docs/FAQ.md)** — threat model, recovery, rollback, guarantees
- **[Due-diligence package](CAUTEL_DUE_DILIGENCE/README.md)** — the 15-minute proof
- **[Claim-to-evidence matrix](docs/CLAIM_TO_EVIDENCE.md)** — every claim, its test, its limitation
- **[The challenge](CHALLENGE.md)** — have a crack
- **[Findings](FINDINGS.md)** — weaknesses hunted, numbered, fixed

## The hard questions, answered on the front page

Evaluators ask sharp questions. The direct answers — threat model,
recovery, rollback, external guarantees, validation scope, and the
operational record — are in **[`docs/FAQ.md`](docs/FAQ.md)**, not buried
in folders. The short version:

- **What is protected?** Application-level attackers, including code
  inside the plane's own sandboxes. **Not** root, and **not** a same-UID
  process (the operator key is owner-readable) — both excluded by
  explicit documentation.
- **Unattended recovery?** Yes — TPM-sealed operational unlock
  (machine as the device factor, tampered-boot fails closed) or an
  on-disk ops key fallback (cold-resume, not cold-path — stated).
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
| Operator key vault — n-of-n shares + hardware factor | ✅ shipped (v2), battery-tested |
| Unattended power-loss recovery — TPM-sealed or on-disk ops key | ✅ shipped, battery-tested; TPM path firmware-limited on some Intel fTPMs (documented) |
| Three-guard fleet (fast ring / full ring / airgap) with mutual liveness | ✅ shipped, battery-tested |
| Evidence evaluator with independence-checked publication rules (R1–R5) | ✅ shipped, battery-tested |
| External black-box round | ✅ 13 claims, 12 held fail-closed, 1 fixed before close — receipts published |
| **Current full sweep** | ✅ **57 suites, 669 checks — revocation-propagation timing (a lagging replica applies the same ordered log and converges to the same pruned state) + policy-conflict fail-closed behaviour (wildcards, inverted windows, dual-authority drains, contradictory requests) + cross-organisation federation (signed authority artifacts: export, verify against published roots, operator-mediated admission, propagated revocation, conservation across the boundary) + sealed audit-window tool (choose a from/to date-time, get a self-verifying evidence package with operator-HMAC token + boundary state digests) + autonomous-corporation demo (a governed day at Nexus: 9 allowed, 5 denied, every event receipted) + sovereign governance capture drill (majority departments + 40% agents + n−1 operator credentials: escalation, override and amendment attempts all fail closed) + economic decoupling battery (zero fees/gas/prices/stakes/tokens/markets in the enforcement path), game-theoretic adversary profiles + Byzantine-coalition drill, reproducible byte-identical builds, enforced pre-commit gate + flake harness, dependency SHA-256 locking + pip-audit, seed-replayable consensus, SMT-solver proofs (Z3), interpreter pinning + poisoned-builtins, constant-time audit, Ouroboros + kill -9 drills, mock-host fail-closed, property-based + differential search, wire-level partition drill (Jepsen-class), crypto known-answer vectors, raw-bytes wire fuzz, deep graph hunt, depth/type bombs + 1,000-command isolation drill — green (2026-10-21, latest recorded run)** |
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
