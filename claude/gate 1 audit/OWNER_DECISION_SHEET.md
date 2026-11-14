# MBI — Owner Decision Sheet

**Status:** PROPOSED · **Date:** 14 Nov 2026 · **Author:** Claude (spec author) · **Reviewer next:** Codex
**Nothing here is applied.** Canonical documents are untouched. Reply "approve all" or list the IDs you change.

## Evidence we are standing on

| Item | Status |
|---|---|
| IDX Ringkasan Saham sample (13 Nov 2026, 945 rows, 28 columns) | **SAMPLE_VERIFIED** for file structure and byte integrity (SHA-256 recorded) |
| Complete IDX market coverage | **NOT verified** (945 rows = what this export contained, nothing more) |
| Field meanings and units | **NOT verified** (consistency checks only, no official definition seen) |
| Storage / derived use / redistribution under IDX terms | **NOT cleared** (terms evidence so far is secondary text only) |
| History | Data observed to exist on the page back to 25 Mar 2026. **Not downloaded. No backfill.** |

Impact tags: **ARCH** architecture · **PROD** product behavior · **DATA** data reliability · **PRIV** privacy/legal · **SCOPE** future scope.

---

## D-A — Defer issuer-document and macro sample files until before Sprint 4
**Impact:** SCOPE, DATA
- **Plain meaning:** PDF/Excel filings and BI/BPS tables are read by Sprint 4 code. Nothing before that needs them.
- **Recommend:** **Approve.**
- **If approved:** no more files from you until Sprint 4 prep. Those readers stay marked "unverified". Sprints 0–3 are unaffected.
- **If rejected:** you gather 3–4 extra files now with no benefit until Sprint 4.
- **Caveat:** before Sprint 4 we still need evidence of each source's route and terms.

## D-B — Approve logical-model corrections C1–C5
**Impact:** ARCH
- **C1 — One name for "what did this batch/scan/run actually cover".** Today four names exist for the same idea. Use `universe_coverage_snapshot` with an explicit scope (batch, discovery run, setup scan).
- **C2 — Identity of a setup occurrence ≠ which scan found it.** One occurrence = one security, one moment, one exact rule version. Each scan references it. A separate label records exactly which input-data version was used, so re-running on revised data stays traceable (old result kept, new result added).
- **C3 — Every security in every scan gets exactly one outcome:** excluded, could-not-evaluate (missing data), evaluated-no-match, or evaluated-match. All kept, not just matches.
- **C4 — Discretionary playbooks need no linked rule version.** The link is optional.
- **C5 — Portfolio stays manual-first** with local account aliases. Broker imports/integrations deferred.
- **Recommend:** **Approve all five.**
- **If approved:** Docs 12, 14, 17, 18, ADR-003 get aligned wording (see patch proposal). Scans store more rows (non-matches), which is why large matrices live in compressed files, not the main database.
- **If rejected/deferred:** contradictions stay; reviewers re-flag them; the database design inherits them later and fixing it then costs more.

## D-C — Add a history-sufficiency gate before any real-data research claim
**Impact:** DATA, PROD
- **Plain meaning:** a rule that says "not enough days of data yet" and blocks conclusions. Thresholds are in `GATE_STRUCTURE_PROPOSAL.md` (my proposed defaults, not measured facts; each study may demand more).
- **Recommend:** **Approve.**
- **If approved:** features and studies show `INSUFFICIENT_DATA` until their declared minimum is met. Early screens may look empty, by design.
- **If rejected:** a handful of days could produce confident-looking but meaningless patterns.

## D-D — Backtesting waits for sufficient point-in-time data
**Impact:** DATA, SCOPE
- **Plain meaning:** no backtest or "this setup works" claim until the D-C gate is passed and data is point-in-time (what was knowable then).
- **Recommend:** **Approve.**
- **If approved:** Sprint 12 (setup/parameter learning) real-data exit cannot be met early. Until then it can be tested only with clearly labelled synthetic fixtures (checks the math, proves nothing about markets).
- **If rejected:** risk of backtests on stale/revised/adjusted data that look good only in hindsight.

## D-E — External LLM processing of personal notes, rants, journals is OFF by default
**Impact:** PRIV
- **Plain meaning:** your private writing is never sent to an outside AI unless you explicitly allow that specific use.
- **Recommend:** **Approve.** Default OFF; consent per use (what is sent, to whom), logged locally.
- **If approved:** note-to-research extraction works only through manual/local methods by default (already supported by Doc 07 §6).
- **If rejected:** default-on would send personal content out of your machine.

## D-F — No historical backfill now; revisit a small distributed cross-date sample after the importer passes initial tests
**Impact:** SCOPE, DATA
- **Plain meaning:** we do not download the ~150 daily files from 25 Mar 2026. Later, a handful of well-spread dates tests whether the file layout stays stable.
- **Recommend:** **Approve.** Proposed later sample: about 5 dates spread across the available range, including at least one month-end and one very low-activity day. Automating the downloads with a script stays prohibited.
- **If approved:** history starts accumulating from importer day one; backtests wait (D-D).
- **If rejected (backfill now):** about 150 manual downloads before we know the importer works; layout changes would waste them.

## D-G — Split "observed universe" readiness from "verified market universe" readiness
**Impact:** ARCH, PROD, DATA
- **Plain meaning:** "945 stocks we received" is not "all IDX stocks". Observed = what files contain. Verified = proven against an independent official list we do not have yet.
- **Recommend:** **Approve.** The importer may be built for observed records. Every screen and report says "observed universe" until verification exists. Raw numbers are stored; unit-dependent metrics stay unapproved until field definitions are confirmed.
- **If approved:** development is not blocked by the missing official master list. No coverage-percentage claim is shown.
- **If rejected:** the importer waits for a source we have not found; development stalls.

## D-H (added) — Record an intended-use statement for IDX data
**Impact:** PRIV, DATA, SCOPE
- **Plain meaning:** our reading of IDX terms is secondhand and does not clearly cover long-term storage or derived data. Existing requirements already say "personal, local, non-commercial". Make that explicit.
- **Recommend:** **Approve this statement:** personal non-commercial research; local-only storage; raw files and derived datasets never committed to Git, shared or published; any citation carries source and access date; any change of use requires written permission from IDX first. A primary-source read of the current terms stays an open task.
- **If approved:** importer development on the sample is allowed under this limit; sharing/exports of data are blocked.
- **If rejected:** we cannot state what the data may be used for, and Gate B stays open longer.

---

## Summary

| Category | Items |
|---|---|
| **Needs your approval** | D-A, D-B (C1–C5), D-C, D-D, D-E, D-F, D-G, D-H |
| **Already determined by existing requirements/your instructions** | Rp0 budget; batch-first; no IDX scraping or crawling; no automated browser downloads; selected-issuer intraday only; no TradingView/BIONS/personal-data dependency; Gameplay A illustrative only (C6 is only a wording fix); no backfill now; Claude = primary developer/spec author, Codex = independent reviewer, ChatGPT = owner's advisor |
| **Unresolved blockers** | (1) Independent official security master (verified universe only); (2) primary-source reading of IDX terms; (3) official IDX definitions for units, `Remarks`, `Weight For Index`, `Tradeble Shares`, non-regular fields; (4) corporate-action source; (5) trading-calendar source; (6) Codex review of this package |
| **Not blockers** | Sprint 0; importer development on observed records |
| **Recommended next action** | You approve or edit D-A…D-H → Codex reviews the five documents → Claude applies the spec patch as one docs-only ticket → you approve Sprint 0 |
