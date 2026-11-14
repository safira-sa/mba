# MBI — Owner Decision Sheet

**Status:** PROPOSED (revision 2, after Codex review; **no decision is approved**) · **Date:** 14 Nov 2026 · **Author:** Claude (spec author) · **Reviewer next:** Codex
**Nothing here is applied.** Canonical documents are untouched. Reply "approve all" or list the IDs you change.

## Evidence we are standing on

| Item | Status |
|---|---|
| IDX Ringkasan Saham sample (13 Nov 2026, 945 rows, 28 columns) | **SAMPLE_VERIFIED** for file structure and byte integrity (SHA-256 recorded) |
| Complete IDX market coverage | **NOT verified** (945 rows = what this export contained, nothing more) |
| Field meanings and units | **NOT verified** (consistency checks only, no official definition seen) |
| Downloading, retaining, processing, transforming, deriving from, or redistributing IDX data under IDX terms | **NOT cleared.** Terms evidence is secondary text only. Gate B is OPEN. The structural inspection of your file was a read-only check and is not evidence of permission |
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
- **C2 — Keep four things separate (revised).** (1) *Occurrence*: one rule version + one security + one market time. (2) *Scan membership*: "this security-time was in this scan's list". (3) *Evaluation record*: the result of testing one rule version on one security-time in one scan, tied to the exact data version used. (4) *Data version* ("vintage"): the exact set of input files and settings. If the same occurrence is checked again on revised data, a new evaluation record is added; the old one stays, unchanged and reproducible. Nothing is overwritten.
- **C3 — Every evaluation record has exactly one outcome (revised):** excluded by the rule, could-not-evaluate (a needed value was missing or unreliable), evaluated-no-match, or evaluated-match. All are kept, not just matches. Several rules in one scan each get their own records, so nothing collides.
- **C4 — Discretionary playbooks need no linked rule version.** The link is optional.
- **C5 — Portfolio stays manual-first** with local account aliases. Broker imports/integrations deferred.
- **Recommend:** **Approve all five.** *C2 and C3 wording changed in this revision; please read them again before approving.*
- **If approved:** Docs 09, 12, 14, 17, 18, ADR-003 get aligned wording (see patch proposal). This includes the new data-version rule and the value-state vocabulary (zero vs missing vs no-trade). Scans store more rows (non-matches), which is why large matrices live in compressed files, not the main database.
- **If rejected/deferred:** contradictions stay; reviewers re-flag them; the database design inherits them later and fixing it then costs more.

## D-C — Add a history-sufficiency gate before any real-data research claim
**Impact:** DATA, PROD
- **Plain meaning:** a rule that says "not enough history yet" and blocks conclusions. How much is enough depends on the feature, the time window, data quality, whether the data was point-in-time, and what the study is trying to show. The numbers in `GATE_STRUCTURE_PROPOSAL.md` (120 and 250 sessions, plus 30 and 60 for event counts and holdout) are **provisional judgment defaults. They are not validated thresholds and not universal minimums.** They are placeholders until you approve or replace them, and every study must declare and justify its own plan.
- **Recommend:** **Approve the gate mechanism.** Adopt the numbers only as labelled placeholders (`PROVISIONAL_DEFAULT`), or give your own.
- **If approved:** features and studies show `INSUFFICIENT_DATA` until their declared minimum is met. Early screens may look empty, by design. Any display using a placeholder says so.
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
- **Recommend:** **Approve.** The importer may be designed and tested for observed records, using synthetic test files built from the contract's structure. Every screen and report says "observed universe" until verification exists. Processing real IDX files waits for Gate B (see D-H).
- **If approved:** development is not blocked by the missing official master list. No coverage-percentage claim is shown.
- **If rejected:** importer design waits for a source we have not found; development stalls.

## D-H (added, rewritten) — Internal data-handling policy for IDX data
**Impact:** PRIV, DATA, SCOPE
- **What it is:** your own internal rule for how you handle the data on your machine. **It is not legal advice. It is not confirmation of IDX permission. It does not establish permission to download, retain, process, transform or derive from IDX data. Local-only use does not make any acquisition or processing route permissible.**
- **Proposed policy text:** handle IDX data for personal research only; keep it on your own machine; never commit raw or derived IDX data to Git; do not share, export or publish it; cite source and access date when you quote anything; if you ever want to change this use, stop and get IDX's written position first.
- **What stays open:** **Gate B remains OPEN** until the applicable current IDX terms are read from primary sources and the permission for each activity (acquire, retain, process, transform, derive) is independently verified, or that activity is marked restricted.
- **Recommend:** **Approve the policy text as internal policy only.** Approving it changes nothing about Gate B.
- **If approved:** the policy is written into the project README. The importer is developed on synthetic fixtures. Running it on the real IDX file or real batches waits for Gate B, unless you separately record that you accept the risk yourself (your risk, not a permission).
- **If rejected/deferred:** no recorded handling policy; Gate B is unaffected and stays open either way.

---

## Summary

| Category | Items |
|---|---|
| **Needs your approval** | D-A, D-B (C1–C5), D-C, D-D, D-E, D-F, D-G, D-H |
| **Already determined by existing requirements/your instructions** | Rp0 budget; batch-first; no IDX scraping or crawling; no automated browser downloads; selected-issuer intraday only; no TradingView/BIONS/personal-data dependency; Gameplay A illustrative only (C6 is only a wording fix); no backfill now; Claude = primary developer/spec author, Codex = independent reviewer, ChatGPT = owner's advisor |
| **Unresolved blockers** | (1) Independent official security master (verified universe only); (2) primary-source reading of IDX terms and verification of permissions (Gate B); (3) official IDX definitions for units, `Remarks`, `Weight For Index`, `Tradeble Shares`, non-regular fields; (4) corporate-action source; (5) trading-calendar source; (6) Codex review of this package |
| **Not blockers** | Sprint 0; importer development on observed records |
| **Recommended next action** | You approve or edit D-A…D-H → Codex reviews the five documents → Claude applies the spec patch as one docs-only ticket → you approve Sprint 0 |
