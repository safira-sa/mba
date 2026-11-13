# Codex Start Here — MBI v0.9 (Audit Only)

## Important: do not code yet

The user is still finalizing product requirements. Perform read-only requirements, logical-model, source and sample feasibility audit only. Do not create application files, install packages, modify schema, create migrations, call provider APIs, scrape websites, parse private exports or train models.

## Read
- `README.md`, `AGENTS.md`, `CLAUDE.md`, `QA_REVIEW_REPORT.md`
- all canonical `docs/00` through `docs/18`
- `docs/decisions/ADR-001_MARKET_EPISODE_AS_CORE.md`
- `docs/decisions/ADR-002_OPEN_WORLD_DISCOVERY.md`
- `docs/decisions/ADR-003_SETUP_INSTANCES_AND_PARAMETER_LEARNING.md`
- examples under `examples/`

## Audit tasks
1. Inspect current repository, Git status, any existing code/database/schema/tests and source samples. Do not assume files exist.
2. Map current concepts to v0.9 logical model. Report duplicates, gaps, contradictions and unnecessary complexity with exact file/line/table evidence.
3. Verify Market Episode, Discovery Candidate, Setup Family, Setup Definition Version, Setup Scan, Setup Instance, Setup Occurrence Group, Entry Scenario, Parameter Experiment, Gameplay/Playbook, Trade Idea/Decision and Execution Fill are distinct and linked correctly.
4. Audit Gameplay A: price above MA10/MA20 and two green candles. List undefined semantics (timeframe, SMA/EMA, input price, completed candle, adjusted/raw price, signal time, eligibility) without guessing.
5. Assess whether all eligible issuer/time cases can be scanned and matched, non-matched, excluded and unavailable denominators reproduced. Distinguish broad EOD coverage from partial intraday/broker/orderbook coverage.
6. Audit point-in-time technical, fundamental, issuer-character, market/cluster, sector, conglomerate and UBO features. Check publication/effective dates, restatements, units, missingness and look-ahead controls.
7. Audit entry-distance variants for MA10 and MA20 separately, percentage vs ATR-normalized distances, signal-time vs entry-time references, executable fill timing, costs/slippage, data resolution, ambiguous fills and hypothetical-vs-actual separation.
8. Review cross-issuer cohorts, repeated issuers, shared event dates, overlapping clusters/ownership networks and market shocks. Identify dependence and effective-sample-size risks. Check consecutive repeated triggers for occurrence grouping/re-arm semantics.
9. Confirm missing optional fundamental/UBO/broker/orderbook context does not erase a valid setup match; only required trigger-feature gaps make the scan row unevaluable.
10. Review parameter experiments: predeclared ranges, baseline, all variants attempted, multiple-testing control, time-based splits, overlap purging/embargo, walk-forward/holdout, counterexamples and robust plateaus.
11. Confirm market-wide setup outcomes are evaluated even when the user did not trade. Personal Edge evaluates only the user's actual/recorded decisions and compares them with the broader opportunity population.
12. Confirm user gameplay, data-discovered patterns and hybrid gameplay use the same tested pipeline without being forced into predefined setups.
13. Audit whether portfolio remains supporting context only.
14. Propose logical model changes only. Do not write SQL.

## Required output
- Executive decision: `READY_FOR_SPEC_APPROVAL` or `BLOCKED`.
- Confirmed blockers/high risks vs questions.
- Concept mapping and contradiction report.
- Setup-instance / parameter-learning feasibility matrix.
- Source and data coverage limitations by field and timeframe.
- Point-in-time feature and ownership-link risks.
- Statistical validity and sample-dependence risks.
- Logical model gaps/duplicates and minimal proposed changes.
- Exact unresolved definitions for Gameplay A; do not invent them.
- Bounded next step and explicit statement that no code/schema/provider action was performed.

## Stop
Stop after the audit report. Do not implement Sprint 0 or ask Claude to edit files. Wait for user approval.
