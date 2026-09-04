# CAUTEL — governed autonomy substrate

**Constitutionally governed execution for AI agents — with tamper-evident proof.**

CAUTEL is a governed autonomy substrate: a runtime in which agents can only
execute, delegate, evolve, and produce evidence under cryptographic
authority. Every action answers three questions before it happens:

1. **May it?** — an unbroken, signed delegation path from a human root
2. **Does it stay inside?** — execution within a constructive sandbox boundary
3. **Can we prove it?** — an append-only, tamper-evident evidence DAG

And the doctrine that makes it different: **when in doubt, refuse.**
If any rule fails — or compliance cannot be *proven* — CAUTEL fails closed.

---

> **NOTICE** — This repository is a **public showcase** of CAUTEL. The
> software is proprietary, unlicensed for use, and is not distributed here.
> All material is provided for evaluation and information only. See
> [License](License).

## Repository map

| Section | What it proves |
|---|---|
| [`docs/`](docs/) | What CAUTEL is and why it exists — the front door |
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
