# MBI — Proposed Gate Structure (replaces the single "Gate -1")

**Status:** PROPOSED · **Date:** 14 Nov 2026 · **Author:** Claude · **Reviewer next:** Codex
**Why:** Gate -1 bundles four unrelated questions: Is the file understood? May we use it? What does it cover? Is there enough history? Each has a different evidence source, owner and consequence.

**Status vocabulary:** `OPEN` · `READY_FOR_APPROVAL` · `PASS` · `PASS_OBSERVED_SCOPE_ONLY` · `BLOCKED`

## 1. The four gates (current status)

| Gate | Question | Entry evidence | Current status | Exit (what makes it PASS) |
|---|---|---|---|---|
| **A. Sample & import-contract readiness** | Is the file's structure understood well enough to specify an importer? | One real file (done); contract drafted | **READY_FOR_APPROVAL** | Owner approves `IDX_SAMPLE_DATA_CONTRACT.md`; Codex finds no blocking defect; the open-semantics register (§13 of the contract) is accepted as "unknown, labelled" |
| **B. Intended-use & source-terms readiness** | May we store, derive from and keep this data, and for what purpose? | Secondary evidence of IDX terms only | **OPEN** | Owner accepts the intended-use statement (D-H); a primary-source read of the current IDX terms is recorded (date, route, wording summary); anything uncleared is listed as a restriction |
| **C. Universe readiness** | What does the batch cover? | 945 observed rows | **C1 Observed: READY_FOR_APPROVAL · C2 Verified: BLOCKED** | C1: observed-universe vocabulary approved (D-G). C2: an independent official security master is identified, its terms reviewed and a reconciliation of codes is performed and recorded. Until then `OBSERVED_ONLY` |
| **D. Historical-data sufficiency** | Is there enough point-in-time history for research claims? | 1 session in hand (13 Nov 2026) | **BLOCKED for research · no effect on importer work** | Per-claim thresholds in §4 are met by accumulated, validated sessions |

Gate A passing does **not** imply B, C2 or D. A statement such as "Gate -1 passed" is no longer allowed; always name the sub-gate.

## 2. What can proceed under each status

| Activity | Needs | Status today |
|---|---|---|
| Spec patching (docs only) | Owner approval of decisions | Allowed on approval |
| **Sprint 0** (foundation, no data) | Owner approval + P0-docs accepted. No data gate. | Allowed on approval |
| Importer development on the sample, local only | A = PASS · B = intended-use statement accepted (primary terms read recommended but not required for local dev) · C1 | Allowed after A and D-H |
| Importing real daily batches into the local store | A PASS · B PASS (primary terms read recorded) · C1 | Waits for B |
| Observed-universe dossiers and one-day cross-sectional views (rank, distribution, same-day outliers) | A · B · C1; every screen labelled `OBSERVED_ONLY`, one session, descriptive | After importer |
| Any coverage percentage or "whole market" wording | C2 PASS | Blocked |
| Lookback features (MA, ATR, relative volume, personality) | D: feature's declared minimum sessions met; corporate-action flags applied | Blocked until accumulated |
| Exploratory research (descriptive, labelled `EXPLORATORY`) | D exploratory floor (§4) | Blocked |
| Setup scans / parameter experiments | D exploratory floor; setup version complete; scan states per C3 | Blocked |
| Backtest, probability, "works" claims | D validated floor (§4) + time-based holdout + effective-sample rule (Doc 18 §9) | Blocked (Decision D-D) |
| Cross-date sample for format stability (≈5 dates) | Importer passes initial tests; B | Deferred (D-F) |
| Historical backfill (~150 files) | Owner decision after importer proves stable; manual downloads only | Not now |
| Sharing, exporting or publishing IDX-derived data, committing raw files | Written IDX permission | Prohibited |
| Issuer-document / macro parsers | Samples before Sprint 4 (D-A); source terms per source | Deferred |

## 3. Where each gate bites in the roadmap

| Sprint | Gates required |
|---|---|
| 0 | none |
| 1 | none |
| 2 (Import Center) | A, B, C1 (real-batch exit also needs D-F's later cross-date sample for format stability) |
| 5 (Dossier) | C1; coverage language per C2 |
| 7–8 (features, response research) | D (feature minimum sessions) |
| 12–13 (setup learning, personal edge comparison) | D exploratory floor for any real-data result; D validated floor for strong claims |

## 4. History-sufficiency thresholds (proposed defaults)

These are **judgment-based starting values, not measured facts**. A study may set higher minimums in its pre-declared plan (Doc 18 §7 step 1); it may never set lower than the floor of its claim class.

| Claim class | Minimum accumulated validated sessions | Additional conditions |
|---|---|---|
| Importer/contract validation | 1 file (met); later ≈5 spread dates | — |
| Single-day descriptive view | 1 | Labelled `OBSERVED_ONLY` |
| Lookback feature value | Metric contract's `min_sessions` = lookback + warm-up (e.g. a 20-session median baseline needs 20 prior valid sessions plus the current one; MA20 needs 20 completed bars) | Otherwise `INSUFFICIENT_DATA`, never a shortened window |
| Rolling issuer personality, `EXPLORATORY` | ≥ 120 sessions (≈ 6 months) | Gap count shown; only issuers meeting the per-issuer minimum are included; corporate-action windows flagged |
| Setup/parameter study, `EXPLORATORY` | ≥ 120 sessions and ≥ 30 independent occurrence groups in each compared cohort | Raw vs effective sample reported (Doc 18 §6) |
| `OUT_OF_SAMPLE_SUPPORTED` / any probability | ≥ 250 sessions (≈ 1 year), a time-based holdout of ≥ 60 untouched sessions, ≥ 30 effective independent events in the holdout, calibration per Doc 09 §8 | Single-regime caveat shown if history spans one regime |

Common conditions for every class:
- Sessions count only if the artifact passed validation and is point-in-time (vintage recorded).
- Missing sessions (no file for a trading date) are counted and shown as gaps. A trading-calendar source is required to know which dates were trading dates; until verified, gap detection is `UNVERIFIED`.
- Fundamental and ownership features follow publication-time rules (Doc 09 §4) regardless of price history length.

**Consequence worth stating plainly:** from a standing start, about 120 sessions is roughly 6 calendar months and 250 sessions about 1 year, unless a lawful historical batch is added later (D-F). The observed availability on the page back to 25 Mar 2026 would give roughly 7.5 months, still short of the validated floor on its own.

## 5. Mapping from the old Gate -1

| Old exit criterion (Decision Record / Doc 16 §9) | New home |
|---|---|
| Batch route sample-verified or rejected | A |
| Format, fields, units, date, coverage inspected | A (structure); C (coverage); contract §13 (semantics) |
| Document/macro routes understood or deferred | Deferred by D-A; evidence before Sprint 4 |
| Terms and acquisition mode recorded | B |
| Universe, history, coverage policy explicit | C and D |
| Intraday supported or deferred | Deferred; not a gate |
| Gameplay A illustrative only | Documentation fix C6; not a gate |
| No schema/parser from guessed fields | Standing rule; contract §13 and IC-60/61 |
| Logical-model corrections approved | Decision D-B (document track) |

## 6. Decision Record update (proposal)

Replace the single status line `BLOCKED — source/sample verification pending` with:
`A: READY_FOR_APPROVAL · B: OPEN · C1: READY_FOR_APPROVAL / C2: BLOCKED · D: BLOCKED (research only)`
Dated, with a pointer to this document.
