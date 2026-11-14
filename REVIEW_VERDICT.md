# MBI Gate -1 Proposal Review 2 Verdict

**Reviewer:** Codex, independent reviewer  
**Date:** 14 November 2026  
**Verdict:** **REVISE**

Revision 2 resolves the original identity collision, makes vintage provenance substantially explicit, rewrites D-H as an internal policy, and labels 120/250/30/60 as provisional. It is still not ready for approval because:

1. Shared scan membership and per-setup evaluation counts are not defined precisely enough to guarantee independent denominators and coverage statistics when one scan evaluates several setup definitions.
2. `OBSERVED_ZERO_CONFIRMED` treats cross-field inference as confirmed semantic meaning, although the contract says no official field dictionary has been seen and all units remain unconfirmed.
3. Gate B remains open, but the “owner risk acceptance” exception could still authorize real IDX processing before terms permissions are verified. Local handling and owner risk acceptance cannot substitute for source permission.
4. “Rule version” is not explicitly declared equivalent to `setup_definition_version_id`, leaving occurrence/evaluation identity terminology open to divergent implementations.

The five prior blockers are not all in the same status: the original scan-collision blocker is resolved at the record-identity level; the vintage and threshold blockers are resolved at proposal level but retain implementation-gating open decisions; D-H’s permission wording is resolved, while the Gate B bypass exception is a new issue; zero/missing/no-trade representation is improved but not semantically sound because of the “confirmed zero” label.

The package continues to preserve Rp0, batch-first acquisition, no scraping or browser automation, selected-issuer intraday only, no TradingView/BIONS/personal-data dependency, local privacy boundaries, Gameplay A as illustrative, no backfill, and no whole-market claim from 945 rows.

No proposal or canonical document was modified. No code, schema, migration, parser, scanner, backtest, external endpoint, or data fetch was used.

