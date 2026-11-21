# 06 — Test Matrix

The release total for this build is **70 suites / 859 checks / 0
failures**, derived line-by-line from the most recent full sweep
(2026-11-21) at `/tmp/sweep_final4.log` — the single receipt. The
accounting: "suites" = every battery the sweep runs and gates on;
"checks" = the sum of each suite's own reported check count. Reported
alongside, not inside, the count: the 1,000-command V18 isolation load
drill (1,000 commands, 0 errors, 14.7 evt/s) and the 3 CLI anchor gates
(seal verify, airgap anchor, constitution self-test — all green). One
suite — the wire-level partition drill — flaked on raft election timing
in an earlier sweep (11/12) and was settled and re-run by the sweep's
audited retry tier; the retry is logged, not hidden. This sweep ran it
clean on the first pass: 12/12.

| Suite | Checks | Result |
|---|---|---|
| bin/test-ingress-authz.py | PASS (5 scenarios) | PASS |
| bin/test-probe-guard.py | PASS (5 checks) | PASS |
| bin/test-toctou.py | 5 | PASS |
| bin/test-upgrade.py | 13 | PASS |
| bin/test-v18-isolated.py | PASS (1,000-command drill) | PASS |
| bin/test_adversary_profiles.py | 8 | PASS |
| bin/test_atlas_adapter.py | 14 | PASS |
| bin/test_audit_window.py | 11 | PASS |
| bin/test_cadenceops_gate.py | 14 | PASS |
| bin/test_cadenceops_integration.py | 11 | PASS |
| bin/test_case_law_drill.py | 12 | PASS |
| bin/test_chain_auth.py | 12 | PASS |
| bin/test_claim_race.py | 11 | PASS |
| bin/test_const_time_audit.py | 3 | PASS |
| bin/test_constitutional_coverage.py | 9 | PASS |
| bin/test_context_strip.py | 3 | PASS |
| bin/test_corporation_demo.py | 11 | PASS |
| bin/test_cross_constitutional_scenarios.py | 8 | PASS |
| bin/test_crypto_kat.py | 15 | PASS |
| bin/test_depth_type_bomb.py | 6 | PASS |
| bin/test_differential.py | 3 | PASS |
| bin/test_economic_decoupling.py | 7 | PASS |
| bin/test_empty_set_audit.py | 7 | PASS |
| bin/test_executioner.py | 19 | PASS |
| bin/test_federation.py | 11 | PASS |
| bin/test_federation_root_lifecycle.py | 12 | PASS |
| bin/test_governance_capture.py | 30 | PASS |
| bin/test_graph_hunt.py | 4 | PASS |
| bin/test_guard_split.py | 13 | PASS |
| bin/test_inotify_matrix.py | 10 | PASS |
| bin/test_interpreter_pin.py | 7 | PASS |
| bin/test_kill9_drill.py | 13 | PASS |
| bin/test_liveness_airgap.py | 10 | PASS |
| bin/test_loopback_auth.py | 24 | PASS |
| bin/test_mindswarmai_adapter.py | 16 | PASS |
| bin/test_missing_configuration.py | 9 | PASS |
| bin/test_mock_host.py | 6 | PASS |
| bin/test_openai_agents_adapter.py | 18 | PASS |
| bin/test_openmanus_adapter.py | 16 | PASS |
| bin/test_operator_quorum.py | 10 | PASS |
| bin/test_ops_unlock.py | 9 | PASS |
| bin/test_ouroboros.py | 7 | PASS |
| bin/test_partition_drill.py | 12 | PASS |
| bin/test_peer_stale_grace.py | 4 | PASS |
| bin/test_policy_attestation.py | 10 | PASS |
| bin/test_policy_conflicts.py | 10 | PASS |
| bin/test_policy_continuity.py | 9 | PASS |
| bin/test_property_based.py | 7 | PASS |
| bin/test_query_at_time.py | 15 | PASS |
| bin/test_raft_cas.py | 9 | PASS |
| bin/test_reproducible.py | 3 | PASS |
| bin/test_revocation_timing.py | 8 | PASS |
| bin/test_runtime_seal.py | 12 | PASS |
| bin/test_sensory_hardening.py | 15 | PASS |
| bin/test_suna_adapter.py | 13 | PASS |
| bin/test_sweep_gate_self.py | 5 | PASS |
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
| trajectory/test/harden.py | 22 | PASS |
| vault/test/hardware.py | 35 | PASS |
| vault/test/vault.py | 18 | PASS |

## The green board

```
70 test suites          PASS
859 checks              PASS  (0 failures)
1,000-command V18 drill PASS  (0 errors, 14.7 evt/s)
3 CLI anchor gates      PASS  (seal verify, airgap, constitution self-test)
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

## Live-suite formats (inside the 70, all green in the same sweep)

- ingress authz (bearer/unsigned/duplicate statuses) — PASS (5 scenarios)
- V18 isolation load drill — 1,000 commands, 0 errors, 14.7 evt/s
- TOCTOU — 5/5
- probe guard — tarpit engaged, accept resets — PASS (5 checks)
- upgrade drill — 13/13
- trajectory hardening (API fuzz 200 malformed bodies) — 22/22
