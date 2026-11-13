# MBI v0.9.2 — Document QA / Gate -1 Decision Record

**Status:** v0.9.2 operating-strategy documents updated. Static package checks remain separate from source/sample validation. Gate -1 remains `BLOCKED` pending actual public batch/document/macro evidence and unresolved logical-model closure. No application validation is claimed.

## Accepted user decisions

1. Data/API budget is Rp0. No paid provider, subscription, upgrade or licensed feed is a dependency.
2. Broad-market acquisition is batch-first: prefer one official user-triggered file/API batch containing many issuers.
3. After a valid batch enters local storage, processing should be automated: archive, parse, validate, normalize, calculate, update dossiers/coverage and queue discovery.
4. User must not enter each ticker/market row manually.
5. Intraday intelligence is limited to selected issuers/events, not whole-market scanning.
6. TradingView Basic export is unavailable to the user and is not a dependency.
7. BIONS and personal account exports are excluded. Portfolio is manual-first, local-only supporting context.
8. Gameplay A in the package is assistant-created and illustrative only; it is not the user's strategy and is not a Gate -1 blocker.

## Architecture changes

- Added ADR-004 for zero-budget batch-first acquisition.
- Distinguished acquisition batch, raw artifact, ingestion run and source/universe coverage.
- Replaced TradingView/BIONS sample requirements with public batch/document/macro samples.
- Defined IDX Ringkasan Saham as the primary broad-EOD candidate, not yet verified until actual downloaded file bytes/rows/fields/filters/history are inspected.
- Restricted intraday to selected issuer/event research.
- Added user action boundary: no per-ticker manual input; at most one official broad-market batch download may be requested if a permitted route cannot be otherwise sampled.
- Clarified that if historic EOD batch archives are unavailable at Rp0, MBI accumulates history prospectively and backtests wait for sufficient history.

## Source evidence reviewed for this document revision

- IDX Terms of Use: https://idx.co.id/id/syarat-penggunaan/ (published terms prohibit web scraping/crawling and require attribution/access date for non-commercial citation/use as stated there).
- IDX Ringkasan Saham: https://www.idx.co.id/id/data-pasar/ringkasan-perdagangan/ringkasan-saham/ (page displays an `Unduh` control and multiple daily fields; actual downloaded file format, row coverage, filters and history remain unverified).
- BPS WebAPI: https://webapi.bps.go.id/developer/ (documented API requires approved registration/credentials, rate limits and terms compliance; terms restrict scraping/harvesting/polling patterns that burden service).
- BPS service information: https://ppid.bps.go.id/app/konten/0000/Layanan-BPS.html (describes free API registration and separates paid statistical products).

## Remaining Gate -1 blockers

1. Actual official IDX Ringkasan Saham batch file has not yet been inspected: format, columns, units, row count, unique securities, filter behavior, selected date and history coverage are unknown.
2. One official issuer disclosure/PUBEX and one financial-statement sample still need route/format/provenance feasibility evidence, or explicit deferral.
3. One official macro source/sample route (BI publication and/or BPS WebAPI compliant batch) needs exact fields, periods, publication/vintage and terms evidence.
4. Logical-model corrections from the prior Codex audit still need approval: canonical Setup Occurrence identity vs scan membership; ordered mutually exclusive scan states/reasons; optional setup-version link for discretionary playbooks; canonical `universe_coverage_snapshot` naming.
5. Broad universe/historic coverage policy must be based on actual source rows and history available, not advertised page totals.

## Explicitly not blockers

- TradingView export: not required; no plan upgrade.
- BIONS import or personal portfolio files: not required; manual-first portfolio only.
- Whole-market intraday: not in current scope.
- Gameplay A definitions: not required; illustrative example only.
- Paid provider or licensed IDX feed: excluded by current Rp0 decision.

## Not performed

- No application code, SQL schema, migration, parser, scanner or backtest was created.
- No BIONS/personal portfolio data was requested or inspected.
- No IDX/BPS crawler or automated harvesting was run.
- No actual IDX `Unduh` file was downloaded/validated in this revision.
- No current external provider entitlement or API credential was tested.

Static document QA does not establish source permissions, data completeness, parser accuracy, market calculations or statistical validity.

## v0.9.2 static consistency checks

- Operating decision: Rp0 / batch-first broad market / limited selected intraday.
- Active Codex prompt: `CODEX_START_HERE_v0.9.2.md`.
- TradingView export and BIONS statements are not required samples.
- Gameplay A is explicitly assistant-created illustrative content, not a Gate -1 blocker.
- Static checks were run; see `QA_STATIC_RESULTS.json`. No source/sample or runtime test is claimed.

## Final v0.9.2 prompt/sample hygiene

- Superseded v0.9 Codex/Claude prompts moved to `archive/superseded_prompts/`; v0.9.2 prompts are the only active root instructions.
- Doc 03 explicitly states batch-first EOD and no per-ticker data entry.
- Doc 14 distinguishes source-derived batch rows from advertised page universe counts.
- See `QA_STATIC_RESULTS.json` for the final static check results.
