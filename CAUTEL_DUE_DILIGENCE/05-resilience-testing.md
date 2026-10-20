# 05 — Resilience Testing

CAUTEL is designed to be **survivable**, not just smart. The resilience
batteries attack the system's relationship with failure itself.

## Power loss / process death

`kill -9` drill (**13/13**): the plane is killed at every lifecycle step —
mid-envelope, mid-claim, mid-commit, mid-seal — and must come back with
committed state intact, no re-execution, no lost receipts (I8). The
ouroboros battery (**7/7**) re-verifies the entire plane from a cold
sweep: the sweep verifies the seal, and the seal verifies the sweep.

Recovery flow, verified end to end:

```
Power Off → Restart → seal verified at service start →
recover state → recover authority graph → recover evidence chain →
continue safely
```

Unattended recovery works two ways: a TPM-sealed operational unlock
(machine as the device factor — tampered boot fails closed) and an
on-disk ops-key fallback (**ops unlock 9/9**). Honest note: on the
current reference laptop the Intel PTT exposes a sha1-only PCR bank, so
the policy-unseal path is FIRMWARE-LIMITED and the ops-key fallback is
the working unattended path — stated, not hidden.

## Network partition

Wire-level partition drill (**12/12**): the cluster is cut, the old
leader isolated, ghost spends injected, one-way links forced. Outcomes:
isolated leaders fail closed, no split state, ghost spends never commit,
conservation holds after every partition, and healed nodes converge to
one digest. Classic Raft is crash-fault-tolerant, not Byzantine — a
colluding majority can censor (availability), it cannot fabricate
authority (safety). Both halves of that claim are tested.

## Replay and reconstruction

Torn-tail battery (**9/9**): a truncated or torn evidence-chain tail is
quarantined verbatim and repaired under an exclusive lock — the chain
never silently continues past damage. Point-in-time queries (**15/15**)
reconstruct past legal state. Chain auth (**12/12**) proves every block
is HMAC-authenticated over its content.

## Hardware-adjacent honesty

The current reference machine's RTC drifts (aging CMOS battery), which
timesyncd corrects with visible wall-clock steps; the clock-sanity gate
(V34/I11) correctly trips `clock_anomaly` → restricted when that
happens. That is the system working as designed — and it is on the
roadmap to fix the hardware. A dying battery can take down availability;
it cannot corrupt authority or evidence.

## The guard fleet

Three independent daemons watch the plane: a fast ring (5 s, heartbeat +
inotify), a full ring (60 s, full verification), and an airgap ring that
pins the evidence chain to offline media. Each verifies the sealed
manifest before it starts. Restricted mode engages on drift, tamper,
clock anomaly, or peer staleness — and lifts only through the governed,
audited clear.
