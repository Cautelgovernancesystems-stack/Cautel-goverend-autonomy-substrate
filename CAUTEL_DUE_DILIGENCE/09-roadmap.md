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

5. **Multi-organisation federation** — portable authority artifacts and
   roots of trust across organisations (designed, not yet standardised).
6. **Population-scale policy simulation** — scenario-level replay exists
   today; population-level impact modelling is planned as an adoption
   integration.
7. **Case-law institutional adoption** — binding settled judicial
   interpretation is demonstrated at the mechanism level (case-law
   drill 12/12); the remaining dependency is institutional: who is
   authorised to encode settled interpretation.
8. **Production deployment** — managed hosting, multi-site redundancy,
   alerting (currently none — there is no remote to alert).

## Explicitly not on the roadmap

- No token, staking, fee, or market mechanism — ever. The governance
  plane contains no economic participation mechanism and depends on no
  speculative value (battery-enforced).
- No "wisdom" claims — the plane proves authorisation, not the
  correctness of reasoning.
- No Byzantine-fault-tolerant consensus claim — censorship resistance
  beyond detection is not promised.
