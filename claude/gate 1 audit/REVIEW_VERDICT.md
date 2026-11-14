# MBI Gate -1 Proposal Review Verdict

**Reviewer:** Codex, independent reviewer  
**Date:** 14 November 2026  
**Scope:** Read-only review of the five proposal documents in this directory.  
**Verdict:** **REVISE**

The package is directionally aligned with the approved Rp0, batch-first, local-first strategy and correctly preserves the 945-row sample as `OBSERVED_ONLY`. It is not ready for owner approval as written because two design decisions are materially unsafe:

1. The proposed scan-membership identity is not unique when one scan evaluates more than one setup definition. `scan run + security + evaluation timestamp` can represent only one state, while the canonical model permits a scan to carry a setup/parameter version and the same security/time to be evaluated against multiple definitions.
2. D-H is framed as an intended-use policy but then says sample importer development is “allowed” under that policy and that rejecting it leaves Gate B open. An internal restriction cannot be presented as permission or legal clearance under IDX terms. Gate B must remain terms-open until primary terms evidence is recorded.

Additional corrections are required to carry the sample contract’s zero/missing/no-activity semantics into the logical model and to keep provisional history thresholds explicitly provisional rather than treating them as validated statistical requirements.

The proposals do correctly preserve these requirements: no scraping/crawling or automated browser acquisition; selected-issuer intraday only; no TradingView export, BIONS or personal portfolio dependency; local-only privacy boundaries; Gameplay A as an assistant illustration; no historical backfill now; and no whole-market claim from 945 rows.

No code, schema, migration, parser, scanner, backtest, data fetch, or external endpoint was used or proposed by this review. Only the three review artifacts were created.

