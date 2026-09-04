# Governance model

## Roles

| Role | Authority |
|---|---|
| **Root authority** | The human owner. Issues and revokes all top-level delegations. |
| **Governors** | Authorised humans (or quorums) who vote on constitutional amendments. |
| **Operators** | Run planes, respond to health alerts, review audit summaries. |
| **Agents** | Form intents under their delegated capabilities. Nothing more. |
| **Executors** | Perform authorised executions only. Nothing more. |

## Separation of powers

- Governors cannot execute. Operators cannot amend. Agents cannot delegate.
- The enforcement engine has no discretion — it applies the constitution
  mechanically.

## Change control

1. Amendment proposed (signed).
2. Quorum of governors votes; the vote is recorded in the evidence DAG.
3. On approval, the amendment becomes a new constitution version.
4. The enforced version is part of every subsequent decision record.

## Federated governance (future)

See [`roadmap/`](../roadmap/) for the federation model: multiple roots,
cross-plane delegation, and inter-organisation evidence exchange.
