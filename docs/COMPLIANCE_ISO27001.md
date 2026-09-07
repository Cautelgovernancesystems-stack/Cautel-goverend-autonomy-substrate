# ISO/IEC 27001:2022 — Annex A Controls Reference

**What this document is.** A mapping of ISO/IEC 27001:2022 Annex A controls
to CAUTEL mechanisms, with an honest status for each. It is the
pre-work an auditor and a certification body expect before an ISMS audit.

**What it is not.** ISO 27001 certifies an *organization's* information
security management system — policies, risk register, internal audit,
management review. Code cannot be "certified"; code *satisfies* the
technological controls while the organisation operates the rest. Statuses:
**M** = satisfied by a CAUTEL mechanism · **P** = partially satisfiable,
needs organisational procedure · **O** = organisational control (no code
can satisfy it).

## A.5 Organisational controls

| control | CAUTEL answer | status |
|---|---|---|
| 5.1–5.4 policies, roles, responsibilities | Policy is constitutional: `constitution.json` is the written law; roles are signed principals (operator / principals / OIDC identities); any change is an attested, tamper-chained transaction | M |
| 5.8 security in project management | Every release runs the full battery sweep + `seal` before commit; the governed commit wrapper refuses un-audited commits | M |
| 5.15–5.18 access control + identity | OIDC-verified principals, ring keys with revocation, session credentials with 60s–24h TTL; identity confusion (IdP minting `operator`) is refused | M |
| 5.19–5.22 supplier/cloud relationship | N/A to a substrate; deployments document it | O |
| 5.24–5.28 incident management, evidence collection | Tamper chain is the incident record: every allow/deny/mint/revoke hash-linked; `restricted` mode is the containment control | M |
| 5.29–5.30 continuity, readiness | Fail-closed by construction: any unprovable condition denies service rather than degrading silently | M |
| 5.31–5.36 legal, privacy, audit | Replay + evidence chain answer "who allowed that, under what rule" — the audit-request artifact | M |
| 5.37 operating procedures | Operator cheatsheet + one-command round scripts | O |

## A.6 People controls

| control | answer | status |
|---|---|---|
| 6.1–6.8 screening, awareness, remote work, confidentiality | Organisation-side | O |
| 6.8 event reporting | Tamper chain records every governance event, including faults | M |

## A.7 Physical controls

| control | answer | status |
|---|---|---|
| 7.1–7.14 perimeters, entry, cabling, media, clear desk | Organisation-side; the substrate is indifferent to physical location | O |

## A.8 Technological controls

| control | CAUTEL answer | status |
|---|---|---|
| 8.2 privileged access rights | Operator key is the only root; rotation-safe key ids; revoked keys never resolve | M |
| 8.3 information access restriction | Policy grants are least-privilege-linted (wildcard actions root-only, sensitive actions scoped); authorise-before-execute on every action | M |
| 8.4 access to source code | Operator-signed runtime seal over the artifact manifest; drift detection on policy + secrets | M |
| 8.5 secure authentication | HMAC-signed envelopes; OIDC RS256 with weak-key refusal; task-scoped session secrets | M |
| 8.8 technical vulnerability management | External black-box attack rounds (round-3B: 13 claims, 12 held, 1 found+fixed) + internal attacker suites + 300+ battery checks per sweep | M |
| 8.10 information deletion | Sealed stores (raft state, sessions, delegation store) refuse tamper; destructive actions are gated commands, never expressible by default | M |
| 8.12 data leakage prevention | Payload gate: structural + semantic bounds at the execution boundary | M |
| 8.16 monitoring activities | Guardian observation cone; degraded visibility withdraws availability; every decision recorded | M |
| 8.17 clock synchronisation | V34 wall-clock sanity: rollback/forward jumps flip restricted mode | M |
| 8.19 installation of software on operational systems | Seal + attestation gate installs; change = governed transaction | M |
| 8.24 use of cryptography | HMAC-SHA256 throughout; signed attestations; hash-chained evidence ledger | M |
| 8.25 secure development lifecycle | Batteries + hardening drills run before every release; hostile-test discipline is mandatory | M |
| 8.29 security testing in development and acceptance | The full sweep (trajectory 22, delegation 24/30/11/22/21/15/46/21, guardian 27, semantic 17, attestation 8, OIDC 31, MCP 18, drills 46) | M |
| 8.31–8.32 development/test separation + change management | Governed commit wrapper: signed + verified transactions, global evidence sealed per change | M |
| 8.34 protection of information systems during audit testing | Evidence chain + replay let an auditor verify without touching the live plane | M |
| 8.1, 8.7, 8.9, 8.11, 8.13, 8.18, 8.20–8.23, 8.26–8.28, 8.30, 8.33 | Partially satisfiable at the substrate level; completed by deployment/organisational procedure | P |

## What certification still requires (honest gaps)

1. **ISMS documentation** — scope statement, information security policy,
   risk assessment methodology + register, statement of applicability.
2. **Organisational controls** — A.6 people, A.7 physical: screening,
   training, clear-desk, media handling.
3. **Process evidence** — internal audits, management reviews, corrective
   actions running over several months.
4. **A certification body** — an accredited registrar (e.g. in AU: JAS-ANZ
   accredited) performs stage 1 + stage 2 audits, then annual surveillance.

The mapping above means the technological half of Annex A is already
mechanically satisfied — the remaining work is the management system
around it, which is process, not product.
