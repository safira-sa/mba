# 12 — QA Matrix, Edge Cases & Release Checks

## 1. Source and ingestion
- Original file bytes/hash remain unchanged.
- Duplicate import is idempotent.
- Corrected/revised source creates a new version/supersession link.
- Import/update cadence is visible; when no import occurs, UI does not imply it has observed the market.
- Unknown/renamed critical columns require mapping.
- Unit, currency, timezone, market segment and price basis are never guessed.
- Partial/bad rows are quarantined with reason, not silently dropped.
- App remains usable when an optional source is unavailable.
- Terms/route review exists before automated collection.

## 2. Issuer dossier and stock character
- Ticker alias/security ID effective dates resolve historical identity.
- Eligible, observed and covered universes are separately represented; delisted/suspended historical members are not silently removed.
- EOD data cannot generate intraday features.
- One cross-sectional batch can contain many issuers; import processes all valid rows without per-ticker data entry.
- Actual observed rows, unique securities and filters define batch coverage; visible website counts do not prove file coverage.
- Re-import of identical batch bytes is idempotent; corrected batch files create new artifact/version lineage.
- Missing TradingView export, BIONS report or intraday sample does not block the broad EOD pipeline.
- Manual portfolio records are local-only and never required for market-learning tests.
- Lots vs shares cannot be confused.
- Turnover cannot be replaced by `close × volume` without proxy label.
- Zero-range candle makes close-location undefined.
- Suspended/no-trade sessions are not treated as ordinary zero-volume days.
- Corporate-action price adjustments are versioned and auditable.
- Relative intraday volume uses matching time buckets and completed historical sessions; only selected-issuer observations are in the Rp0 baseline.
- Missing broker/foreign/orderbook fields stay unknown.

## 3. Event and episode logic
- Event occurrence, publication, first-observed-public and retrieval times remain separate.
- Historical as-of replay excludes documents published later.
- No-news search records query/source/time and never claims exhaustive absence.
- One public event affecting multiple tickers does not create fake independent sample size.
- Multiple detectors on the same episode are not independent cases.
- Overlapping macro/corporate events remain competing explanations.
- A later disclosure does not rewrite the original anomaly record.
- Event study output is not labelled causal without an identification strategy.

## 4. Document intelligence
- PDF page references retained; scans flagged for OCR/manual review.
- XLSX sheet/cell provenance retained.
- XBRL concept, taxonomy, unit, context, dimensions, instant/duration, consolidated/parent basis and decimals retained.
- YTD vs standalone quarter handled correctly.
- Corrected filing retained; earlier version not overwritten.
- Management target/guidance remains a claim until separately observed as actual.
- OCR/LLM extraction must cite source span and require review for material facts.

## 5. Rant-to-research
- Preserve raw text exactly.
- Extraction proposals include source spans and unresolved fields.
- Extraction confidence is not truth confidence.
- Sarcasm, hypotheticals, corrections, multiple tickers and pure emotional notes do not become facts.
- "If I buy" / "maybe sell" does not create execution or ledger event.
- User acceptance of a hypothesis does not verify it.
- A rejected extraction leaves raw note intact.

## 6. Discovery and gameplay
- New unquantified gameplay can be stored without code changes.
- Unsupported operators stay non-runnable.
- User, discovered and hybrid playbooks remain distinguishable.
- Positive and negative examples included.
- Candidate pattern tested out of sample and in shadow mode.
- Pattern, detector, gameplay, playbook, signal and prediction are separate records.
- Sparse samples return descriptive-only/insufficient-data.
- Multiple tests/correlated tickers/overlapping episodes are controlled.
- Old model/playbook predictions and snapshots remain immutable.

## 7. Open-world discovery and conviction support
- Top-down public policy/macro input can map to candidate sectors/issuers outside saved watchlists only through documented transmission paths.
- Bottom-up issuer anomaly can surface without a known news match and remain unexplained.
- Candidate is not silently promoted to formal Market Episode, verified fact, gameplay or trade signal.
- Relationship/network expansion preserves relationship type, source, validity period and verification state.
- Post-IPO/lifecycle cohort records missing free-float/lock-up/ownership data as unknown; delisted/suspended eligible cases are not silently removed.
- Each discovery run records intended, eligible, scanned and excluded universe plus field-level coverage.
- Candidate brief includes support, contradiction, alternative explanations, analogues, unknowns, as-of times and next conditions.
- Research sorting does not masquerade as expected return/probability/conviction.
- User conviction is stored separately and is not overwritten by system inference.
- User acceptance/rejection does not turn an unsupported claim into a verified fact.

## 8. Setup instances and parameter learning
- Setup Family, immutable Setup Definition Version, Setup Scan, Setup Instance, Entry Scenario, Parameter Experiment, Playbook, Personal Decision and Execution Fill are distinct records.
- A scan reports eligible, trigger-evaluable, matched, non-matched, excluded and unavailable counts; denominator is reproducible. Missing optional context does not erase a trigger match.
- Matched and negative examples are retained; no winner-only sample.
- Completed-bar semantics, timeframe, MA type/period/input, price basis and warm-up are explicit.
- Two-MA conditions test each MA separately; MA10 and MA20 distances are not collapsed.
- Percentage distance and ATR-normalized distance use distinct metric IDs.
- Fundamental values are point-in-time; reporting period, publication/vintage, revision, consolidated/parent basis and units are retained.
- Ownership/conglomerate/UBO context is effective-dated, sourced and status-labelled; missing links remain unknown.
- Stock Character, regime, sector/cluster and issuer context are referenced through versioned snapshots.
- Hypothetical entry scenarios specify executable timing, fill convention, slippage/fees and resolution; ambiguous fills remain ambiguous.
- Entry scenarios never create actual fills, cash movements or portfolio positions.
- Outcome horizons are declared before evaluation; no look-ahead into feature snapshots.
- Consecutive matches for the same security/setup family can be grouped by a versioned re-arm rule; raw matches remain intact.
- Related issuer instances sharing date/event/cluster/network or the same occurrence group are not treated as independent by default.
- Signal-time MA distances and entry-time MA distances are separate; next-open scenarios cannot use that session's future close/MA.
- Parameter experiments record every attempted variant, baseline, sample, failure, time split and multiple-testing plan.
- Robust parameter plateaus are preferred over isolated in-sample optima.
- Personal Edge separates market-wide setup quality from user selection and execution.

## 9. Personal edge
- Actual trades vs no-trade/missed/rejected populations separate.
- Decision-time context only; no future disclosure/chart data leaks into snapshot.
- Fills are actual source records or user input, never inferred from bars.
- Ambiguous daily target/stop ordering flagged.
- Selection, setup, execution, risk behavior and outcome measured separately.
- Missing trade rationale in older history is not retroactively invented.

## 10. Portfolio context
- Multi-account same-ticker positions remain distinct and aggregate exactly once.
- Internal transfers are not external deposits/withdrawals.
- Strategy sleeves are virtual attribution, not new assets.
- Stale equity/cash and unknown fees block definitive sizing.
- Stop risk is not guaranteed max loss under gaps/suspensions.
- Overlapping clusters are not naively summed as disjoint exposure.
- Corporate action without validated handler flags reconciliation required.

## 11. Release gate
- Clean startup and restart.
- Migration on a copy; verified backup restore.
- SQLite integrity and foreign-key checks pass.
- Raw archive backup/restore verified.
- Unit/integration/regression tests pass.
- Diff has no secrets/private reports/unrelated changes.
- Documentation and changelog updated.
- Exact commands/results and known limitations reported.
