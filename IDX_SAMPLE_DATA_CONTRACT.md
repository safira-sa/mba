# IDX Ringkasan Saham — Sample-Derived Import Contract (Schema-Agnostic)

**Status:** PROPOSED v0.2 (revised after Codex review) · **Date:** 14 Nov 2026 · **Not a SQL/physical schema.** No code, no parser.
**Permission note:** this contract describes file structure only. It does **not** assert that downloading, retaining, processing, transforming or deriving from IDX data is permitted. Source-terms readiness (Gate B) is OPEN. The structural statistics below came from a read-only inspection of the owner-provided copy; that inspection is not evidence of permission.
**Evidence base:** one file only (13 Nov 2026). Every "observed" statement below is true for that file alone. Rules marked **POLICY** are design decisions, not facts about IDX. Nothing here is IDX-documented unless stated; no official field dictionary has been seen.

## 1. Observed facts (one file)

| Fact | Value |
|---|---|
| Name as reported by owner | `Ringkasan Saham-20261113` (copy in analysis environment: `Ringkasan_Saham-20261113.xlsx`) |
| Size / SHA-256 (of the analyzed copy) | 828,218 bytes / `96989e7521c4e2494217481895891e039772a3262d5ae6e5f9fc4323c2e67d0c` |
| Container | `.xlsx`, one sheet `Sheet1`, header row 1, 945 data rows, 28 columns (A1:AB946) |
| Generator property | `openpyxl`; no Excel signature, no shared-strings part → consistent with server-side generation at download, not a manual re-save (inference) |
| Embedded timestamps | created = modified = 2026-11-14 01:44:11, timezone not stated in the file |
| Selection when downloaded | Owner: page default (Fri 13 Nov 2026), no filter changed; page showed 945 total rows |
| Row identity | 945 unique `Kode Saham`, 0 duplicates, sorted ascending; `No` = 1..945; 944 four-letter codes, one five-letter (`GOTOM`) |
| Activity | 834 rows with Volume>0; 111 with Volume=0 (and Frekuensi=0 on exactly those) |

## 2. Raw artifact and provenance rules

- **IC-01 (POLICY)** Store the original bytes unmodified; compute SHA-256 on first read; never open-and-resave the original. Parsing works on a read-only copy.
- **IC-02** Record both the owner-reported filename and the stored filename (platform may rewrite spaces).
- **IC-03** The acquisition record carries: source page URL; acquisition mode `OFFICIAL_UI_BATCH_DOWNLOAD`; "user-triggered" flag; owner-declared date/filters (here: default, none changed); owner-observed page total; retrieval time (owner-supplied or file-system time, labelled which); terms-review status at that date; generator metadata observed (`openpyxl`, embedded timestamps with timezone `UNKNOWN`).
- **IC-04** The page total is a **corroborating declaration**, never a coverage proof. Here file rows (945) = page total (945), which supports "export contained the full filtered set" for this one state only.
- **IC-05** The sample is a real artifact. Synthetic fixtures derived from it must be labelled `SYNTHETIC_TEST_ONLY` and kept separate. The real file stays outside Git (internal owner policy D-H; the policy is not an IDX permission).

## 3. Structural validation (hard stops → whole file quarantined)

- **IC-10** Exactly one sheet named `Sheet1`; header in row 1.
- **IC-11** Header must equal this ordered list, spelling as observed (including `Tradeble`): `No, Kode Saham, Nama Perusahaan, Remarks, Sebelumnya, Open Price, Tanggal Perdagangan Terakhir, First Trade, Tertinggi, Terendah, Penutupan, Selisih, Volume, Nilai, Frekuensi, Index Individual, Offer, Offer Volume, Bid, Bid Volume, Listed Shares, Tradeble Shares, Weight For Index, Foreign Sell, Foreign Buy, Non Regular Volume, Non Regular Value, Non Regular Frequency`. Any difference (added, missing, renamed, reordered) → quarantine and request mapping. **No guessing by column position.**
- **IC-12** Expected cell types as observed: text for `Kode Saham`, `Nama Perusahaan`, `Remarks`, date; integers elsewhere; `Index Individual` mixed integer/decimal (767 decimal, 178 integer). Unexpected type in a cell → that row quarantined with reason.
- **IC-13** Row count, unique-code count, duplicate codes recorded for every import. Duplicate code within a file → quarantine duplicates, never pick one.

## 4. Batch date vs last-trade date

- **IC-20** `Tanggal Perdagangan Terakhir` equals `13 Nov 2026` on **all 945 rows, including all 111 rows with no regular activity.** It therefore cannot be a per-security last-trade date in this file. **Treat it as a batch-date echo only.** Never use it as evidence a security traded that day.
- **IC-21** Trading date of the batch is established from three independent signals: owner-declared page date, the column value, and the filename suffix (`20261113`, weak signal). All must agree, otherwise quarantine for review. Single-source date is flagged `DATE_SINGLE_SOURCE`.
- **IC-22** Actual last-trade date per security is `UNKNOWN` from this source. It may be derived only from accumulated history (latest prior batch where regular activity was reported), labelled as derived and bounded by available history.
- **IC-23** Date parsing: store original text unchanged **plus** parsed date. Accept only exact observed pattern `DD Mon YYYY` with English month abbreviations (observed: `Nov`). Indonesian month names (e.g. `Agu`, `Okt`, `Mei`, `Des`) or any other pattern → quarantine, not locale-guessed. The value is a date without time or timezone; interpret as an IDX trading date in `Asia/Jakarta` for labelling only. Publication time `UNKNOWN`.

## 5. Field catalogue

Columns: observed content · zero/missing rule · unit status. "Raw" always preserved; "derived" values are separate and labelled.

| # | Column | Observed | Zero / missing rule | Unit |
|---|---|---|---|---|
| 1 | `No` | 1..945 sequential | Row position only; no business meaning | n/a |
| 2 | `Kode Saham` | Uppercase letters, 4–5 chars | Identity key candidate **within the batch only**; ticker ≠ stable security identity | n/a |
| 3 | `Nama Perusahaan` | Text ("… Tbk." on all rows) | Raw text; names change | n/a |
| 4 | `Remarks` | 30-character string on all rows, 702 distinct | **Retained raw, uninterpreted** (IC-64) | n/a |
| 5 | `Sebelumnya` | Integer | Previous close as labelled. 0 → quarantine row (not checked in this inspection; rule is POLICY) | `UNIT_UNKNOWN` (IDR plausible) |
| 6 | `Open Price` | 0 on 349 rows, **including 238 rows with Volume>0** | **0 = NOT_REPORTED → null.** Never a price of 0. | `UNIT_UNKNOWN` |
| 7 | `Tanggal Perdagangan Terakhir` | See §4 | Batch-date echo | n/a |
| 8 | `First Trade` | 0 on 349 rows (same count as Open; 238 traded) | 0 = NOT_REPORTED → null | `UNIT_UNKNOWN` |
| 9 | `Tertinggi` | 0 on exactly the 111 no-activity rows; never 0 on traded rows | No-activity → null (`NOT_APPLICABLE_NO_ACTIVITY`) | `UNIT_UNKNOWN` |
| 10 | `Terendah` | Same as `Tertinggi` | Same | `UNIT_UNKNOWN` |
| 11 | `Penutupan` | Integer; on all 111 no-activity rows equals `Sebelumnya` | Carried-forward close on no-activity rows: **flag `CLOSE_CARRIED_FORWARD`, not a new observation.** 0 → quarantine (POLICY, not checked) | `UNIT_UNKNOWN` |
| 12 | `Selisih` | Equals `Penutupan − Sebelumnya` on all 945 rows | Source-reported; keep raw; identity check (§6) | `UNIT_UNKNOWN` |
| 13 | `Volume` | 0 on 111 rows | See §7. Do **not** add or subtract `Non Regular Volume` | **Cross-check consistent with shares, not documented** |
| 14 | `Nilai` | Integer, large (>1e11 occurs in sample) | Zero only with zero volume (rule); never replaced by `close×volume` | **Cross-check consistent with IDR, not documented** |
| 15 | `Frekuensi` | 0 on exactly the Volume=0 rows | Same as `Volume` | count; definition not documented (Doc 03 §2.C applies) |
| 16 | `Index Individual` | Decimal/integer, never 0 | Meaning **unknown**; do not use in any metric | `UNIT_UNKNOWN` |
| 17 | `Offer` | 0 on 129 rows | 0 = NO_QUOTE → null | `UNIT_UNKNOWN` |
| 18 | `Offer Volume` | 0 on all rows where `Offer`=0 (observed) | Zero with a quote may be real; keep raw | `UNIT_UNKNOWN` |
| 19 | `Bid` | 0 on 120 rows | 0 = NO_QUOTE → null | `UNIT_UNKNOWN` |
| 20 | `Bid Volume` | Integer | Raw; (bid-side counterpart of Offer Volume not separately checked) | `UNIT_UNKNOWN` |
| 21 | `Listed Shares` | Integer, up to ~2.5e10 | Raw; capital-structure field as labelled | `UNIT_UNKNOWN` |
| 22 | `Tradeble Shares` | Equals `Listed Shares` on 909 of 945 rows | **Not free float** (IC-65) | `UNIT_UNKNOWN` |
| 23 | `Weight For Index` | 0 on 423 rows | Meaning of 0 unknown (not in index vs not reported). Raw kept; derived null; unused | `UNIT_UNKNOWN` |
| 24 | `Foreign Sell` | Integer; zeros occur | 0 is **ambiguous** (true zero vs not reported). Raw kept, flag `ZERO_AMBIGUOUS`; no foreign-net metric until defined | `UNIT_UNKNOWN` |
| 25 | `Foreign Buy` | Same | Same | `UNIT_UNKNOWN` |
| 26 | `Non Regular Volume` | >0 on 85 rows; exceeds `Volume` on 22 | See §8 | `UNIT_UNKNOWN` |
| 27 | `Non Regular Value` | Integer | See §8 | `UNIT_UNKNOWN` |
| 28 | `Non Regular Frequency` | Integer | See §8 | count; definition unknown |

**Quote fields (Offer, Offer Volume, Bid, Bid Volume):** **IC-30** They are closing quote snapshots as labelled, taken at an unstated time. Not order flow, not depth, not executed trades, not a spread history. Spread or imbalance features computed from them are labelled "single best-level snapshot, snapshot time unknown".

## 5A. Value-state and session-status vocabulary (carried into the logical model)

Every field observation keeps: raw value (exactly as in the file) + `value_state` + reason code. States never become numeric zero and are never read as bearish or as a signal.

| `value_state` | Meaning | Sample evidence (structural, not officially confirmed) |
|---|---|---|
| `OBSERVED` | A non-zero, non-blank value | Most cells |
| `OBSERVED_ZERO_CONFIRMED` | Zero whose meaning is supported by cross-field evidence | `Volume`, `Nilai`, `Frekuensi` = 0 together on the 111 no-activity rows (`Volume=0 ⇔ Frekuensi=0` held on all rows) |
| `ZERO_AMBIGUOUS` | Zero present, meaning not confirmed (real zero vs not reported) | `Foreign Sell/Buy`, `Non Regular *`, `Weight For Index` (423 zeros), `Offer/Bid Volume` when a quote exists |
| `NOT_REPORTED` | Source encodes absence as 0 or blank | `Open Price` / `First Trade` = 0 on 238 rows that traded |
| `NOT_APPLICABLE` | Value cannot exist in that circumstance | `Tertinggi`/`Terendah` = 0 on no-activity rows |
| `NO_QUOTE` | No bid/offer price available | `Offer` = 0 (129 rows), `Bid` = 0 (120 rows) |
| `UNAVAILABLE` | Field not in the source at all | sector, board, security type, ISIN, adjusted price, suspension flag, free float |
| `UNKNOWN` | Cannot classify | Anything failing a rule without an explanation |

Session status (separate axis, per security per date):
- Activity: `REGULAR_ACTIVITY_REPORTED` or `NO_REGULAR_ACTIVITY_REPORTED` (IC-50).
- Trading status: `SUSPENDED_EXPLICIT` only when a source states it; otherwise `TRADING_STATUS_UNKNOWN`. **This sample never supports `SUSPENDED_EXPLICIT`** because `Remarks` is uninterpreted.

Downstream rules (also in the logical model, Spec Patch N10): a metric whose required input is in any state other than `OBSERVED` / `OBSERVED_ZERO_CONFIRMED` is undefined for that row; scan rows with such a trigger input are `UNEVALUABLE_REQUIRED_DATA_MISSING`; aggregates exclude those rows and report the count by state; carried-forward closes on no-activity rows are flagged and are not new price observations.

## 6. Cross-field validation (row-level; failure → quarantine row, never repair)

Observed result on the sample in brackets.
- **IC-40** `Selisih = Penutupan − Sebelumnya` [945/945 pass].
- **IC-41** Rows with activity: `Terendah ≤ Penutupan ≤ Tertinggi` and `Tertinggi ≥ Terendah` [0 violations].
- **IC-42** `Volume=0 ⇔ Frekuensi=0` [holds on all 945].
- **IC-43** No-activity rows: `Tertinggi = Terendah = 0` and `Penutupan = Sebelumnya` [111/111].
- **IC-44** `Tradeble Shares ≤ Listed Shares` [0 violations].
- **IC-45 (sanity only, non-authoritative)** For active rows, `Nilai / (Volume × Penutupan)` should be near 1. Sample: median 0.995; 1st–99th percentile 0.94–1.04. It supports a provisional reading (volume in shares, value in IDR), **it does not define the units.** Large departures trigger review flags, not rejection.

## 7. Explicit no-activity status

- **IC-50 (POLICY)** Derive the activity status: `NO_REGULAR_ACTIVITY_REPORTED` when `Volume=0` and `Frekuensi=0` and `Nilai=0`; else `REGULAR_ACTIVITY_REPORTED`. Any partial combination → quarantine.
- **IC-51** `NO_REGULAR_ACTIVITY_REPORTED` is **not** "suspended", "halted" or "illiquid". Trading status is `TRADING_STATUS_UNKNOWN` from this file (the information may be encoded in `Remarks`, which is uninterpreted). Example of why: BHAT has Volume=0 but Non Regular Volume = 1,800,000.
- **IC-52** Such rows count in the observed universe and in coverage; they contribute no return or volume observation for that date beyond the carried-forward flagged close (Doc 09 §6: no-trade ≠ zero volume).

## 8. Regular and non-regular fields

- **IC-53** Keep `Volume/Nilai/Frekuensi` and `Non Regular *` as separate series. Never sum, subtract or net them.
- **IC-54** Observation: `Non Regular Volume` exceeds `Volume` on 22 rows (e.g. AGII, APIC, BACA, BHAT). This is evidence that `Volume` does **not** include non-regular volume in this file. It is an inference; confirm against an IDX definition before any total-activity metric.
- **IC-55** Zero in `Non Regular *` is ambiguous (none vs not reported); flag `ZERO_AMBIGUOUS`.

## 9. Units and price basis

- **IC-60 (POLICY)** Unit status values: `CONFIRMED` (official definition), `PROVISIONAL_CROSS_CHECK` (consistency evidence only), `UNIT_UNKNOWN`. All 28 columns are currently **not CONFIRMED.**
- **IC-61** Importer may store raw values. Metrics that depend on unit semantics stay unapproved (Doc 09 §11) until `CONFIRMED`, or until the owner explicitly accepts `PROVISIONAL_CROSS_CHECK` for a named metric with visible labels.
- **IC-62** Price basis is `UNKNOWN`. No adjustment indicator exists in the file; do not call prices raw or adjusted. No corporate-action information is present, so any return across a split/rights/dividend discontinuity is a quality-flagged window (Doc 09 §6).
- **IC-63** `Remarks` and `Index Individual` and `Weight For Index` are never used in metrics.

## 10. Remarks, Tradeble Shares, Weight

- **IC-64** `Remarks`: observed length 30 on every row and positional variation, no official decoding seen. Store verbatim. Do not interpret any character (no board, suspension, special-notation inference). A future decoding requires an official definition or an owner-approved, documented mapping.
- **IC-65** `Tradeble Shares` is not free float. Equality with `Listed Shares` on 96% of rows shows it cannot serve as a dated free-float denominator (Doc 03 §2.C). Free-float turnover stays unavailable.

## 11. Coverage and missingness

- **IC-70** Coverage record for this batch: `observed_rows=945`, `unique_codes=945`, `rejected_rows=0` (for sample; computed per import), `universe_status = OBSERVED_ONLY`, `verified_market_universe = UNVERIFIED`. No coverage ratio is shown.
- **IC-71** Row presence ≠ field availability. Per-field counts **by value state** (§5A) are part of the coverage report.
- **IC-72** Fields absent from this source stay `UNAVAILABLE`, never zero: sector, board, security type, ISIN, adjusted price, intraday, broker flow, depth, suspension status, free float, trading-calendar flags.
- **IC-73** Instruments excluded by IDX's own selection, if any, are unknowable here. No warrants/ETFs/rights were identified by name in the sample; this is an observation, not a statement of what the page excludes.

## 12. Duplicates, revisions, vintages

- **IC-80** Artifact identity: SHA-256 + source identity. Same bytes imported again → no-op; link a second acquisition record if provenance differs.
- **IC-81** Same trading date, different bytes → new artifact version; a **data vintage** is (source route, trading date, artifact hash). Neither version is assumed correct. Row-level differences are reported, not merged. Supersession requires explicit review.
- **IC-82** Derived features and scans record the vintage set they used (supports Decision C2). Recalculation on a newer vintage creates new results; old results remain.
- **IC-83** A later batch never rewrites an earlier date's records.
- **IC-84** Failed/partial imports are recorded as runs with reasons; accepted rows of a partial import are marked `PARTIAL_IMPORT` and cannot be used for completeness claims.

## 13. Open semantics register (do not guess; resolve from IDX documentation)

| Question | Needed to resolve |
|---|---|
| Units of volume, value, quote volumes, shares fields, foreign fields | Official field definition |
| Meaning/derivation of `Remarks` positions | Official definition |
| `Index Individual`, `Weight For Index` meaning | Official definition |
| Why `Open Price`/`First Trade` = 0 on traded rows | Official definition |
| Does `Volume` exclude non-regular trades? | Official definition |
| Snapshot time of bid/offer; data cut-off/publication time | Official definition |
| Is `Tradeble Shares` any float concept? | Official definition |
| Revision behavior of published daily data | Cross-date sample later (D-F) and IDX documentation |
| Whether the export covers all listed equity instruments | Independent official security master |

If IDX publishes no definitions, these stay `UNKNOWN` and metrics remain provisional with visible labels.

## 14. Conformance checks for a future importer (descriptive; not code)

1. Re-import of the same bytes changes nothing.
2. Header deviation quarantines the file.
3. Row with `Open Price`=0 and Volume>0 yields null open, with the raw value retained.
4. No-activity rows yield `NO_REGULAR_ACTIVITY_REPORTED`, null high/low, flagged carried-forward close, suspension `UNKNOWN`.
5. Non-regular fields remain separate even where they exceed `Volume`.
6. `Tanggal Perdagangan Terakhir` is never used as per-security last-trade date.
7. Unparseable date pattern quarantines.
8. Coverage report shows `OBSERVED_ONLY`, no ratio.
9. Real sample is never present in repository tracking.
