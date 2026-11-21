# Integration Contracts — the handover repos, mapped to CAUTEL

The eight repositories delivered in the engineering handover
(2026-11-19) are reference material with cleared rights; ATLAS and
mindswarmai additionally carry written grant rights from the original
author. This document records, for each, exactly what CAUTEL has
adopted (code-level integration), what is mapped as a pattern reference
(no code adopted), and what remains owner-gated. The private
repositories' code never leaves the governed tree; only these mappings
are public.

| Repo | Status | Battery / evidence |
|---|---|---|
| OpenAI Agents Python | ADOPTED | 18/18 (`bin/test_openai_agents_adapter.py`) |
| CadenceOps (soul-thread-nexus) | ADOPTED | 11/11 (`bin/test_cadenceops_integration.py`) |
| ZeroClaw | MAPPED | pattern reference (no code adopted) |
| Hermes Agent Zero Auth | MAPPED | pattern reference (no code adopted) |
| OpenManus | ADOPTED | 16/16 (`bin/test_openmanus_adapter.py`) — governed tool-call choke point |
| suna | ADOPTED | 13/13 (`bin/test_suna_adapter.py`) — governed trigger -> sandbox gate |
| ATLAS | ADOPTED | 14/14 (`bin/test_atlas_adapter.py`) — governed pre-trade filter chain as authority |
| mindswarmai | ADOPTED | 15/15 (`bin/test_mindswarmai_adapter.py`) — governed swarm-run entitlements |

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


## 5. OpenManus — agent tool-call framework (ADOPTED)

OpenManus is a general-purpose AI-agent framework (ReAct loop,
ToolCallAgent, planning flow) from the MetaGPT lineage. The mapping to
CAUTEL:

| OpenManus concern | CAUTEL equivalent |
|---|---|
| `ToolCallAgent.execute_tool` -> `ToolCollection.execute` (single choke point for every tool call) | the enforcement chain: envelope -> authorize -> deny-before-execute |
| ReAct `think`/`act` loop capped by `max_steps` / `max_observe` | budgets + admission control (step caps, in-flight caps, rate budgets) |
| `PlanningFlow` `PlanStepStatus` (not_started / in_progress / completed / blocked) | the claim/execute/commit state machine (exactly-once) |
| `ToolCollection` whitelist — only tools present in `tool_map` run | principal scopes: only declared action/resource is authorizable |

The honest contrast: OpenManus executes whatever tool the LLM selects
from the collection — there is no authority model, no signature, no
budget; the step-status enum is advisory bookkeeping, not a durable
ledger. CAUTEL already proves this exact shape in
`integration/openai_agents_adapter.py` (18/18), so a governed OpenManus
adapter is a natural future adoption candidate, but it is a distinct
SDK surface (`Tool` base class, `ToolCollection.execute`, planning
flow) — not a trivially identical duplicate. Owner-gated.

## 6. suna — sandboxed generalist agent (ADOPTED)

Suna (kortix-ai) is a self-hosted generalist AI agent that acts on the
user's behalf: FastAPI backend, Next.js frontend, per-project Daytona
sandbox, Supabase. The mapping:

| suna concern | CAUTEL equivalent |
|---|---|
| per-project isolated sandbox (browser + code interpreter + filesystem, Daytona) | task-scoped `sess-*` credentials + sandbox-runner (fail closed) |
| encrypted credential vault (Fernet + SHA-256 integrity check) | per-principal key rings; secrets outside git, chmod 600 |
| credential profiles mapped to MCP qualified names | principal scopes: declared action/resource, least privilege |
| triggers (scheduled/webhook) -> background agent run in sandbox | exactly-once claim/execute/commit + admission control |
| billing/model gate before a workflow runs | deny-before-execute budget gate |

The honest contrast: suna isolates the *execution environment*; CAUTEL
governs *authority* — who may do what — before a sandbox even starts.
Suna's credential encryption protects secrets at rest but has no
signed-envelope identity, no audit chain, no revocation/expiry sweep. A
future integration would place CAUTEL in front of the trigger executor
(trigger -> envelope -> authorize -> admit -> sandbox), not replace it.
Owner-gated.

## 7. ATLAS — risk-bounded trading execution (ADOPTED)

ATLAS is a crypto strategy backtest/paper-trading platform whose
centerpiece is a pre-trade risk filter chain. The mapping:

| ATLAS concern | CAUTEL equivalent |
|---|---|
| pre-trade filter chain: signal -> regime gate -> cost hurdle -> meta-labeler -> position sizer (each gate denies before execution) | the enforcement chain: deny-before-execute |
| hard guardrails (max daily loss %, max drawdown %, max position %, max concurrent positions, per-trade stop/take) | budgets + scopes (deterministic caps) |
| kill switch — manually reset only, cooldown after | revocation + restricted mode (permanent until operator clears) |
| PBO / sample-size gates block overfit, statistically unvalidated strategies | least-privilege admission: no unvalidated principal executes |
| per-gate decision log for forensics | the audit chain (tamper-evident, every decision receipted) |

The honest contrast: ATLAS's gates are *numeric risk thresholds* for
trading; CAUTEL's chain is *authority plus budgets* — it answers who
may act, not whether a position is profitable. ATLAS's "audit" is an
append-only reason log, not a tamper-evident signed chain, and it has
no identity/authorization model. Written grant rights from the original
author make ADOPT possible later (expressing the filter chain as
budget/scope rules for a trading principal); it is not required now.
Owner-gated.

## 8. mindswarmai ("Revealing Mind") — agent-orchestration UI scaffold (ADOPTED)

mindswarmai is a Lovable/Vite React + Supabase front-end for an
autonomous-agent platform ("Revealing Mind": parallel Mind Swarm,
orchestrate/deploy/monitor agents). The mapping:

| mindswarmai concern | CAUTEL equivalent |
|---|---|
| subscription-tier entitlements (maxAgents, maxRunsPerMonth, hasSwarmAccess, ledger visibility/export) | principal scopes + budgets (deterministic caps) |
| admin-actions edge function: `has_role('admin')` RPC + service-role key + per-user rate limit | operator-gated authoritative actions + admission rate check |
| autonomy receipts (task, actions, evidence links, `verified`) | the receipt / audit chain (command_id, state digest, allow token) |
| `verified: false`, "sample design-only — never presented as real data" | CAUTEL's determinism: no fabricated execution record |

The honest contrast: mindswarmai's entitlements are *billing-tier
limits in UI code*; CAUTEL's scopes and budgets are cryptographically
enforced at the execution layer. The autonomy receipt is a design mock
explicitly labeled as not-real data, where CAUTEL receipts are signed
and tamper-evident. Most of the repo is a front-end/schema scaffold
with no execution plane to integrate. Written grant rights make ADOPT
possible later if a real execution backend appears; nothing is required
now. Owner-gated.
## Owner-gated (no action without Andrew's approval)

- Render/Memory-DB provisioning for CadenceOps M1
- Any deployment of the governed adapters against live credentials
- Publishing any code from the private repositories
- Activating 2-of-3 operator quorum on the live plane
- (2026-11-21: the four MAPPED repos — OpenManus, suna, ATLAS,
  mindswarmai — were approved by the owner and ADOPTED: adapters +
  adversarial batteries + sweep registration, sealed; statuses above)
