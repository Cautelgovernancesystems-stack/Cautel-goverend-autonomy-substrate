# Operator Key Vault — who released the keys, and with what hardware

The crown jewels (secrets ring, operator signing key) never sleep in
plaintext. Payloads are wrapped (Fernet) under a master key that itself
never exists on disk: the master is XOR-split into n shares, each share
wrapped under an individual operator credential (scrypt KDF + per-share
verifier). The split is **n-of-n** — every credential is required; dual
control by construction, not by configuration.

## v2: the second factor lives in hardware

The master is additionally XOR-folded with a **device factor** recoverable
only through an unexportable device key (an ECCP256 keypair generated
on-device, e.g. a YubiKey PIV slot). Credentials without the device, or
the device without the credentials, unlock nothing.

Protocol, replay-proof by construction:

- **Seal** — an ephemeral keypair performs ECDH against the enrolled
  device public key; the shared secret (HKDF, salted with the vault id)
  wraps the device factor. The device private key never leaves the
  hardware.
- **Enrollment record** — the device signs the enrollment blob (ephemeral
  pub + vault id + device pub + serial). The stored device block is
  device-authenticated: an attacker who rewrites the store cannot
  substitute an ephemeral key and extract the factor without forging a
  signature under the device key.
- **Unlock** — the vault issues a fresh 32-byte challenge; the device
  signs it (ECDSA/SHA-256, domain-separated); the vault verifies against
  the enrolled public key, then the device performs the ECDH unwrap and
  the master is reconstructed. A recorded unlock can never be replayed:
  the vault never reuses a challenge.

## Refusal vocabulary (bounded, recorded, never the credential)

Every attempt — success and failure — is hash-chained in the tamper
chain, without ever recording a credential, signature, or response:

- `vault_sealed` / `vault_released` — who sealed the keys, which shares
  and which device released them, and when. This is the answer to *"who
  started the plane, at what time."*
- `vault_attempt` / `vault_device_refused` — every failure, with a
  bounded reason code (serial mismatch, key mismatch, signature rejected,
  enrollment rejected, factor rejected). Brute force leaves evidence, not
  access.
- `vault_tampered` — a tampered store, a swapped verifier, or an unknown
  schema, refused.
- Five failed attempts lock the vault for five minutes (`vault_cooldown`),
  recorded on the chain. Device refusals count exactly like wrong
  credentials.

## Hardening summary

- Store hash-sealed; tamper or a replaced verifier fails closed.
- Device swap refused: serial, public key, and the device-signed
  enrollment record must all match.
- Identity confusion refused: the factor wrap is salted with the vault
  id, so a device block spliced from another vault (even one validly
  signed by the same device) cannot unwrap here.
- Downgrade fails: a v2 store rewritten as v1 yields garbage, not
  payloads.
- Simulated devices can never masquerade as hardware: simulated sealing
  requires an explicit flag and every receipt records the kind.
- No device present → probe fails closed; nothing is written.
- With the boot gate on, a locked vault refuses plane startup — the gate
  sits ahead of the layered runtime seals.

## Validation

- 49/49 automated checks (13 core vault, 36 hardware-factor adversarial:
  replay, serial clone, key swap, enrollment forgery, store downgrade,
  cross-vault splice, brute-force cooldown, chain hygiene).
- Included in the full-plane sweep (22 suites, 480 checks, green).
- External black-box round: 13 claims against a frozen build — 12 held
  fail-closed, 1 real finding fixed before close, re-run clean.
