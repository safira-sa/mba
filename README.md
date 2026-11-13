# MBI v0.9.2 — Zero-Budget Batch-First Acquisition Strategy

**Status:** Consolidated product architecture and Gate -1 operating decision. **Not an application release.**  
**Supersedes:** v0.8 as the active specification. v0.5–v0.8 packs remain historical references only; do not combine them with this pack.  
**Current permitted action:** Gate -1 closure and source/sample feasibility only. No app implementation, physical schema, migration, parser, scanner or backtest is authorized yet.

## Product in one sentence

MBI is a local-first Indonesian equity open-world market-behavior research and learning system that discovers leads beyond the user's radar, links external context to issuer/market response, studies repeated setup occurrences across issuers and circumstances, and supports the user's independent analysis and personal trading edge.

## Central product flow

`External Context + Broad Market Observations → Open-World Discovery → Exposure Mapping → Market Episodes / Response Research → Historical Learning → Setup Scans & Instances → Cross-Issuer / Parameter Studies → Gameplay & Playbook Evaluation → Personal Edge → Evidence Briefs → Updated Research`

Market Episode & Response Learning is the context/event research core. Open-World Opportunity Discovery is a first-class front door. **Setup Instance & Parameter Learning** tests a gameplay/setup across many issuers and circumstances, not only the user's executed trades. Personal Edge evaluates how the user noticed, selected and executed a subset of those opportunities. Portfolio remains supporting context.


## Current operating strategy (ADR-004)

- Data/API budget: Rp0; no paid provider, plan upgrade or paid entitlement dependency.
- Broad market: user-triggered official batch download/API batch where permitted; one batch should contain many issuers. No per-ticker manual market-data entry.
- After intake: automated local archiving, parsing, validation, normalization, daily feature calculation, dossier update and discovery queue.
- Intraday: selected issuer/event only; not broad-market scanning.
- TradingView Basic export is unavailable to the user and is not required. BIONS/personal account exports are excluded. Portfolio is manual-first and local-only.
- IDX website scraping/crawling is prohibited by published website terms; the visible Ringkasan Saham download route is a candidate, not yet a verified source until actual file/coverage is tested.
- If free historic EOD batch history is unavailable, MBI accumulates validated batches prospectively and reports the true history window.


## Critical unit distinctions

- **Discovery Candidate:** a lead surfaced by catalyst, anomaly, network, lifecycle or cross-sectional scan.
- **Market Episode:** a scoped context/event/anomaly research container that may involve many securities.
- **Setup Family / Version:** a reusable idea and its exact versioned measurable conditions.
- **Setup Instance:** one occurrence of a setup version for one security at one timestamp, whether traded or not.
- **Entry Scenario:** a hypothetical entry/management assumption for research; never an actual fill.
- **Trade Idea / Decision:** what the user considered, rejected, watched or entered.
- **Execution Fill:** an actual transaction supported by a source or explicit user entry.

These are related through explicit links but must never be merged into one generic "trade" or "signal" record. See Doc 18.

## Read order

1. `CODEX_START_HERE_v0.9.2.md` — active Gate -1 closure task.
2. `GATE_MINUS_1_DECISION_RECORD.md` — user's accepted operating strategy and outstanding blockers.
3. `QA_REVIEW_REPORT.md` — document-level QA and known non-validation boundaries.
4. `docs/00_PRODUCT_CHARTER_AND_TERMS.md` and `docs/01_END_TO_END_LEARNING_FLOW.md` — product and learning flow.
5. `docs/02_DATA_SOURCES_AND_INGESTION.md` — batch-first source acquisition, local processing and fallback.
6. `docs/13_SOURCE_REGISTER.md` and `docs/16_SAMPLE_DATA_CHECKLIST.md` — source status and minimum samples.
7. `docs/03`–`docs/09` — issuer, episodes, gameplay, journal, rant intake, portfolio and metric contracts.
8. `docs/10_SOFTWARE_ARCHITECTURE_AND_AGENT_GOVERNANCE.md` — maintainable architecture and agent workflow.
9. `docs/11_ROADMAP_AND_ACCEPTANCE_GATES.md` and `docs/12_QA_TEST_MATRIX.md` — gates and tests.
10. `docs/14_LOGICAL_DATA_MODEL.md` and `docs/15_PRODUCT_EVOLUTION_AND_COMPATIBILITY.md` — logical entities and future changes.
11. `docs/17` and `docs/18` — open-world discovery and cross-issuer setup learning.
12. `docs/decisions/ADR-001` through `ADR-004` — accepted/proposed architectural decisions.

## Non-negotiable product constraints

- Local-first, Windows-friendly, single-user first, initial data/API budget Rp0.
- No broker login automation, credential/PIN/OTP/SID/SRE/RDN storage, automatic orders, undocumented endpoints, access bypass, quota circumvention or aggressive bulk scraping.
- No assumption of complete/full-market/live data. Every result shows covered universe, source, field coverage, as-of time and known gaps.
- Base system works without a runtime LLM or paid API. External AI is optional and cannot replace deterministic calculations.
- Raw inputs/files are immutable. Extraction is a proposal. User acceptance does not verify facts.
- Numerical features are deterministic, versioned and tested. LLMs may propose extraction/classification with source spans, never authoritative arithmetic or direct canonical writes.
- Missing/stale/conflicting data is unknown, never silently zero or bearish.
- No future information in historical replay. Fundamental and ownership facts use point-in-time publication/effective semantics.
- No probability without defined label/horizon/cohort/independent sample, leakage controls, time-based out-of-sample validation and calibration.
- Parameter experiments retain every variant tested; no single in-sample optimum is represented as universally best.
- Market-wide setup instances are separate from user trades. Hypothetical entries are not fills.
- New patterns may be proposed; active production setup/playbook versions require review and approval.

## What v0.9 clarifies and adds

1. Preserves v0.8 open-world discovery and Market Episode research core.
2. Adds Setup Family, immutable Setup Definition Version, Setup Scan, Setup Instance, Entry Scenario, Parameter Experiment/Variant and Setup Outcome as distinct concepts.
3. Requires market-wide setup scans across all eligible covered issuers/time observations, not only user trades or watchlists.
4. Captures continuous technical values and point-in-time fundamentals, Stock Character, market regime, sector/cluster and dated ownership/UBO context for each occurrence when available.
5. Separates setup detection, alternative hypothetical entry scenarios, actual user decisions and actual fills.
6. Adds cross-issuer cohorts, parameter sensitivity/robustness regions, negative cases and dependence-aware sample accounting.
7. Updates the roadmap to place setup-instance/parameter learning before Personal Edge.
8. Keeps portfolio supporting; no SQL migration is authorized by this document pack.
9. v0.9.2 records the approved zero-budget batch-first acquisition mode, selected-issuer-only intraday scope, automatic local processing after intake, and exclusion of TradingView Basic/BIONS dependencies.

## Active prompt hygiene

`CODEX_START_HERE_v0.9.2.md` is the single active Codex task instruction. `prompts/CODEX_AUDIT_PROMPT.txt` is only a short pointer to it. Historical supersession maps are archival context, not instructions. Do not copy old v0.7/v0.8 prompt text into a current task.

## No code authorization

The next Codex task is a read-only Gate -1 source/batch-feasibility closure. Claude independently reviews that report. Do not build the app, add dependencies, create migrations, scrape/crawl sources, create parsers or backtests until the user explicitly approves a bounded sprint.
