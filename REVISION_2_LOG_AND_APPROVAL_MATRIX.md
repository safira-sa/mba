# MBI Proposal Revision 2 — Change Log and Approval Matrix

**Date:** 14 Nov 2026 · **Trigger:** Codex independent review, verdict REVISE · **Canonical documents untouched.**
**Input limits:** the Codex files (`REVIEW_VERDICT.md`, `FINDINGS.md`, `APPROVAL_MATRIX.md`) were not attached. I worked from the five blockers as summarized in the request. Any additional Codex findings in those files are **not addressed** until they are provided.

## 1. Exact changes

### SPEC_PATCH_PROPOSAL.md
| Section | Change | Blocker |
|---|---|---|
| Header | Revision 2 note; owner decisions not treated as approved | — |
| §0 "Evaluation identity (C2, revised)" | Replaced the single instance key with four concepts: occurrence key (version + security + as-of), scan membership (scan + security + as-of; denominator only), evaluation record (`evaluation_id`; natural key = scan run + setup version + security + as-of + data vintage), data vintage. | 1, 2 |
| §0 "Repeated evaluation…" | New: how the same occurrence on different vintages or different scan runs is represented; no implicit "current"; determinism check | 1, 2 |
| §0 "Evaluation state (C3, revised)" | State set applied per evaluation record; counts rederived; out-of-scope reported separately | 1 |
| §0 "Data vintage (N9)" | New definition, immutability, cutoff-based reproducibility, list of record types that must carry it (incl. outcomes via a separate outcome vintage) | 2 |
| §0 "Value-state vocabulary (N10)" | New: field value states and session/trading status; no state becomes zero or implies bearishness; effect on metrics, scan rows, aggregates | 4 |
| §0 coverage snapshot (C1) | Counts reworded to match C3; `data_vintage_id` added | 1, 2 |
| §1 rows C2, C3 | Rewritten with new proposed changes and acceptance tests (two definitions in one scan; same occurrence on two vintages; same-vintage rerun) | 1, 2 |
| §2 row D12 | 120/250 relabelled provisional; per-metric justified `min_sessions`; defaults kept in one labelled reference place | 5 |
| §3 row N2 | Rewritten as internal data-handling policy with three disclaimers; Gate B stays OPEN | 3 |
| §3 new rows N9, N10 | Vintage throughout; value states in the logical model | 2, 4 |
| §4 | Patch order includes N9, N10 | — |

### IDX_SAMPLE_DATA_CONTRACT.md (v0.2)
| Section | Change | Blocker |
|---|---|---|
| Header | Permission note: structure only; no permission asserted; inspection not evidence of permission | 3 |
| IC-05 | D-H described as an internal policy, not an IDX permission | 3 |
| New §5A | Value-state table with sample evidence (e.g. `Open Price` = 0 on 238 traded rows → `NOT_REPORTED`; `Volume/Nilai/Frekuensi` = 0 together → `OBSERVED_ZERO_CONFIRMED`; `Offer`/`Bid` = 0 → `NO_QUOTE`); separate session-status axis; sample never supports `SUSPENDED_EXPLICIT` | 4 |
| IC-50/51 | Aligned to `NO_REGULAR_ACTIVITY_REPORTED` and `TRADING_STATUS_UNKNOWN` | 4 |
| IC-71 | Coverage counts by value state | 4 |

### GATE_STRUCTURE_PROPOSAL.md
| Section | Change | Blocker |
|---|---|---|
| §1 Gate B row | Renamed "Source-terms & permission readiness"; exit = primary-source terms read and per-activity permission verified; local-only handling does not satisfy it | 3 |
| §2 matrix | Split importer work: synthetic fixtures need Gate A only; any processing of real IDX data (sample or batches) needs Gate B PASS; earlier proceeding is a separate recorded owner risk acceptance, not a permission finding. Dossier row now requires B PASS | 3 |
| §4 | Retitled "reference points (provisional judgment defaults)"; label `PROVISIONAL_DEFAULT`; not validated, not universal; dependence on feature, timeframe, quality, point-in-time availability, objective; study declares own plan; "floor" wording removed; numbers shown as provisional | 5 |

### OWNER_DECISION_SHEET.md
| Section | Change | Blocker |
|---|---|---|
| Evidence table | Terms row now covers download, retain, process, transform, derive; Gate B OPEN | 3 |
| D-B (C2, C3) | Rewritten in plain language to the four-concept model; re-read requested | 1, 2 |
| D-C | Provisional labelling and sufficiency dependencies | 5 |
| D-G | Importer design on synthetic fixtures; real data waits for Gate B | 3 |
| D-H | Rewritten as internal policy with explicit disclaimers; Gate B unaffected | 3 |
| Summary | Blocker list names Gate B permissions | 3 |

### SPRINT_0_PROPOSAL.md
Start condition 1, S0-8 and the dependency note now describe D-H as an internal policy that clears nothing; Sprint 0 touches no data.

## 2. Unresolved issues

1. Additional Codex findings (if any) not seen.
2. Gate B: primary-source terms and per-activity permissions remain unverified; nothing in these documents settles them.
3. The "scan membership" / "evaluation record" split adds a level; Codex should confirm it is the minimum needed.
4. A vintage selection rule (e.g. "latest retrieved ≤ cutoff") is described, not decided; the default rule needs owner or Codex confirmation.
5. Official IDX definitions for units, `Remarks`, `Weight For Index`, `Tradeble Shares`, non-regular fields; security master; corporate actions; trading calendar (as before).
6. Provisional numbers (120, 250, 30, 60) have no empirical basis yet.

## 3. Approval matrix (none approved)

| ID | Decision | Revision effect | State |
|---|---|---|---|
| D-A | Defer issuer-document and macro samples to before Sprint 4 | Unchanged | PENDING OWNER |
| D-B | Approve C1–C5 | **C2 and C3 materially changed**; re-read before approving | PENDING OWNER (re-review) |
| D-C | History-sufficiency gate | Mechanism unchanged; numbers now provisional placeholders | PENDING OWNER (re-review) |
| D-D | Backtests wait for sufficient point-in-time data | Unchanged | PENDING OWNER |
| D-E | External LLM off by default | Unchanged | PENDING OWNER |
| D-F | No backfill now; later small cross-date sample | Unchanged; any later download also subject to Gate B | PENDING OWNER |
| D-G | Observed vs verified universe | Importer on synthetic fixtures until Gate B | PENDING OWNER (re-review) |
| D-H | Internal data-handling policy | **Rewritten**; not a permission; Gate B untouched | PENDING OWNER (re-review) |

## 4. Are the five Codex blockers resolved?

| # | Blocker | Addressed in the proposals? | Resolved? |
|---|---|---|---|
| 1 | Scan membership not unique across several setup definitions | Yes (membership without version; evaluation record with version) | Pending Codex re-review |
| 2 | Vintage not explicit | Yes (N9, key, record, outcomes, studies) | Pending Codex re-review |
| 3 | D-H implied IDX permission | Yes (rewritten; Gate B text; importer on real data gated by B) | Pending Codex re-review |
| 4 | Zero/missing/no-trade not in the logical model | Yes (N10, contract §5A) | Pending Codex re-review |
| 5 | Thresholds not labelled provisional | Yes (§4 rewrite, D-C) | Pending Codex re-review |

**Statement:** all five blockers are addressed in the proposal text. None is declared resolved, because resolution requires Codex re-review. No canonical document was modified, no code written, no data fetched, no owner decision approved.
