# Publish checklist — read before every public push

CAUTEL's public face is **conceptual only**. Before any commit goes public,
verify:

- [ ] No `IG_API_KEY`, `IG_USERNAME`, `IG_PASSWORD`, `IG_DEMO_*` or other
      credentials anywhere in the tree (`grep -rE "IG_(API_KEY|PASSWORD)" .`)
- [ ] No `.env`, keystores, seals, or attestation blobs
- [ ] No raw audit ledgers, telemetry, or execution traces — summaries only
- [ ] No legal/patent documents (`.ops/legal/**` and equivalents) — these are
      confidential and publication can affect IP rights
- [ ] No implementation code or configuration files — diagrams and specs only
- [ ] No personal data, account identifiers, or hostnames
- [ ] Validation pages contain methodology + result summaries, not raw logs
- [ ] `git log` history itself is clean (if re-publishing an existing repo,
      scrub history: `git filter-repo`)

When in doubt: describe the *what*, never the *how*.
