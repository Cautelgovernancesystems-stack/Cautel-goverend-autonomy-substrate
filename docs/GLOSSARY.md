# Glossary

| Term | Definition |
|---|---|
| **Authority object** | Any signed credential, role, key or delegation used to justify an action. |
| **Authorised execution** | An intent that passed all constitutional checks and is handed to the execution plane. |
| **Constitutional engine** | The enforcement component that checks every intent against the rules. |
| **Delegation graph** | The directed, signed structure by which the human root grants capabilities to agents and executors. |
| **Evidence DAG** | The append-only directed acyclic graph of decisions, executions and checkpoints. |
| **Fail-closed** | Default behaviour on any violation or unverifiable state: refuse execution. |
| **Intent** | A structured request for action, including claimed authority. |
| **Invariant** | A property that MUST hold before, during and after every execution. |
| **Plane** | A subsystem with its own authority, storage and failure domain. |
| **Replay** | Re-deriving the full state from the evidence chain to prove consistency. |
| **Seal** | A cryptographic commitment binding a record to its predecessors. |
| **Tamper chain** | The hash-linked structure that makes silent modification of evidence detectable. |
| **Trajectory chain** | The ordered, chained record of an agent's accepted actions over time. |

- **Lineage** — a self-hashing record of where a fact came from and what
  it descends from; ancestry makes "two views of the same bytes" detectable.
- **Commitment** — a salted hash standing in for a sensitive value; the
  receipt proves the value without holding it.
- **Vault** — the sealed store that keeps the operator keys wrapped under
  m-of-n dual control until released.
- **Layered seals** — per-layer integrity hashes verified before boot;
  trajectory and guardian verify first, and tamper refuses startup.
- **Reason codes** — the bounded, documented fault vocabulary exposed on
  health surfaces (never raw internals).
