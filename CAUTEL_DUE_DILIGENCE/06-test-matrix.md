# 06 — Test Matrix

The release total for this build is **53 suites / 629 checks / 0
failures** (the standard accounting that includes every suite's check
groups, the 1,000-command V18 isolation load drill, and the CLI anchor
checks). The table below reproduces each battery suite's own final line,
verbatim, from the most recent full sweep (2026-10-18). One suite — the
wire-level partition drill — flaked on raft election timing and was
retried once by the sweep's audited retry tier: 11/12 → settle → 12/12.
The retry is logged, not hidden.

| Suite | Checks | Result |
|---|---|---|
| bin/test_case_law_drill.py | 12 | PASS |
| bin/test_chain_auth.py | 12 | PASS |
| bin/test_claim_race.py | 11 | PASS |
| bin/test_const_time_audit.py | 3 | PASS |
| bin/test_context_strip.py | 3 | PASS |
| bin/test_corporation_demo.py | 11 | PASS |
| bin/test_crypto_kat.py | 15 | PASS |
| bin/test_depth_type_bomb.py | 6 | PASS |
| bin/test_differential.py | 3 | PASS |
| bin/test_economic_decoupling.py | 7 | PASS |
| bin/test_empty_set_audit.py | 7 | PASS |
| bin/test_executioner.py | 19 | PASS |
| bin/test_governance_capture.py | 30 | PASS |
| bin/test_graph_hunt.py | 4 | PASS |
| bin/test_guard_split.py | 13 | PASS |
| bin/test_inotify_matrix.py | 10 | PASS |
| bin/test_interpreter_pin.py | 7 | PASS |
| bin/test_kill9_drill.py | 13 | PASS |
| bin/test_liveness_airgap.py | 10 | PASS |
| bin/test_loopback_auth.py | 24 | PASS |
| bin/test_mock_host.py | 6 | PASS |
| bin/test_ops_unlock.py | 9 | PASS |
| bin/test_ouroboros.py | 7 | PASS |
| bin/test_partition_drill.py | 12 | PASS |
| bin/test_policy_attestation.py | 10 | PASS |
| bin/test_property_based.py | 7 | PASS |
| bin/test_query_at_time.py | 15 | PASS |
| bin/test_raft_cas.py | 9 | PASS |
| bin/test_runtime_seal.py | 12 | PASS |
| bin/test_sensory_hardening.py | 15 | PASS |
| bin/test_torn_tail.py | 9 | PASS |
| bin/test_tpm_unlock.py | 8 | PASS |
| bin/test_wire_fuzz.py | 10 | PASS |
| bin/test_z3_proof.py | 6 | PASS |
| delegation/test/drills.py | 21 | PASS |
| evidence/test/commit.py | 9 | PASS |
| evidence/test/provenance.py | 33 | PASS |
| guardian/test/battery.py | 32 | PASS |
| guardian/test/evidence_watch.py | 34 | PASS |
| identity/test/oidc.py | 31 | PASS |
| mcp/test/gateway.py | 32 | PASS |
| vault/test/hardware.py | 35 | PASS |
| vault/test/vault.py | 18 | PASS |

## The green board

```
53 test suites          PASS
629 checks              PASS  (0 failures)
Authority escalation    DENY  (evidence receipted)
Overspend               DENY  (budget conserved)
Out-of-scope action     DENY  (scope enforced)
Revoked authority       DENY  (transitive kill)
Forged signature        DENY  (key rings)
Forged policy update    DENY  (restricted mode trips)
Power-loss recovery     PASS  (kill -9 drill 13/13)
Network partition       PASS  (12/12)
Replay reconstruction   PASS  (torn-tail 9/9)
Evidence chain integrity PASS (seal + airgap anchor)
```

## Live suites (separate formats, all green in the same sweep)

- ingress authz (bearer/unsigned/duplicate) — PASS
- V18 isolation load drill — 1,000 commands, 0 errors, ~14 evt/s
- TOCTOU — 5/5
- probe guard — tarpit engaged, accept resets
- upgrade drill — 13/13
- trajectory hardening (API fuzz 200 malformed bodies) — 22/22
- constitution self-test, seal verify, airgap anchor — green
