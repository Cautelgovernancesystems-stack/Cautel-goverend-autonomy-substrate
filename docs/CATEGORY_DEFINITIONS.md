# Category definitions

CAUTEL governs a small set of first-class objects. Every rule in
[`governance/`](../governance/) and invariant in [`specs/`](../specs/) refers
only to these categories.

## Agents

The autonomous or semi-autonomous principals that form intents (e.g., an LLM
agent, a scheduled job, a user script). Agents never act directly; they emit
**intents**.

## Intents

Structured requests describing an action, its arguments, its declared
purpose, and the delegation path under which it claims authority. An intent
is data — no execution happens at intent time.

## Executors

The runtime components that would carry out an approved intent (tools, model
calls, file operations, network operations). Executors only ever run
**authorised executions**, never intents.

## Authority objects

Credentials, roles, keys, and delegations that form the authority substrate.
See [`specs/AUTHORITY_OBJECT.md`](../specs/AUTHORITY_OBJECT.md).

## Planes

| Plane | Responsibility |
|---|---|
| **Constitutional plane** | Owns the rules, the delegation graph, and the enforcement decision. |
| **Execution plane** | Owns the sandbox and everything the agent is allowed to touch. |
| **Evidence plane** | Owns the append-only, tamper-evident record of everything. |

Planes communicate only through sealed, versioned messages. A plane that
cannot verify the others stops — the whole system is fail-closed by default.
