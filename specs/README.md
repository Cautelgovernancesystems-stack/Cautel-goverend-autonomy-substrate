# Formal specifications

These pages define **what** CAUTEL's objects are and the invariants they
satisfy — never **how** they are implemented.

| Page | Object |
|---|---|
| [AUTHORITY_OBJECT.md](AUTHORITY_OBJECT.md) | Credentials, roles, delegations |
| [DELEGATION_GRAPH.md](DELEGATION_GRAPH.md) | How authority flows |
| [EVIDENCE_DAG.md](EVIDENCE_DAG.md) | The append-only evidence structure |
| [TRAJECTORY_CHAIN.md](TRAJECTORY_CHAIN.md) | Ordered agent action history |
| [GOVERNANCE_INVARIANTS.md](GOVERNANCE_INVARIANTS.md) | Properties that MUST always hold |

Conventions: RFC 2119 keywords (MUST / MUST NOT / SHOULD). Each object is
defined by its fields, its lifecycle states, and its invariants.
