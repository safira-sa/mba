# Changelog

## v0.9.1 — Audit prompt and archive cleanup (13 November 2026)
- Corrected the Codex audit prompt to point only to `CODEX_START_HERE_v0.9.md`.
- Removed superseded v0.8 agent prompts from the active v0.9 package root; the standalone v0.8 archive remains historical.
- Marked the embedded v0.7 supersession map as archive-only to prevent stale instructions from being treated as active.
- Regenerated the SHA-256 manifest and package archive.
- No product scope, logical model, application code or SQL migration changed.

## v0.9 — Setup Instance & Cross-Issuer Parameter Learning (13 November 2026)
- Added Setup Family, immutable Setup Definition Version, Setup Scan, Setup Instance, Entry Scenario, Parameter Experiment/Variant and Setup Outcome as distinct logical concepts.
- Required scans across eligible covered security/time observations, including untraded and negative cases, with reproducible denominators.
- Added point-in-time technical, fundamental, issuer-character, market/cluster and dated ownership/UBO feature context for each occurrence when available.
- Added separate MA10/MA20 distance features and percentage vs ATR-normalized measures.
- Separated hypothetical entry scenarios from actual executions and portfolio records.
- Added cross-issuer cohort/dependence controls, parameter sensitivity, holdout/walk-forward and robust-plateau requirements.
- Added setup-occurrence grouping so adjacent repeated triggers are not automatically counted as independent opportunities.
- Separated required trigger-feature availability from optional fundamental/ownership/flow context coverage.
- Clarified signal-time vs entry-time MA references, especially for next-session entry scenarios.
- Updated personal learning, logical data model, QA matrix, roadmap and agent audit instructions.
- No application code, SQL migration, provider integration or model training created.

## v0.8 — Open-World Opportunity Discovery
See `V0.7_SUPERSESSION_MAP.md` and the archived v0.8 package.

## v0.9.2 — Zero-Budget Batch-First Acquisition (13 November 2026)
- Recorded user's approved operating strategy: Rp0 data/API budget, batch-first broad-market acquisition, automated local processing after intake, limited selected-issuer intraday.
- Added ADR-004 and Gate -1 decision record.
- Replaced TradingView export/BIONS sample dependencies with one official IDX cross-sectional batch candidate, selected issuer documents and macro samples.
- Marked TradingView Basic export as unavailable/not required; BIONS and personal portfolio exports excluded.
- Marked Gameplay A as assistant-created illustration only, not a user strategy or Gate -1 blocker.
- Added `acquisition_batch` to the logical model, distinct from raw artifacts and ingestion runs.
- Updated Codex/Claude instructions, source register, sample checklist, roadmap, QA and privacy boundaries.
- No application code, physical schema, SQL migration, parser, scanner or backtest created.
