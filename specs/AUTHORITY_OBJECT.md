# AuthorityObject — specification

## Definition

An AuthorityObject is the unit of authority in CAUTEL: a signed statement
that binds an **identity** to a **capability** for a **scope** and
**validity window**.

```
AuthorityObject {
  id            : opaque unique id
  subject       : identity of the holder (agent, executor, operator)
  capability    : the action class granted
  scope         : the resources the capability applies to
  issued_by     : parent AuthorityObject (delegation chain)
  valid_from    : timestamp
  valid_until   : timestamp | null
  signature     : signature by issued_by over the above fields
}
```

## Lifecycle states

`issued → active → expired | revoked`

## Invariants

1. Every AuthorityObject MUST trace to the **human root authority** through a
   chain of valid signatures (no orphan authority).
2. An object MUST NOT grant a capability wider than the intersection of the
   capabilities of its ancestors (no privilege amplification).
3. A revoked or expired object MUST be treated as nonexistent by the
   enforcement engine.
4. Revocation MUST propagate: any object whose ancestry includes a revoked
   object is itself invalid.
