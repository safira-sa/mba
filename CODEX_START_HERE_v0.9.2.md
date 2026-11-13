# Codex Start Here — MBI v0.9.2 (Gate -1 Closure Only)

## Current authorization

The user approved this operating strategy:
- Data/API budget: Rp0.
- Broad-market acquisition: batch-first, preferably one user-triggered official batch containing many issuers.
- Intraday: limited to selected issuers/events.
- Processing after accepted file/API batch intake: automated locally.
- TradingView Basic export: unavailable to the user; no upgrade and no dependency.
- BIONS/personal portfolio data: explicitly excluded; portfolio is manual-first and local-only.

We remain at **Gate -1**. Do not start Sprint 0 or write code.

## Read first

- `README.md`, `AGENTS.md`, `CLAUDE.md`, `GATE_MINUS_1_DECISION_RECORD.md`, `QA_REVIEW_REPORT.md`.
- `docs/00` through `docs/18` and `docs/decisions/ADR-001` through `ADR-004`.
- `examples/setup_parameter_learning_gameplay_A.yaml` is an assistant-created illustrative example only; it is not the user's actual gameplay and is not a Gate -1 blocker.

## Audit objectives

1. Verify the operating decision is consistently reflected in source strategy, sample checklist, data architecture, roadmap, privacy boundaries and agent instructions.
2. Research only source routes that are permitted. IDX website terms prohibit scraping/crawling; do not scrape/crawl, reverse engineer private endpoints or bulk harvest.
3. Investigate the official IDX Ringkasan Saham `Unduh` control as the primary broad-EOD batch candidate. Establish exact downloadable file format, row count, unique issuers, fields, units, filters, date semantics, historical date availability and whether the file represents the full selected universe. A visible table/download button alone is not proof.
4. Verify a minimal route/sample for one official issuer disclosure/PUBEX item and one financial statement format (PDF/XLSX; XBRL only if actually available).
5. Verify one official macro sample/route: BI downloadable publication/table and/or BPS WebAPI. BPS requires approved registration/credentials and rate-limit/terms compliance; no HTML scraping or burdening polling.
6. Define source statuses: `ROUTE_OBSERVED`, `TERMS_REVIEWED`, `SAMPLE_VERIFIED`, `CONDITIONAL`, `UNVERIFIED`, `DEFERRED`.
7. Define the source-derived broad EOD universe based on actual batch rows. Separate detailed research universe, personal watchlist and historical universe. Do not require manual ticker-by-ticker entry.
8. Determine whether legal Rp0 historical daily batches are available. If not, specify prospective accumulation and the exact limits on historical learning/backtests.
9. Treat intraday as selected-issuer/event only. If no compliant free sample exists, defer it without blocking the broad EOD architecture.
10. Confirm TradingView export and BIONS imports are not required. Do not ask for either file or any personal account data.
11. Confirm Gameplay A is illustrative only. Do not ask the user to define its timeframe, MA or entries. The generic setup/learning engine must remain open to future user-originated and data-discovered gameplay.
12. Review the logical-model clarifications from the previous audit, including canonical occurrence identity, mutually exclusive scan states and `universe_coverage_snapshot` terminology. Propose logical changes only.
13. Review zero-budget batch failure modes: incomplete export, page-only export, source terms changes, historical gaps, missing rows, duplicate revisions, unit ambiguity and stale batches.

## Sample handling

- Prefer a single official cross-sectional batch file over many per-ticker files.
- Codex may research public routes and terms. Do not scrape/crawl IDX or BPS, or automate browser harvesting.
- If a sample must be downloaded by the user, request only the smallest exact batch (ideally one date's IDX Ringkasan Saham export) and explain why. Do not ask for all sources at once.
- Do not ask for BIONS, personal portfolio, credentials, screenshots containing private data or a paid TradingView export.
- Real samples and synthetic fixtures must be clearly separated. Synthetic data never validates an external file format or source permissions.

## Required output

Return:
1. `READY_FOR_SPEC_APPROVAL` or `BLOCKED`.
2. Updated source feasibility matrix with evidence and last-verified date.
3. Batch-file field/row/issuer coverage report, or exact unresolved reason if no file can be obtained.
4. Public document and macro source feasibility/sample report.
5. Broad universe and historical-coverage policy.
6. Selected-intraday scope and limitations.
7. Remaining logical-model gaps.
8. Exact minimal user actions (if any), no generic list of files.
9. Gate -1 acceptance checklist and bounded next step.

## Hard stop

Do not create or modify application code, install packages, create SQL/physical schema/migrations, parsers, scanners, backtests or database files. Do not request personal portfolio/BIONS data. Do not promote Gameplay A to a user requirement. Stop after the Gate -1 closure report.
