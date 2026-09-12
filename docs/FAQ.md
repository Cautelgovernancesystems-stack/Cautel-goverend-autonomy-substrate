# Hard questions, direct answers

People evaluating CAUTEL ask sharp questions — about the threat model,
recovery, rollback, external integrations, and what has *actually* been
validated. The answers are here, up front, with the boundaries stated
plainly. No answer in this document is an aspiration: each one reflects
the current build and its test receipts.

---

## 1. What exactly does CAUTEL protect against?

**Application-level attackers — including code running inside the plane's
own sandboxes — are in scope. Root and same-UID attackers are not.**

- **In scope:** an unprivileged local attacker who can read and modify
  plane files; a compromised agent or tool executing inside the bwrap
  sandboxes; other-UID processes with loopback network reach. Every
  enforcement surface is keyed (operator HMACs on the tamper chain, on
  every raft RPC, on endpoint requests, and on node-state responses), so
  keyless forgery trips fail-closed. The external round's attacker
  profiles (compromised agent, stolen credentials, malicious tool, local
  user, network attacker) all sit inside this boundary.
- **Out of scope, by explicit documentation:** a compromised host kernel
  or root (isolation is seccomp + rlimits + namespaces — no LSM/VM
  boundary); secrets on disk while the plane runs (0600, readable by
  root); and — precisely because the operator key is owner-readable — a
  **same-UID** process. The keyed-integrity walls hold against keyless
  and cross-UID attackers; anyone who can read the key can forge the
  MACs. Cryptography protects the representation of authority; it does
  not create it.

## 2. Can CAUTEL recover unattended after a restart?

**No — and that is deliberate.**

Unlocking the operator key vault requires the n-of-n credential shares
**and** the hardware-bound device factor, supplied interactively by an
operator. The boot gate refuses to serve while the vault is locked
(proven by batteries H27/H28 and V10–V12: the refusal, and the clean boot
after unlock). No systemd unit auto-unlocks; no recovery credential set
exists on disk.

The trade-off is resilience-for-control: the plane starts cold-deny, and
an operator re-unlocks after any restart. High-availability workloads
that need unattended restart would require a new recovery-credential
design (for example, recovery shares sealed to a TPM) — designed, but not
built, and not claimed.

## 3. What stops rollback of business state (restore an older snapshot)?

**Rollback is made detectable and fail-closed by four independent layers —
it is not prevented by magic.**

1. **Append-only ledger, seal-pinned.** The operator seal pins
   `(chain_head, chain_blocks)` and the prefix is verified — a restored
   or truncated chain fails the pin (`seal_head_not_in_window`,
   `ledger_truncated`). Every spend and every revocation is a receipt on
   that chain.
2. **The airgap anchor.** `check_airgap.py` verifies the chain prefix
   against offline media — the one check no on-host self-verification can
   pass. An attacker restoring old files on the host cannot update the
   offline snapshot without physical custody.
3. **The decision log** is prefix-checked (append-only, byte-exact).
4. **The authority/budget state is replicated** (raft) with a keyed state
   seal and guardian G6 digest agreement — a restored node diverges from
   its peers and degrades the cone; the plane fails closed.

Boundary, stated plainly: an attacker who can replace the seal, the
airgap media, *and* every raft node's state together controls the
anchors of trust themselves and is outside the model. Physical custody
of the airgap media is part of the boundary.

## 4. How far do exactly-once guarantees extend into external systems?

**Internal: strict. External: at-most-once plus a stable idempotency key.
There is no distributed transaction.**

- **Inside the plane:** claim → atomic EXECUTING mark → commit. The same
  `command_id` can never execute twice (11/11 race battery; 9/9
  replicated CAS over raft). If the connection dies mid-execution, the
  command sits EXECUTING and any retry is refused (`execution_in_flight`)
  — fail-closed, no double-send.
- **Idempotent / cooperative external system:** pass CAUTEL's
  `command_id` as the external idempotency key. CAUTEL guarantees it will
  not re-send the same command identity on its own.
- **Non-idempotent external system:** no guarantee — CAUTEL cannot
  observe whether the external action happened. It refuses to silently
  re-send; an operator can deliberately release the execution for a
  manual retry. Exactly-once cannot be extended past a non-cooperative
  service without a transactional protocol, and CAUTEL does not implement
  one.

## 5. Where do evidence lineages come from, and how is independence enforced?

- A claim (including its declared sample and counterexamples) is
  submitted by the claiming party — an agent **can** submit those fields.
- **Lineage records cannot be forged by the claimant:** every lineage
  record carries an operator-keyed provenance, and `verify_lineage`
  refuses any record without a valid one (self-hashing alone is
  recomputable, which is why provenance is keyed). In practice lineages
  come from tagged collection/verification roles
  (`producer="pipeline-v1"`, `"manual-verify"`, …), not from the agent's
  own process.
- **Independence is checked, not declared:** publication rule R1
  requires every confirmation's input lineage to be **row-disjoint AND
  ancestor-disjoint** from the claim's production lineage; the declared
  domain must exactly cover the production rows; universal claims with
  counterexamples inside their own sample are refused; and an incomplete
  lineage refuses rather than pretending independence. (Evaluator
  battery: 33/33, including the ancestry-depth fail-closed case.)

## 6. What did the external assessor actually see?

**The 13-claim black-box round is real, its receipts are published — and
it predates several current defenses. The separation is labeled, not
hidden.**

- The round covered authorization, budget, revocation, finalization,
  evidence, and identity surfaces, and produced the unsigned-revert
  finding that was fixed before the round closed. Its artifacts live in
  [`validation/`](../validation/) — METHODOLOGY, BREAK_FIX_GREEN,
  SAFETY_TESTS, REPLAY_CONSISTENCY.
- It did **not** cover the three-guard fleet, the hardware-factor vault,
  or the later hardening (loopback endpoint auth, per-block chain MACs,
  raft CAS, the executioner kill-chain rework, the clock-drift
  tripwire). Those are covered by internal adversarial batteries only.
  The validation documentation keeps this separation explicit: the
  external round's scope, then the "hardened build" additions.
- **Independence, stated plainly:** the validation is thorough and honest
  but written by the same engineering effort as the system. There is no
  third-party certification (no Common Criteria, SOC 2, etc.), and no
  third-party assessor report. "Externally validated" here means
  black-box methodology with published receipts — not independent audit.

## 7. What is the operational record?

Straight answers, no rounding up:

| Question | Current answer |
|---|---|
| Longest continuous run | **Hours to ~1 day** per component between restarts, on a single developer machine (restarts during the hardening arc were deliberate deploys) |
| Heaviest workload | The validation load itself: 1,000 signed commands through the live router at ~14 events/s, 0 dropped |
| Recovery time after a triggered restart | Services restart in **seconds** (systemd `Restart=always`, seal-verified start); full recovery including the interactive vault unlock is **minutes, operator-in-the-loop** |
| Legitimate refusals in normal operation | Only the designed ones: admission rate limits, sequence-gap refusals, fail-closed restricted windows. Attack drills *do* trigger the guards — that is the system working |
| Production-ready external integrations | **None.** The only integration is the local Ollama inference backend. No broker, payment, or third-party API integration exists in the codebase |
| Outside operators | **None.** No one outside the development environment has operated CAUTEL end-to-end |

**Current verification totals** (2026-09-12): 28 battery suites, 425
adversarial checks, plus the 1,000-command V18 isolation load drill and
the CLI anchors (constitution self-test, seal verify, airgap check) — all
green. The external round remains: 13 claims, 12 held fail-closed, 1
found and fixed before close, re-run clean.

## The honest summary

CAUTEL's security design, recovery behaviour, and evidence model are
genuine and heavily tested — but the tests are self-authored, the
external round predates parts of the current system, unattended recovery
and HA provisions do not exist yet, and the system has never been
operated by anyone outside its development environment. Treat it as a
rigorously self-tested substrate with a precisely documented threat
model — not as independently validated or operationally proven software.
The documentation refuses to claim otherwise, and so should anything
built on top of it.
