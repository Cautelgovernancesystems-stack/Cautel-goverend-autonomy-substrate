# 08 — Architecture

## The enforcement chain

```
signed envelope (identity + integrity)
  → clock sanity (I11)
  → authorisation (I4, deny before execute)
  → sequence (I3, per-source ordering)
  → idempotency (I2, exactly-once)
  → admission control (I9, rate budgets / priority reserves)
  → evidence dependency gate (I10, fail closed without telemetry)
  → state-bound ALLOW (the only path to execution)
```

Every request crosses every gate. Any gate that cannot be proven green
denies the request and receipts the denial. Restricted mode — entered on
policy drift, tamper, clock anomaly, or peer staleness — denies
everything, including requests with otherwise valid envelopes.

## Authority state

A pure state machine: authority enters only via operator-signed grants;
it moves via signed delegations (conservation-checked at the
linearisation point); it is consumed by one-shot signed spends. The same
pure function runs on every replica — deterministic law, no wall clock,
no RNG — so a log entry commits to the same decision everywhere. The
replication layer (Raft) replicates bytes; the state machine applies
law. A malicious leader can commit garbage to the log and every honest
node denies it at apply.

## Trust model

- **Roots of trust:** the human operator (operator key), the sealed
  policy (operator-signed attestation), the vault (n-of-n credentials +
  optional TPM device factor).
- **Trusted:** the enforcement engine and its attested interpreter.
- **Untrusted by default:** agents, departments, the network, the log
  replication layer, the host's other users — each is assumed
  compromised and must present signed authority anyway.
- **Out of model (stated, not hidden):** root, same-UID processes, the
  holder of all operator material, physical access, speculative
  execution (Rowhammer-class) — detection via chain hashes and the
  interpreter pin, not prevention.

## Deployment (current reference build)

- Single machine, Ubuntu, systemd user units.
- Three guard daemons (5 s fast ring, 60 s full ring, airgap ring),
  each verifying the sealed manifest before start.
- Router + gateway + enforcement engine; evidence chain on disk, pinned
  to offline media.
- Governed change control: policy edits require re-attestation and a
  new seal; commits pass a full-sweep gate (any suite regression rejects
  the commit).
- Sealed audit windows (`bin/audit_window.py`): choose a from/to
  date-time and CAUTEL writes a self-contained evidence package — chain
  slice, decisions, boundary state digests at both ends, operator-HMAC
  token over the manifest — that verifies against the live chain
  without trusting its author.

## The boundaries (same list in every artifact)

1. A colluding replication majority can censor availability; it cannot
   fabricate authority.
2. The holder of all operator credentials + the signing key IS the
   constitution (root class, out of model).
3. No third-party security audit yet (see 09).
4. No production deployment or outside operators yet — single-machine
   reference build.
5. One operator signing key (no m-of-n signature threshold on grants);
   multi-operator strength lives in the n-of-n vault.
