# Gate -1 Operating Decision Record — MBI v0.9.2

**Status:** `BLOCKED — source/sample verification pending`  
**Decision date:** 13 November 2026  
**No coding authorization:** active.

## User-approved operating choices

| Decision | Approved value | Consequence |
|---|---|---|
| Data/API budget | Rp0 | No paid provider, plan upgrade, subscription or licensed feed dependency. |
| Broad-market acquisition | Batch-first | Prefer an official user-triggered cross-sectional file or finite permitted API batch containing many securities. No ticker-by-ticker manual data entry. |
| Processing | Automated after intake | MBI archives, parses, validates, normalizes, calculates and queues discovery after an accepted batch is imported locally. |
| Acquisition automation | User-triggered batch by default | Do not claim unattended downloads unless the exact source route and terms explicitly permit them. |
| Intraday | Limited selected issuers/events | No all-market intraday scanner; absent intraday data does not invalidate EOD discovery. |
| TradingView | Basic plan, no export | Excluded from required samples and source dependencies; no upgrade. |
| BIONS/personal portfolio | Excluded from Gate -1 | No BIONS exports, account reports or personal portfolio data requested. Portfolio is manual-first, local-only supporting context. |
| Gameplay A | Assistant-created illustration | Not the user's strategy, not required to be runnable, not a Gate -1 blocker. |

## Product interpretation

**Batch-first does not mean manual ticker entry.** One accepted daily market file should be processed across all rows/securities it actually contains. If the source does not supply a full universe, the system reports the actual coverage and does not claim more.

**Automation is layered:**
1. Acquisition: user-triggered official batch download or finite permitted API batch.
2. Intake: local batch import.
3. Processing: automatic archive, parsing, validation, normalization and canonicalization.
4. Analytics: automatic eligible EOD metrics, issuer dossiers and broad cross-sectional anomaly discovery.
5. Intraday: selected-issuer/event follow-up only, based on actual permitted capture/source availability.

## Approved candidate routes

- IDX Ringkasan Saham visible `Unduh` route is the primary broad-EOD batch candidate. The page displays daily price/activity fields; actual file format, row coverage, filter semantics, history depth and export terms must be verified from a real downloaded file.
- IDX disclosure/PUBEX/financial reports use selected user-triggered official document downloads; no crawling or mass harvesting.
- BI official statistical publications use selected official file downloads until a compliant machine-readable route is verified.
- BPS WebAPI may be used for finite documented API batches after application registration/approval, local token handling, rate-limit review, caching and attribution. No HTML scraping or burdening polling.
- Stockbit capture can supplement selected issuers only. No public full-market API is assumed.
- Third-party free APIs are optional selected-issuer research candidates only, disabled until terms/quota/coverage are checked; they are not the broad-market source of record.

## Gate -1 sample request philosophy

Codex/research should find permitted public sample routes first. The user is not expected to gather every document manually. If one file is needed to validate the broad batch, request at most one official IDX Ringkasan Saham download through the visible UI. Do not ask for TradingView CSV, BIONS export, personal portfolio data or manual ticker-by-ticker observations.

## Gate -1 unresolved items

1. Actual IDX daily batch bytes: file format, columns/units, row count, unique security IDs, market filters, trading-date semantics and history depth.
2. Official issuer-document sample route and PDF/XLSX/XBRL availability.
3. Official BI/BPS macro sample route, period/publication/vintage semantics and current terms.
4. Initial source-derived broad EOD universe and historical universe policy.
5. Whether free historical daily files can be obtained in a lawful batch. If not, prospective accumulation is the plan and historical tests wait for sample depth.
6. Logical-model corrections from prior audit: canonical setup occurrence identity separated from scan membership; one mutually exclusive scan-state model; discretionary playbooks need not link to setup versions; canonical name `universe_coverage_snapshot`.

Intraday sample acquisition is optional and may be deferred. Gameplay A parameter definitions are not requested at this gate.

## Gate -1 exit criteria

Close Gate -1 only when source terms/routes and real file evidence establish the first batch import contract, public document and macro routes are understood or explicitly deferred, source-derived universe/coverage policy is explicit, logical-model corrections are approved, and all unavailable fields/history limitations are visible. No parser/schema implementation is required to close the documentation gate; those are later sprint tasks informed by accepted samples.
