# 09 — Roadmap

What is next, and — with the same receipts discipline — what is honestly
not yet.

## Near term

1. **Hardware attestation per agent** — extend the interpreter pin /
   device-factor model so each agent principal can carry hardware-bound
   identity, not just key-ring identity.
2. **Reference hardware repair** — replace the drifting RTC (CMOS
   battery) that currently trips the clock-sanity gate; it is an
   availability nuisance the system correctly detects.
3. **External audit** — Trail of Bits / Sigma Prime / Least Authority
   class engagement. This is a business decision, not a code gap: the
   battery wall is designed to hand an auditor an attack catalogue and a
   claim list on day one.
4. **First enterprise demonstration customer** — the highest-value
   milestone per every advisor: a governed autonomous department doing
   real business work under CAUTEL (see 07 for the reference demo).

## Medium term

5. **Multi-organisation federation** — v1 demonstrated: portable,
   signed authority artifacts cross an organisational boundary (export,
   verify against published roots, operator-mediated admission, propagated
   revocation, cross-boundary budget conservation — 11/11). Remaining:
   PKI-based roots, key rotation, multi-hop federation.
6. **Population-scale policy simulation** — scenario-level replay exists
   today; population-level impact modelling is planned as an adoption
   integration.
7. **Case-law institutional adoption** — binding settled judicial
   interpretation is demonstrated at the mechanism level (case-law
   drill 12/12); the remaining dependency is institutional: who is
   authorised to encode settled interpretation.
8. **Production deployment** — managed hosting, multi-site redundancy,
   alerting (currently none — there is no remote to alert).

## First third-party integration — BUILT (2026-11-20)

CadenceOps (soul-thread-nexus) memory actions now run through the
enforcement chain via a typed adapter: scoped API keys map to CAUTEL
principals, authoritative writes (decisions, supersede, promote,
archive) are operator-gated regardless of key scope, and the full
claim → execute → commit exactly-once cycle is enforced. Battery:
11/11 (`bin/test_cadenceops_integration.py`). The reference integration
is hermetic — live credentials and deployment remain owner-gated.

## Constitutional round — COMPLETE (2026-11-21)

The audit's mandate is closed: the doctrine line is enshrined in the
whitepaper; the three gap detectors are live in the sweep; and the
three candidate findings are fixed with batteries — m-of-n operator
signing (10/10), federation root lifecycle (12/12), the grandfathering
rule (9/9). Remaining constitutional work: PKI-based federation roots,
multi-hop federation, and the human decision of whether to activate
2-of-3 on the live plane (owner-gated).

## Next hardening round (mandate from the constitutional root audit, 2026-10-22)

1. **Constitutional completeness assurance** — DONE (2026-11-20): the
   first three detectors are live (`bin/constitutional_coverage.py`),
   tested against deliberately incomplete constitutions (9/9), wired
   into the sweep, with the passing-scan wording. See
   docs/CONSTITUTIONAL_COMPLETENESS.md.
2. **m-of-n operator signing** — DONE (2026-11-20): threshold signing
   declared and enforced (`operator_quorum.threshold`), distinct-key,
   revoked-key-proof, back-compat — 10/10.
3. **Federation root lifecycle** — DONE (2026-11-20): generation-aware
   artifacts, rotation with grace, withdrawal, governed reactivation —
   12/12. Remaining: PKI-based roots, multi-hop federation.
4. **Policy continuity rule** — DONE (2026-11-20): `policy_continuity`
   declared (grandfather default, rebind mode enforced), grants record
   their policy version — 9/9.

## Explicitly not on the roadmap

- No token, staking, fee, or market mechanism — ever. The governance
  plane contains no economic participation mechanism and depends on no
  speculative value (battery-enforced).
- No "wisdom" claims — the plane proves authorisation, not the
  correctness of reasoning.
- No Byzantine-fault-tolerant consensus claim — censorship resistance
  beyond detection is not promised.
