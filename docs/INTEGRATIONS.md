# Integration Contracts — the handover repos, mapped to CAUTEL

The eight repositories delivered in the engineering handover
(2026-11-19) are reference material with cleared rights. This document
records, for each, exactly what CAUTEL has adopted, what is mapped as a
reference, and what remains owner-gated. The private repositories'
code never leaves the governed tree; only these mappings are public.

## 1. OpenAI Agents Python — governed tool adapter (ADOPTED)

**Status: integrated.** `integration/openai_agents_adapter.py` wraps the
SDK's `function_tool` contract with the constitutional boundary:

```
agent calls tool -> proposal (envelope for the registered principal,
action/resource declared at registration) -> enforcement chain ->
ALLOW: tool executes, result carries the receipt (command_id, state
digest, allow token), execution COMMITS -> DENY: the tool never runs;
the agent receives the structured denial
```

Properties proven by `bin/test_openai_agents_adapter.py` (18/18):

- the approval is bound to the **canonical action representation**:
  approve X, mutate to Y, execution fails at the envelope layer
  (`payload_mismatch`) and at the claim layer (`command_conflict`)
- the crash cycle: approve -> claim -> worker dies -> restart ->
  reconcile continues the SAME claim -> commit -> replay hard-denied
- unauthorized proposals never execute; restricted mode denies even
  valid proposals; every decision is receipted; the law is
  deterministic

The wrapper is SDK-compatible (type/name/description/params_json_schema/
on_invoke_tool) and works with or without the SDK installed.

## 2. CadenceOps (soul-thread-nexus) — memory-write contract (ADOPTED)

**Status: adapter integrated.** `integration/cadenceops_adapter.py`
governs every memory-API write. The contract:

| Memory-API endpoint | CAUTEL decision path |
|---|---|
| `GET /healthz /identity /context /decisions /handover/latest /knowledge/search /project-state /conflicts` | read-scope key -> `telemetry` principal |
| `POST /observations /knowledge/proposals` | proposal-scope key -> `chaos` principal; envelope + authorize + admission + exactly-once |
| `POST /decisions /handover /project-state /records/:id/supersede /records/:id/promote /records/:id/archive` | **operator-gated regardless of key scope** — authoritative writes |
| replay of any committed command | `duplicate` |

Battery: 11/11. The memory-API itself (M0) is not deployed — Render
provisioning remains owner-gated.

## 3. ZeroClaw — execution-restriction reference (MAPPED)

ZeroClaw is a Rust agent runtime built for execution restrictions and
resource lifecycle on minimal hardware. The mapping to CAUTEL:

| ZeroClaw concern | CAUTEL equivalent |
|---|---|
| execution restrictions per tool | the enforcement chain: deny-before-execute, scope + budget |
| resource lifecycle | delegation expiry, the expire sweep, revocation (transitive) |
| low-footprint runtime | the deterministic pure-law engine (no RNG, no wall clock in apply) |
| per-user isolation | per-principal key rings + task-scoped `sess-*` credentials |

No code is adopted — the mapping documents where ZeroClaw's patterns
already exist in CAUTEL's law. Future adoption candidates: its
sandbox-adjacent lifecycle patterns, reviewed at integration time.

## 4. Hermes Agent Zero Auth — gateway-pattern reference (MAPPED)

Hermes provides a JWT gateway with per-user namespace isolation for
multi-tenant agent deployments. The mapping:

| Hermes pattern | CAUTEL equivalent |
|---|---|
| JWT access tokens (24h) + refresh (7d) | task-scoped `sess-*` OIDC credentials (short-lived, fail closed) |
| trusted header derived ONLY from the verified JWT | the signed envelope — identity is never taken from an unverified header |
| per-user namespace isolation | per-principal authority scopes; cross-scope denied |
| admin panel manages users | operator-gated authoritative actions (grants, revocation, expiry) |

The honest contrast: Hermes isolates *tenants*; CAUTEL governs
*authority*. A future integration would place CAUTEL behind Hermes'
gateway, not replace it — the gateway proves who you are, CAUTEL proves
what you may do.

## Owner-gated (no action without Andrew's approval)

- Render/Memory-DB provisioning for CadenceOps M1
- Any deployment of the governed adapters against live credentials
- Publishing any code from the private repositories
- Activating 2-of-3 operator quorum on the live plane
