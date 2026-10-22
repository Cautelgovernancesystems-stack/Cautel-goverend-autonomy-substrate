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

## Next hardening round (mandate from the constitutional root audit, 2026-10-22)

1. **Constitutional completeness assurance** — a distinct capability,
   not just more security tests: a coverage specification, the first
   three gap detectors (authority-root assurance, trust-root lifecycle,
   policy continuity), detectors tested against deliberately incomplete
   constitutions, and the passing-scan wording that says exactly what
   it proves. See docs/CONSTITUTIONAL_COMPLETENESS.md.
2. **m-of-n operator signing** — threshold signing for authority
   genesis, with genuinely independent keys, approval paths and
   recovery.
3. **Federation root lifecycle** — governed rotation, withdrawal and
   compromise handling for partner roots.
4. **Policy continuity rule** — defined grandfathering/migration
   semantics for grants across constitutional amendments.

## Explicitly not on the roadmap

- No token, staking, fee, or market mechanism — ever. The governance
  plane contains no economic participation mechanism and depends on no
  speculative value (battery-enforced).
- No "wisdom" claims — the plane proves authorisation, not the
  correctness of reasoning.
- No Byzantine-fault-tolerant consensus claim — censorship resistance
  beyond detection is not promised.
