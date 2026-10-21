# 04 — Adversarial Testing

Methodology: a frozen build, operator-signed fixtures, a fixed claim
list, and a rule that every finding becomes a new invariant and a new
test before the round closes. Black-box where possible, white-box where
the claim is about the internals. The attack catalogue below is the
current battery wall; every row is a real engine output.

## Attack catalogue — expected → observed → evidence → result

| Attack class | Expected | Observed | Evidence generated | Result |
|---|---|---|---|---|
| Authority escalation — compromised department forges a grant | DENY | DENY (`unauthorized`) | yes | PASS |
| Authority escalation — compromised agent forges a grant | DENY | DENY (`unauthorized`) | yes | PASS |
| Scope escalation (book:AU → book:*) | DENY | DENY (`scope_violation`) | yes | PASS |
| Overspend beyond delegation budget | DENY | DENY (`budget_exhausted`) | yes | PASS |
| Compromised department forges an operator sweep | DENY | DENY (`unauthorized`) | yes | PASS |
| Non-delegator reduce on a foreign delegation | DENY | DENY (`unauthorized`) | yes | PASS |
| Coalition (51% departments, 40% agents, n−1 vault credentials) | no invariant change | foundational state byte-identical after capture | yes | PASS |
| Byzantine leader forges a log entry | commits to log, DENIED at apply | denied at apply on every honest node | yes | PASS |
| Colluding majority censors | guardian flags the censor | G2/G4 degrade; honest nodes converge | yes | PASS (availability boundary documented) |
| Fraudulent policy update (policy + co-written unsigned attestation) | restricted | refused, restricted mode | yes | PASS |
| Forged attestation signature | refused | `bad_attestation_signature` | yes | PASS |
| Attestation deletion after first attest | fail closed | `policy_attestation_missing` | yes | PASS |
| Emergency override abuse while restricted | nothing executes | even a valid signed envelope denied | yes | PASS |
| Constitutional amendment with n−1 vault shares | refused | `insufficient_shares` | yes | PASS |
| Forged operator HMAC (op-auth) | refused | `bad_auth` / expired / mismatch | yes | PASS |
| Replay of a spent command | DENY | `duplicate` / one-shot ids | yes | PASS |
| Wall-clock rollback / jump | fail closed | `clock_anomaly` → restricted | yes | PASS |
| Torn / truncated evidence chain tail | quarantine + refuse | quarantined verbatim, repaired under lock | yes | PASS |
| Raw-bytes wire fuzz | survive | 10/10, no crashes, conservative | yes | PASS |
| Depth/type bombs (`principal=[...]`, deep payloads) | structured denial | denied, never raised | yes | PASS |
| Probe the ALLOW boundary by denial flooding | throttled | identity tarpitted after threshold | yes | PASS |
| Economic vampire attack | N/A by construction | zero economic variables in the decision path | yes | PASS |
| Poisoned interpreter / builtins | detected | interpreter pin trips | yes | PASS |
| Supply-chain tamper between builds | detected | two builds byte-identical across 4,606 files | yes | PASS |
| Kill -9 at every lifecycle step | recover | 13/13, state + chain intact | yes | PASS |
| Revocation race on a lagging replica | stale spend denied on EVERY replica | denied (`revoked_authority`); identical pruned digests | yes | PASS |
| Policy conflicts (wildcards, inverted windows, dual authority, contradictory requests) | fail closed | rejected at load / denied at apply with structured codes | yes | PASS |
| Network partition / split brain | fail closed, converge after heal | 12/12 | yes | PASS |

## Failures found → patched → retested (this hardening arc, all on record)

| # | Finding (real) | Patch | Retest |
|---|---|---|---|
| 1 | `verify_envelope` raised TypeError on `principal=[...]` (depth/type bomb) | type gate before dict lookup | green |
| 2 | `seal.py` NameError broke the attestation path (found by A9) | import fix + regression check | green |
| 3 | Six constant-time violations (`==` on bearer tokens, HMACs, payload hashes) | `hmac.compare_digest` everywhere | const-time audit 3/3 |
| 4 | Attestation deletion could downgrade to bootstrap state | fail closed after first attest | A6 green |
| 5 | Co-write forgery (edit policy + re-write attestation) | operator-signed attestation | A2–A5 green |
| 6 | Partition drill flakiness under sweep load | select-based pump, widened retry windows | 12/12 |
| 7 | Z3/pip CVEs in the verification toolchain | pinned venv + `pip-audit` clean | green |
| 8 | Governance capture exposed: secrets are served while restricted | withheld — governed clear refused during restricted (now G5b) | green |

The full finding log lives in the governed commit history; each commit
is a patch applied with the retest result in the message.
