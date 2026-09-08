# Operator Key Vault

The crown jewels never sleep in plaintext. The secrets ring and the
operator signing key are wrapped (Fernet) under a master key that itself
never exists on disk: the master is XOR-split into n shares, each share
wrapped under an individual operator credential (scrypt + per-share
verifier). Unlocking requires `threshold` shares — **dual control by
default: no single operator alone releases the startup keys.**

## The receipt history — who released the keys, when

Every attempt is hash-chained in the tamper chain, **including failures**:

- `vault_sealed` — when the vault was sealed, with share count, threshold,
  and payload keys.
- `vault_released` — which share indices unlocked it, and when. This is
  the answer to *"who started the plane, at what time."*
- `vault_attempt` — every failed attempt, recorded **without** the
  credential that was tried. Brute force leaves evidence, not access.
- `vault_tampered` — a tampered store or a swapped verifier, refused.

Five failed attempts lock the vault for five minutes (`vault_cooldown`),
and the lockout itself is recorded.

```bash
# seal (2-of-2)
python3 - <<'EOF'
import sys; sys.path.insert(0, ".")
from vault import vault as V
V.seal_vault(["operator-one", "operator-two"],
             {"secrets_ring": <ring>, "operator_key": <key>},
             threshold=2)
EOF

# unlock (both operators present)
python3 - <<'EOF'
import sys; sys.path.insert(0, ".")
from vault import vault as V
payloads, why = V.unlock_vault({0: "operator-one", 1: "operator-two"})
print(why, sorted(payloads) if payloads else "")
EOF

# query the release history from the chain
tail -c 1M logs/cautel_audit.chain | grep '"node": "vault"'
```

## Boot gate

```bash
export CAUTEL_REQUIRE_VAULT=1   # the plane refuses to boot while locked
```

The gate sits **ahead** of the layered seals: no key release, no startup —
and both events are on the same chain.

## Hardening

- the store is hash-sealed: tamper or a replaced verifier fails the seal
  and restricts the plane (`vault:tampered`);
- credentials are never stored, never recorded, and used only to derive
  keys;
- attempts throttle via KDF cost + the cooldown window.

## Roadmap

Hardware-factor shares (an unexportable device key that signs a fresh
challenge per unlock) are the planned v2 — passphrase + physical key,
with neither half sufficient alone.

Battery: `python3 vault/test/vault.py` — 13 checks (threshold, tamper,
verifier-swap, cooldown, boot gate, recovery).
