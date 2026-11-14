# MBI — Proposed Gate Structure (replaces the single "Gate -1")

**Status:** PROPOSED (revision 2, after Codex review) · **Date:** 14 Nov 2026 · **Author:** Claude · **Reviewer next:** Codex
**Why:** Gate -1 bundles four unrelated questions: Is the file understood? May we use it? What does it cover? Is there enough history? Each has a different evidence source, owner and consequence.

**Status vocabulary:** `OPEN` · `READY_FOR_APPROVAL` · `PASS` · `PASS_OBSERVED_SCOPE_ONLY` · `BLOCKED`

## 1. The four gates (current status)

| Gate | Question | Entry evidence | Current status | Exit (what makes it PASS) |
|---|---|---|---|---|
| **A. Sample & import-contract readiness** | Is the file's structure understood well enough to specify an importer? | One real file (done); contract drafted | **READY_FOR_APPROVAL** | Owner approves `IDX_SAMPLE_DATA_CONTRACT.md`; Codex finds no blocking defect; the open-semantics register (§13 of the contract) is accepted as "unknown, labelled" |
| **B. Source-terms & permission readiness** | Are acquisition, retention, processing, transformation and derived use of this source permitted? | Secondary evidence of IDX terms only. The owner's internal data-handling policy (D-H) exists but **is not a permission** | **OPEN** | The applicable current terms are read from primary sources and recorded (date, route, wording summary); permissions for each activity (acquire, retain, process, transform, derive) are independently verified or the activity is marked restricted/prohibited. Local-only handling does not satisfy this gate |
| **C. Universe readiness** | What does the batch cover? | 945 observed rows | **C1 Observed: READY_FOR_APPROVAL · C2 Verified: BLOCKED** | C1: observed-universe vocabulary approved (D-G). C2: an independent official security master is identified, its terms reviewed and a reconciliation of codes is performed and recorded. Until then `OBSERVED_ONLY` |
| **D. Historical-data sufficiency** | Is there enough point-in-time history for research claims? | 1 session in hand (13 Nov 2026) | **BLOCKED for research · no effect on importer work** | Per-claim thresholds in §4 are met by accumulated, validated sessions |

Gate A passing does **not** imply B, C2 or D. A statement such as "Gate -1 passed" is no longer allowed; always name the sub-gate.

## 2. What can proceed under each status

| Activity | Needs | Status today |
|---|---|---|
| Spec patching (docs only) | Owner approval of decisions | Allowed on approval |
| **Sprint 0** (foundation, no data) | Owner approval + P0-docs accepted. No data gate. | Allowed on approval |
| Importer development on **synthetic fixtures built from the contract's structure** (headers, types, rules; no IDX values) | A = PASS | Allowed after A |
| Running the importer or conformance checks on the **real IDX sample or real batches** (any processing of IDX data) | A = PASS · **B = PASS**. If the owner nonetheless wants to proceed earlier, that is a recorded owner risk acceptance, not a permission finding, and is a separate decision | Waits for B |
| Observed-universe dossiers and one-day cross-sectional views (rank, distribution, same-day outliers) | A · **B PASS** · C1; every screen labelled `OBSERVED_ONLY`, one session, descriptive | After importer |
| Any coverage percentage or "whole market" wording | C2 PASS | Blocked |
| Lookback features (MA, ATR, relative volume, personality) | D: feature's declared minimum sessions met; corporate-action flags applied | Blocked until accumulated |
| Exploratory research (descriptive, labelled `EXPLORATORY`) | D provisional exploratory reference (§4) | Blocked |
| Setup scans / parameter experiments | D provisional exploratory reference (§4); setup version complete; scan states per C3 | Blocked |
| Backtest, probability, "works" claims | D provisional strong-claim reference (§4) + time-based holdout + effective-sample rule (Doc 18 §9) | Blocked (Decision D-D) |
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
| 12–13 (setup learning, personal edge comparison) | D provisional exploratory reference for any real-data result; provisional strong-claim reference for strong claims, each replaced by the study's declared sufficiency plan |

## 4. History-sufficiency reference points (PROVISIONAL JUDGMENT DEFAULTS)

**Label: `PROVISIONAL_DEFAULT`.** The figures 120 and 250 trading sessions (and the 30-group and 60-session conditions) are **judgment-based placeholders. They are not statistically validated, not universal minimums, and not measured from this data.** They exist so the gate has something concrete to show until the owner approves or replaces them.

Evidence sufficiency actually depends on: the feature and its lookback; the timeframe; data quality and gap count; point-in-time availability (vintages, publication times); corporate-action handling; the number of *independent* events, not rows; the number of market regimes covered; and the research objective and claim strength. A study must declare its own sufficiency plan and justify it before running (Doc 18 §7 step 1). A declared minimum may be higher or, with written justification reviewed by the owner, different from the placeholder. Any result displayed on a placeholder carries the label `PROVISIONAL_DEFAULT`.

| Claim class | Provisional reference (sessions) | Additional conditions |
|---|---|---|
| Importer/contract validation | 1 file (met); later ≈5 spread dates | — |
| Single-day descriptive view | 1 | Labelled `OBSERVED_ONLY` |
| Lookback feature value | Metric contract's justified `min_sessions` = lookback + warm-up (e.g. a 20-session median baseline needs 20 prior valid sessions plus the current one; MA20 needs 20 completed bars) | Otherwise `INSUFFICIENT_DATA`, never a shortened window |
| Rolling issuer personality, `EXPLORATORY` | 120 sessions (≈ 6 months; provisional) | Gap count shown; only issuers meeting the per-issuer minimum are included; corporate-action windows flagged |
| Setup/parameter study, `EXPLORATORY` | 120 sessions and 30 independent occurrence groups in each compared cohort (both provisional) | Raw vs effective sample reported (Doc 18 §6) |
| `OUT_OF_SAMPLE_SUPPORTED` / any probability | 250 sessions (≈ 1 year), a time-based holdout of 60 untouched sessions, 30 effective independent events in the holdout (all provisional), calibration per Doc 09 §8 | Single-regime caveat shown if history spans one regime |

Common conditions for every class:
- Sessions count only if the artifact passed validation and is point-in-time (vintage recorded).
- Missing sessions (no file for a trading date) are counted and shown as gaps. A trading-calendar source is required to know which dates were trading dates; until verified, gap detection is `UNVERIFIED`.
- Fundamental and ownership features follow publication-time rules (Doc 09 §4) regardless of price history length.

**Consequence worth stating plainly:** if the placeholders are adopted, 120 sessions is roughly 6 calendar months and 250 sessions about 1 year from a standing start, unless a lawful historical batch is added later (D-F, and subject to Gate B). Data observed on the page back to 25 Mar 2026 would be roughly 7.5 months; whether it may be acquired is a Gate B question and whether it suffices is a per-study question. Nothing here says 120 or 250 is enough or necessary.

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
