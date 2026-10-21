# The Constitutional Challenge — have a crack

CAUTEL is built on the assumption that there is always another hole.
This page is the attack surface, stated as a set of questions we believe
the system answers with **No** — and the exact battery that proves it.
Reviewers are invited to falsify any line below. A documented, falsified
line becomes a new test and a new entry in [`FINDINGS.md`](FINDINGS.md).

## Constitutional escape attempts

| # | The attack | The answer | The proof |
|---|---|---|---|
| E1 | Can authority appear? | **No.** Authority enters only via operator-signed grants; any other signer is `unauthorized`. | governance-capture drill, delegation drills |
| E2 | Can delegation create authority? | **No.** Conservation arithmetic at the apply point; multiplication/splitting denied. | property-based, differential, adversary profiles |
| E3 | Can two valid authorities combine into a larger one? | **No.** Every authority line is bounded to its own grant; a dual-holder drains to exactly the sum, never beyond. | policy-conflicts C7 |
| E4 | Can federation accidentally increase authority? | **No.** A foreign artifact is admitted as a local mirror with identical budget; conservation holds across the boundary. | federation F8 |
| E5 | Can revocation be delayed? | **No — at the law.** Revocation is transitive at apply; revocation and spending operations converge to identical replicated authority state under the tested lag-and-catch-up scenarios. *Boundaries, stated: convergence-after-catch-up is the tested property; "a stale replica cannot authorise while its view is stale" holds via Raft leader-completeness (only an up-to-date leader commits client operations — partition drill, coalition drill); a colluding replication majority could delay the revocation's commit (availability, no BFT) — never its effect once committed.* | revocation-timing 8/8 |
| E6 | Can evidence be selectively hidden? | **No.** Every allow and every deny is receipted; a torn chain tail is quarantined and refused, never skipped. | torn-tail, audit-window |
| E7 | Can audit windows be manipulated? | **No.** Operator-HMAC token over the manifest, boundary state digests, and a live-chain prefix check. | audit-window 11/11 |

## Software escape attempts (same discipline)

| # | The attack | The proof |
|---|---|---|
| S1 | Forge a signature / envelope | loopback auth 24/24, chain auth 12/12 |
| S2 | Replay a command | exactly-once (I2), claim race 11/11 |
| S3 | Race the budget | rational 100-thread race, property-based |
| S4 | Break distributed consistency | partition drill 12/12, raft CAS 9/9 |
| S5 | Tamper the policy and re-attest it | policy attestation A1–A10 |
| S6 | Tamper the evidence chain | torn-tail, block auth, airgap anchor |
| S7 | Forge a federation root or artifact | federation F2–F6 |
| S8 | Poison the interpreter or builtins | interpreter pin 7/7 |
| S9 | Tamper the release between builds | reproducible builds (4,606 byte-identical files) |
| S10 | Overload the ALLOW boundary | probe-guard tarpit, V18 1,000-command drill |

## The standing offer

The current full sweep is **57 suites / 669 checks / 0 failures**. Every
line above maps to a battery in that sweep. If you can falsify any
line — or find an attack we have not listed — the finding goes into
[`FINDINGS.md`](FINDINGS.md) with your name on it, and the fix becomes a
new test before the next release.
