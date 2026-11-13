# 11 — Roadmap, Sprint Sequence & Acceptance Gates

## Gate -1 — Product/data feasibility (no code)

**Approved operating decision (ADR-004):** Rp0 data/API budget; broad-market acquisition via user-triggered official batch download or a separately approved finite API batch; automatic local processing after intake; limited selected-issuer intraday. No TradingView upgrade, BIONS export, personal portfolio data, paid feed or per-ticker market-data entry is required.

**Work:**
- Review product requirements and close logical-model corrections from the prior audit.
- Verify the official IDX Ringkasan Saham `Unduh` route by inspecting one actual user-triggered batch file if available: format, fields, units, filters, date semantics, row count, unique securities and historical date support.
- Review the current IDX terms and ensure no scraping/crawling is used. A visible download control is not enough; verify the actual bytes and coverage.
- Verify one selected official issuer document route (disclosure or PUBEX) and one public financial-report format where available.
- Verify one official macro source/route: BI publication batch and/or BPS WebAPI only after registration/terms/rate-limit conditions are understood.
- Build a source feasibility matrix with `ROUTE_OBSERVED`, `TERMS_REVIEWED`, `SAMPLE_VERIFIED`, `CONDITIONAL`, `UNVERIFIED`, `DEFERRED` states.
- Define broad EOD universe based on the rows and security identities actually present in the accepted batch; keep detailed research universe, personal watchlist and historical universe separate.
- Document whether historic daily batches can be acquired at Rp0. If not, explicitly plan prospective accumulation and defer historical backtests until sufficient history exists.
- Define limited intraday as selected-issuer/event research only. It is not a blocker if no compliant sample route is verified; record the limitation.
- Mark Gameplay A as assistant-created illustration only. Do not ask the user to define it and do not make it a Gate -1 blocker. The generic engine must support future user-originated and data-discovered gameplay.
- Keep BIONS and personal portfolio imports excluded; portfolio remains manual-first and local-only.
- Validate sample files without creating parsers, SQL or physical schema.

**Gate -1 sample scope:**
1. One cross-sectional IDX Ringkasan Saham batch file, preferably covering many issuers for one date, if obtainable through the official visible download route and permitted terms.
2. One official issuer disclosure/PUBEX document sample.
3. One financial statement sample (PDF/XLSX; XBRL only if actually available).
4. One official macro sample/route (BI publication or approved BPS WebAPI sample).
5. Selected-issuer intraday sample is optional and deferred if unavailable.

The user is not required to collect all documents or enter tickers manually. Codex/research should identify source routes and prepare the smallest exact request. The likely user action is at most one official broad-market batch download if the file cannot be obtained through an authorized route by the research process.

**Exit:** the primary broad-market batch is sample-verified or explicitly rejected with a documented fallback and actual coverage ceiling; source terms and acquisition modes are recorded; issuer-document and macro routes have sample/feasibility evidence or are explicitly deferred; historical range and universe policy are explicit; intraday limits are accepted; no physical schema/parser is based on guessed fields; Gameplay A is not treated as user input or blocker; no personal portfolio/BIONS data is requested.

## Sprint 0 — Repository governance

**Build:** Git baseline, Python environment, project skeleton, `AGENTS.md`, `CLAUDE.md`, `.gitignore`, test runner, changelog, backlog and ADR templates.

**Exit:** clean startup stub, no raw personal files/secrets/DB in Git, role boundaries and test command documented.

## Sprint 1 — Foundation & universal inbox

**Build:** local shell, SQLite connection/migration framework, source registry, immutable raw inbox, attachment archive, basic note/incident capture, audit metadata, backup/restore.

**Exit:** input survives restart; raw hash is stable; no extraction writes canonical facts; DB integrity and restore tests pass.

## Sprint 2 — Import Center & canonical market data

**Build:** batch-driven CSV/XLSX/PDF/XBRL import from local incoming folders, staging preview, field mapping, timezone/unit validation, row quarantine, dedup/idempotency, provenance and Data Health. One batch can contain many issuers; no per-ticker manual data entry.

**Exit:** the accepted real broad-market batch imports correctly; all rows are processed as a batch; unknown columns/units stop safely; re-import does not duplicate; coverage matches actual file rows; no broker automation or web crawler.

## Sprint 3 — Decision journal (start collecting learning data early)

**Build:** trade ideas, watch/no-trade/reject/missed decisions, immutable decision snapshot, revisions, executions as separate records, outcome horizon definitions.

**Exit:** user can journal without an active playbook; old incomplete trades remain incomplete; no chart-derived fake fills.

## Sprint 4 — IDX document and macro evidence library

**Build:** document catalog, URL/file capture, PDF text/XLSX parsing proof, extraction spans, XBRL sample feasibility, BI/BPS/macro observations with vintage/time semantics, manual news research capture.

**Exit:** one real sample per supported format parsed with source locations; unsupported/scanned documents quarantined; source claims distinct from achieved facts. XBRL normalization stays deferred unless sample tests pass.

## Sprint 5 — Issuer Daily Dossier

**Build:** security master from source-backed batch observations, trading sessions, daily EOD cross-section, issuer/date views, source coverage and optional selected-issuer Stockbit observation layer.

**Exit:** one batch import updates dossiers for every security actually present; one date/ticker dossier displays available/missing layers with provenance; no EOD-to-intraday inference; batch universe coverage is reproducible; full-market claims only if source coverage proves them.

## Sprint 6 — Incident Recorder and Market Episode construction

**Build:** capture timestamped incidents, connect docs/events/observations/issuers/clusters, episode chronology, hypothesis links and unresolved state.

**Exit:** an anomaly can be recorded before explanation, later linked to a public event without rewriting the original as-of knowledge.

## Sprint 7 — Stock Character and deterministic feature engine

**Build:** daily session character, rolling issuer personality, point-in-time fundamental/ownership feature links, market/cluster baselines, metric registry and immutable feature snapshots.

**Exit:** golden metric tests pass; sample/lookback/adjustment/units visible; fundamental/ownership facts use publication/effective-time semantics; sparse/ambiguous fields block unsupported features.

## Sprint 8 — Market response and historical episode research

**Build:** event windows, market-adjusted response, abnormal turnover/volume, follow-through/reversal, pre-event chronology and episode matching.

**Exit:** point-in-time replay, event overlap controls, negative examples and no-look-ahead tests pass; causal claims are not inferred from association.

## Sprint 9 — Open-world Opportunity Discovery

**Build:** top-down catalyst-led and bottom-up anomaly-led lanes, relationship/network expansion, issuer lifecycle/post-IPO cohorts where data exists, and cross-sectional pattern discovery. Implement discovery runs, universe/coverage snapshots, candidate funnel, source-backed exposure paths and research-lead records.

**Exit:** policy/macro input can surface issuer candidates outside saved watchlists through a supported transmission path; issuer anomaly can surface without known news; relationships are verified/reported/inferred/unverified; coverage/staleness explicit; briefs show support, contradiction, analogues, uncertainty and next questions.

## Sprint 10 — Conviction-support research brief

**Build:** link user's thesis to supporting/contradicting source facts, issuer exposure, market confirmation, historical analogues, unknowns and confirmation/invalidation conditions. Preserve the user's own conviction note separately.

**Exit:** user can inspect why a lead surfaced and evidence both for/against it. No system-generated single conviction score.

## Sprint 11 — Gameplay, setup-family and detector framework

**Build:** user-originated, data-discovered and hybrid gameplay; setup families and immutable setup-definition versions; supported declarative operators; detector/playbook lifecycle; shadow mode. Keep setup detection distinct from entry/exit execution policy.

**Exit:** raw gameplay survives unchanged; vague rules remain draft; exact numeric conditions and time semantics are versioned; no Setup Instance is confused with a trade; discovered patterns do not automatically become active strategies.

## Sprint 12 — Cross-issuer Setup Instance and parameter learning

**Build:** reproducible scans across declared universes; matched/non-matched/excluded/unavailable coverage; Setup Instances for every valid occurrence; point-in-time technical/fundamental/character/market/cluster/ownership feature snapshots; alternative entry scenarios; parameter experiments; cohort and sensitivity analysis; walk-forward/holdout evaluation.

**Exit:** an approved rule-based setup family (the Gameplay A file is illustrative only) can be scanned across multiple issuers and historical dates; MA10/MA20 distances are stored separately; all tested parameter variants and failures remain auditable; market-wide outcomes are separate from actual user fills; no optimality/probability claims without robust out-of-sample evidence; correlated issuer/date/event observations are not treated as independent by default.

## Sprint 13 — Personal Edge Learning

**Build:** compare user's observed/selected/rejected/missed/traded subset against the market-wide Setup Instance population; selection, entry, execution, management, risk and outcome analyses.

**Exit:** separates market setup quality, user's selection and execution; older trade rationales are not invented; personal confidence is distinct from model probability.

## Sprint 14 — Portfolio context (supporting, not central)

**Build:** account references, multi-account positions/exposures, basic weighted-average or FIFO method if needed, internal transfer handling, risk/concentration context.

**Exit:** account-level and consolidated views reconcile without double counting; any missing/stale input is visible; full bookkeeping remains out of scope.

## Sprint 15 — Intelligence delivery

**Build:** morning/research reports, issuer pages, episode explorer, Setup Instance explorer, open-world opportunity queue, parameter-study reports, research/conviction briefs, personal learning insights and source citations.

**Exit:** candidates outside saved watchlists are visible when coverage allows; each reading links to sources, metrics, episodes/instances and universe coverage; support and counter-evidence are available; facts, hypotheses and inferences remain distinct.

## Sprint 16 — Reliability and release

**Build:** restore drills, performance checks for 8 GB RAM machine, regression suite, docs, operational guide and release packaging.

**Exit:** clean install, backup restore, integrity checks, source/metric/learning tests and user acceptance pass.

## Sprint rule

Only one bounded sprint/ticket at a time. No sprint begins until prior exit criteria and user acceptance are satisfied. Scope additions go through feature intake/impact review and may be scheduled later; they do not silently expand active work.
