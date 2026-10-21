# Reviewer Kit — start here

Ten minutes from here to your first attack. No prior knowledge of the
repository required.

## 1. The claim in one paragraph

CAUTEL is a governance runtime for autonomous systems. Authority enters
only via operator-signed grants, moves only via budgeted, scoped,
signed delegations, and is consumed only by one-shot signed spends.
Every allow and every deny is receipted on a tamper-evident chain. The
claims under test are in [`CHALLENGE.md`](CHALLENGE.md); each claim maps
to its mechanism, test and limitation in
[`docs/CLAIM_TO_EVIDENCE.md`](docs/CLAIM_TO_EVIDENCE.md).

## 2. Scope, stated once

CAUTEL claims to enforce authority properties against application-level
attackers — including code running inside its own sandboxes. It does
**not** claim protection against: root, same-UID processes, the holder
of all operator material, physical access, or speculative-execution
(Rowhammer-class) attacks (detection only). A colluding replication
majority can censor availability; it can never fabricate authority. See
[`CAUTEL_DUE_DILIGENCE/08-architecture.md`](CAUTEL_DUE_DILIGENCE/08-architecture.md)
for the full trust model.

## 3. Pick one property and attack it

| Claim | What a violation would look like |
|---|---|
| Authority cannot appear (E1) | any accepted object whose chain does not root at an operator-signed grant |
| Delegation cannot amplify authority (E2/E3) | spent + live remaining exceeding a grant's budget |
| Scope escalation is denied (E4) | an accepted spend outside its delegation's scope |
| Revocation cannot be bypassed (E5) | a spend accepted on any replica after its revocation committed |
| Evidence cannot be silently removed (E6) | a receipt missing for an allow/deny, without the chain tail being quarantined |
| Audit artifacts cannot be altered undetectably (E7) | a saved audit-window package that still verifies after tampering |
| Federation cannot inflate authority (F-series) | an admitted foreign artifact with more authority than it was granted |
| Replay is refused (S2) | one command identity committing twice |
| Policy conflicts fail closed (C-series) | ambiguity silently resolving into permission |

Each violation has a signature — a decision code that must not exist, an
invariant that must not break, two replicas that must not diverge. If
you observe the signature, you have a finding, not a theory.

## 4. Two risks, tested separately

A successful attack on the **implementation** does not disprove the
**category**; agreement that the category matters does not validate the
implementation. Please state which of the two your feedback addresses —
both are wanted, they are just scored separately.

## 5. Scope agreement

Self-contained review of the published materials requires no
coordination. Any access to a live system, its data, or non-public
materials happens only after a written scope agreement — negotiated
first, never assumed.

## 6. How to report a finding

Description → Impact → Reproduction steps → Suggested test. Findings
land in [`FINDINGS.md`](FINDINGS.md) with credit, and the fix becomes a
permanent test before the next release. Unresolved disagreements are
recorded too — a documented disagreement is also a result.

## 7. Reproduce the reported results

The full sweep is **57 suites / 669 checks / 0 failures (2026-10-21)** —
a *reported* result, not independent validation; that is precisely what
you are invited to provide. Per-suite final lines are reproduced in
[`CAUTEL_DUE_DILIGENCE/06-test-matrix.md`](CAUTEL_DUE_DILIGENCE/06-test-matrix.md).
The public repository contains documentation and evaluation materials;
the implementation is proprietary and not distributed here.
