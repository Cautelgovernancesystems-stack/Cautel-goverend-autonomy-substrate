# Independent Review Findings

The public ledger of every weakness found in CAUTEL — by our own
batteries and, going forward, by external reviewers. Format: Description
→ Impact → Fixed → Retested → Pass. We do not hide weaknesses; we hunt
them, number them, and turn each one into a permanent test.

## Findings from the internal hardening arc (self-found)

| # | Finding | Impact | Fixed | Retest |
|---|---|---|---|---|
| 001 | `verify_envelope` raised TypeError on a non-string `principal` (found by the depth/type bomb) | enforcement crash on hostile input | type gate before dict lookup | green (depth-type-bomb 6/6) |
| 002 | `seal.py` NameError broke the attestation verification path | the signed-seal gate could fail closed at boot | import fix + regression check (A9) | green (policy attestation 10/10) |
| 003 | Six constant-time violations (`==` on bearer tokens, HMACs, payload hashes, OIDC audience) | timing side-channel on auth material | `hmac.compare_digest` at all six sites | green (const-time audit 3/3) |
| 004 | Attestation deletion could downgrade to the pre-attestation bootstrap state | policy enforcement could be silently un-pinned | fail closed after first attest (A6) | green |
| 005 | Co-write forgery: edit the policy AND re-write the (unsigned) attestation | law could change on a running system without any key | operator-signed attestation | green (A2–A5) |
| 006 | Wire-level partition drill flaked under sweep load | zero-failure baseline rode on scheduler luck | select-based pump + widened retry windows + audited retry tier | green (12/12) |
| 007 | Z3/pip CVEs in the verification toolchain | the verifier itself was the weak link | pinned venv, `pip-audit` clean | green |
| 008 | Governance capture exposed that key material was still served while restricted | the governed clear could be attempted mid-tamper | secrets withheld while restricted; governed clear refused during restricted (now G5b) | green (capture drill 30/30) |
| 009 | Point-in-time query battery's fixed 60s cap no longer fit the 538k-block chain | real-chain smoke timed out as the evidence grew | subprocess headroom sized to chain growth | green (15/15) |

## External findings

*None yet — this section exists on purpose. The first external reviewer
who falsifies a line in [CHALLENGE.md](CHALLENGE.md) gets Finding #010
and a named credit here.*
