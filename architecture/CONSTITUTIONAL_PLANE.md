# Constitutional plane (conceptual)

The constitutional plane is the only component that decides **may this
happen**. It holds:

1. **The constitution** — the rule set (see
   [`governance/CONSTITUTION.md`](../governance/CONSTITUTION.md)).
2. **The delegation graph** — how authority flows from the human root.
3. **The enforcement engine** — the checker that evaluates every intent.

```mermaid
flowchart LR
    R[Human root authority] -->|signed delegation| D[Delegation graph]
    I[Intent] --> C{Enforcement engine}
    D --> C
    C -->|all rules hold| OK[Authorised execution]
    C -->|any rule fails / unverifiable| NO[Fail closed]
```

Key property: the enforcement engine has **no discretion**. It evaluates the
rules mechanically; anything it cannot verify is a refusal. See
[`governance/ENFORCEMENT_SEMANTICS.md`](../governance/ENFORCEMENT_SEMANTICS.md).
