# Break → fix → green pass log

Format for every validation finding. Only summaries are published.

```markdown
## B-07 — constraint tamper attempt (summary)

- **Date:** YYYY-MM-DD
- **Class:** B — rule tampering
- **Attempt:** modify an enforced constraint outside the amendment procedure
- **Observed:** enforcement engine refused to load the altered state;
  degraded mode engaged; alert raised (break reproduced)
- **Fix:** {one-line conceptual description}
- **Re-test:** identical attempt after fix
- **Result:** refused and recorded; chain verified root-to-head
- **Status:** GREEN
```

## Published matrix (template — fill from your runs)

| ID | Class | Attempt | Status |
|---|---|---|---|
| A0 | authorised control | control plane executes approved intent | GREEN |
| A1 | unauthorised execution | execute without delegation | GREEN |
| B1 | rule tampering | edit constitution out-of-band | GREEN |
| B2 | constraint tampering | alter constraint file | GREEN |
| C1 | evidence tampering | rewrite ledger entry | GREEN |
| C2 | seal break | modify sealed state | GREEN |
| D1 | duplicate sequence | replay same message | GREEN |
| D2 | out-of-order sequence | reorder messages | GREEN |
| D3 | replay attack | resend old signed intent | GREEN |
| E1 | crash sim | kill a plane mid-execution | GREEN |
| E2 | time skew | shift timestamps | GREEN |

> Replace the GREEN column with your actual latest run results; never publish
> a GREEN you have not reproduced.
