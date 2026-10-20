# 01 — Executive Summary

## One sentence

**CAUTEL is the operating system for autonomous organisations** — the
governance layer that lets AI agents operate like employees inside a
governed corporation instead of acting as uncontrolled software.

## The problem

Every agent framework today is `Think → Act`. If the AI decides something
stupid, it can spend money, call APIs, modify systems, create risk — and
the organisation discovers it afterwards. Intelligence shipped before
governance.

## What CAUTEL changes

CAUTEL changes the pipeline to:

```
Think → Request Authority → Governance Review → Evidence Generated → Execute
```

An agent can propose anything. It cannot authorise itself to do anything.
Every accepted action carries a signed, provable path of authority; every
denied action is refused *before execution* and receipted as evidence.

## The six-line constitutional model

1. Authority cannot appear — it must be **granted**.
2. Authority can be **delegated** — but only within its grant.
3. Delegation is **constrained** — scope and budget, checked at every step.
4. Execution is **bounded** — denied before it happens when out of bounds.
5. Evidence is **mandatory** — every allow AND every deny is receipted.
6. When proof is impossible, CAUTEL **fails closed**.

Most agent frameworks do not attempt any of these.

## The category

Workflow engines ask *what should happen next*. CAUTEL asks *who has
authority to make that happen*. LangGraph, CrewAI and AutoGen are
orchestration systems; CAUTEL is a **constitutional runtime** — a
different layer of the stack:

> Cloud gave us compute. Kubernetes gave us orchestration.
> CAUTEL gives autonomous systems governance.

## The corporation vision

| | Human world | Autonomous world |
|---|---|---|
| Worker | Employee | Agent |
| Container | Human corporation | CAUTEL corporation |
| Contract | Employment agreement | Signed delegation of authority |
| Budget | Department budget | Delegation budget |
| Audit | Accounting records | Tamper-evident evidence chain |
| Discipline | HR process | Revocation — transitive, instant |

## Status (2026-10-18)

53 test suites, 629 checks, 0 failures — including a sovereign
governance-capture drill (majority-compromised departments and agents,
n−1 operator credentials: every escalation and amendment attempt fails
closed), a Byzantine-coalition drill, wire-level partition testing, and
a governed-corporation demonstration (see 07).
