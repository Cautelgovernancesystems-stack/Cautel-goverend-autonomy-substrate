# Constitutional Completeness Assurance

The specification-to-be for CAUTEL's second assurance layer, written
down as the rationale from the first constitutional root audit
(2026-10-22). Status: **recorded, not yet implemented** — the frozen
release is untouched; this document is the mandate for the next
hardening round.

## The three layers

| Layer | Question | Assurance today |
|---|---|---|
| **L1 — Constitutional enforcement** | Does CAUTEL enforce the declared rules, authority boundaries, state transitions and evidence requirements correctly? | The existing machinery: 57 suites / 669 checks, 0 failures |
| **L2 — Constitutional completeness** | Have the necessary governance questions been answered? Are critical mechanisms declared? Are omissions, contradictions and undefined transitions detected? | **The gap this audit exposed.** No machinery exists. |
| **L3 — Domain correctness** | Are the requirements suitable for the business, industry, legal obligations and real-world consequences? | Irreducibly human. Expert judgement. |

The distinction matters: L2 can detect *known classes* of omission.
L3 determines whether the requirements make sense in the real world.
Neither replaces the enforcement layer.

**The audit's finding, in one sentence:** all 669 checks can pass while
a constitutional omission remains undetected — because the checks are
doing exactly what they were designed to do.

## The first three detector classes

| Detector | Question it must answer | Required result |
|---|---|---|
| Authority-root assurance | Does every authority tier have a declared signing and compromise model? | Explicit threshold, or documented exception |
| Trust-root lifecycle | Does every root-like credential have defined rotation, withdrawal and compromise handling? | Verifiable lifecycle, or documented exception |
| Policy continuity | Are grants bound to a policy version, or is their survival across amendments explicitly governed? | Defined migration/grandfathering semantics |

### Design principles (binding on the implementation)

1. **Silence is not a decision.** A missing grandfathering rule is
   reported as an omission requiring authorised resolution — the
   detector never invents the rule.
2. **Wiring evidence, not field names.** A configuration declaring
   `threshold: 2` while the signing path accepts one key must be
   flagged: the detector requires evidence that the declared mechanism
   is wired into enforcement, or it must clearly distinguish
   configuration coverage from implementation verification.
3. **The detectors are tested themselves.** Deliberately incomplete
   constitutions must be caught; false positives and misleading
   declarations must be rejected.
4. **Expert review stays separate.** Experts discover new gap classes;
   each discovery becomes a reusable detector. The catalogue grows as
   knowledge grows.
5. **The passing scan says exactly what it proves:** no known,
   catalogued gaps were detected. Not universal completeness. The same
   honesty the enforcement layer already carries.

## Why this matters at federation scale

When CAUTEL governs one boundary, a missing rule is important. In a
federated ecosystem of independently governed companies, departments
and agents, it is existential: the federation must explicitly define
trust withdrawal, delegated authority, policy changes, cross-company
accountability, and what happens when one participant's constitution
changes. Every builder can follow their own instructions flawlessly
while the federation as a whole builds something unsafe.

## Next-round mandate (when the freeze lifts)

1. Adopt the doctrine line into the governed whitepaper (draft agreed
   with the reviewer): *CAUTEL enforces the constitutional invariants
   and authority conditions defined by its governing configuration. It
   does not, by enforcement alone, establish the completeness,
   correctness, or suitability of those requirements for the governed
   domain. Responsibility for validating those requirements remains
   with the designated human governance authority, supported by
   appropriate domain expertise and evidence.*
2. Implement the three detectors against the constitutional coverage
   specification.
3. Test the detectors against deliberately incomplete constitutions.
4. Record each detector as a permanent assurance gate.
5. Address the three candidate findings themselves: m-of-n operator
   signing, federation root rotation/withdrawal, and the grandfathering
   rule.
