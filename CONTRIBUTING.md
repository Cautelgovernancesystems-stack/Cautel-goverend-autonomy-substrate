# Contributing

CAUTEL's public repository accepts contributions to its **conceptual layer**:

- `docs/` — explanations, glossary, whitepaper sections
- `architecture/` — diagrams and lifecycles
- `specs/` — formal object definitions and invariants
- `governance/` — rule text and semantics
- `validation/` — methodology and summary formats
- `roadmap/` — direction proposals

## How to contribute

1. Open an issue describing the change (or pick a `good first issue`).
2. Fork, branch, and edit only the folders above.
3. Keep every page conceptual — no code, no configs, no real logs.
4. Open a PR. Link the issue. Keep diffs small.

## Style

- American English, sentence case headings.
- Mermaid for all diagrams (see `architecture/`).
- Specifications use MUST / MUST NOT / SHOULD (RFC 2119).
- If a page needs information that isn't public, mark it `[RUNTIME — NOT PUBLIC]`
  rather than approximating implementation detail.

## Licence of contributions

By contributing you agree your contribution is licensed under the repository
LICENSE (CC-BY-4.0 for documentation).
