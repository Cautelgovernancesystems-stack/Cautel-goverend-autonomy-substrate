# Runtime Assurance for Enterprise Agents

DARPA's Assured Autonomy program solves a problem the enterprise now has
with AI agents: **how do you certify a non-deterministic controller?**

You cannot verify in advance what a learned system will do. The answer
DARPA and its partners (Galois, Vanderbilt, Imperial College London)
landed on is *Runtime Assurance* — an independent authority that monitors
the learner at execution time, holds formal safety bounds, and intervenes
when the system approaches an unsafe envelope. The learner proposes; the
assurance layer disposes.

CAUTEL is the same architecture, moved from the airframe to the agent.

## The mapping

| Runtime Assurance concept | CAUTEL mechanism |
|---|---|
| The learned controller (deep NN) proposes outputs | the agent proposes actions — text, tool calls, decisions |
| Formal safety bounds / reachable envelopes | the constitution: policy rules, budget law, semantic payload bounds — mechanically evaluated, never prompts |
| Independent monitor at execution time | the enforcement chain: envelope → clock sanity → signature → authorize → sequence → exactly-once → admission → state-bound ALLOW |
| Intervene before the unsafe envelope is entered | fail-closed refusal: any unprovable condition denies the action; whole-plane `restricted` mode on drift, tamper, or clock anomalies |
| Certification pathway for non-deterministic systems | the audit half: tamper-chain receipts, self-hashing evidence, replay — the artifact a certifier actually reads |
| Out-of-distribution anomalies | the guardian observation cone + evidence evaluator: claims must be confirmed on row- and ancestor-disjoint evidence, or they do not publish |

## Why the analogy is exact

Both systems face the same fact: **the learned component is never fully
verifiable, so the boundary must be enforced by something other than the
learned component.** A prompt cannot police a prompt. A neural network
cannot certify itself. In both cases the answer is a runtime authority
with higher precedence than the learner — one that refuses, not one that
advises.

The difference is the envelope. DARPA bounds physical reachability
(speed, altitude, collision). CAUTEL bounds organizational reachability:
which actions, on which resources, under which policy version, within
which budget, provable after the fact.

## The one-liner

> The same architectural answer DARPA uses to certify autonomous aircraft —
> an independent runtime authority that intervenes when the learner
> approaches an unsafe envelope — is what CAUTEL runs for enterprise
> agents. Runtime assurance for the enterprise.

## Evidence

External black-box attack round (round-3B): 13 claims against a frozen
build, 12 held fail-closed, 1 real finding fixed before the round closed,
re-run clean. Full battery sweep: trajectory 22, delegation
24/30/11/22/21/15/46/21, guardian 27, semantic gate 17, attestation 8,
OIDC identity 31, MCP gateway 23, evidence provenance 25, commitments 9,
hardening drills 46 — all green.
