# MBI — Spec Patch Proposal

**Status:** PROPOSED, nothing applied · **Date:** 14 Nov 2026 · **Author:** Claude · **Reviewer next:** Codex
**Source findings:** `GATE_MINUS_1_AUDIT_REPORT.md` (C1–C6, D1–D14), plus new items N1–N8 from the sample inspection and owner instructions.
**Numbering note:** the audit report has **no D10 or D11** (numbering gap in the report). No finding is missing; the rows are recorded below as "unassigned" for traceability.
**Delivery form:** one docs-only ticket "P0-docs" (no code), applied by Claude after owner approval, then reviewed by Codex. Section references use the package's own headings (no line numbers available).

## 0. Canonical definitions (referenced by the patches)

**Coverage snapshot (C1).** One concept: `universe_coverage_snapshot`, with `scope ∈ {ACQUISITION_BATCH, DISCOVERY_RUN, SETUP_SCAN}`. Holds intended / eligible / observed / evaluated / excluded / unavailable counts with reasons, plus `universe_status ∈ {OBSERVED_ONLY, VERIFIED_AGAINST_MASTER}` and the vintage set used. Replaces `discovery_universe_snapshot`, `discovery_coverage_snapshot`, `setup_scan_coverage` (kept as historical aliases in Doc 14 §A–§I).

**Setup occurrence identity and vintage (C2).**
- *Setup instance* identity = setup definition version + security identity + evaluation timestamp. No scan or experiment context in the identity.
- *Scan membership* = scan run + security + evaluation timestamp → one evaluation state (below), with a reference to the setup instance when matched.
- *Data vintage identity* = set of (source route, trading date, artifact hash) + normalization-map version + price-basis/corporate-action basis. Recorded on the scan run and on each feature snapshot.
- Recalculation on revised input creates a new scan run and new feature snapshot on a new vintage. The earlier membership, snapshot and outcome stay. The same instance may be MATCHED in one vintage and NOT_MATCHED in another; both are visible.

**Evaluation state (C3).** Exactly one per security per scan, evaluated in this precedence:
1. `EXCLUDED_INELIGIBLE` (+ reason code: outside declared universe, security type, listing age, etc.)
2. `UNEVALUABLE_REQUIRED_DATA_MISSING` (+ reason: which trigger input is missing or insufficient history)
3. `EVALUATED_NOT_MATCHED`
4. `EVALUATED_MATCHED`
All counts derive from these: in-scope = sum of the four; eligible = in-scope − (1); evaluable = eligible − (2); matched = (4). "Scanned" and "evaluated" as separate counts are dropped. All four kinds of row are retained (large matrices in versioned Parquet per Doc 18 §4).

**Universe status vocabulary (D-G).** `OBSERVED_ONLY` (rows actually present in accepted artifacts) vs `VERIFIED_AGAINST_MASTER` (reconciled with an independent official security master). No coverage percentage until the latter exists.

---

## 1. Logical-model corrections (C1–C6)

| ID | File / section | Proposed change | Reason | Depends on | Acceptance criteria |
|---|---|---|---|---|---|
| **C1** | Doc 14 §G (`setup_scan_coverage`), §I (`discovery_universe_snapshot`), §A/§B; Doc 17 §12 (`discovery_coverage_snapshot`); Doc 12 §7–§8; Decision Record #6 | Replace the four names with `universe_coverage_snapshot` + `scope`. Add scope `ACQUISITION_BATCH` to Doc 14 §A. Add the status vocabulary above. | Four names for one idea | Owner D-B | `grep` finds no residual old name except in an "aliases/superseded" note; Doc 14 defines scope values; Docs 12/17 use the one name |
| **C2** | Doc 14 §5 (setup instance key), §G; Doc 18 §4 (identity paragraph), §10; Doc 12 §8 | Identity per §0. Introduce scan-membership link. Add data vintage identity to scan run and feature snapshot. Keep `setup_occurrence_group` as is. | Instance identity currently includes scan context; revised data not traceable | C1, C3, Doc 14 §A vintage language | Doc 14 §5 key excludes scan/experiment; vintage definition exists once; Doc 18 §4 references it; QA row exists for "same occurrence re-found by two scans" and "recalc on revised vintage" |
| **C3** | Doc 12 §8 bullet 2; Doc 14 §G (`setup_scan_coverage`), §3; Doc 18 §4, §10.4; Doc 09 §8 | Single state set and precedence per §0. Remove "trigger-evaluable", "evaluated", "scanned" as parallel counts. State that all four row kinds are retained. | Four overlapping vocabularies | C1 | Same four states appear in Docs 09, 12, 14, 18; each count definition derivable; QA row: "each security in exactly one state" |
| **C4** | Doc 14 §G final paragraph; Doc 05 §2/§4 (already consistent) | "A playbook version references setup definition version(s) **where it uses a runnable setup**; the link is optional for discretionary playbooks." | Contradiction with Doc 05 | none | Doc 14 wording matches Doc 05; Doc 18 §2 item 7 unchanged |
| **C5** | Doc 14 §H (`execution_fill` "broker report"), §J (`brokerage_account_ref`, `account_event` "imported"); Doc 08 §2/§6; Doc 06 §4; Doc 12 §10 | Rename to `local_account_alias`. Fill source ∈ `USER_ENTERED` (broker-report import = deferred value, not enabled). `account_event` = user-entered; import fields marked deferred. | Manual-first decision vs import wording | Owner D-B | No Doc 14 text implies broker import is in current scope; naming matches Doc 08 |
| **C6** | Doc 18 §1 ("Example user idea"); ADR-003 Context ("The user wants a gameplay such as…"); `examples/setup_parameter_learning_gameplay_A.yaml` (`origin: USER`, `user_label`, `raw_hypothesis` presented as user wording); Doc 18 §3 and Doc 11 Sprint 12 (already correct); Doc 05/09/12 mentions | Re-attribute to an **illustrative assistant example**: Doc 18 §1 "Illustrative example idea"; ADR-003 "e.g. an illustrative gameplay"; YAML `origin: ASSISTANT_ILLUSTRATION`, drop `user_label`, mark `raw_hypothesis` as assistant-drafted. Keep the `example_status` banner. | Provenance misattribution risks treating it as user strategy | none | No active document or fixture attributes Gameplay A to the user; `grep -i "user" ` hits around Gameplay A are all negations or generic-future references |

## 2. Audit findings D1–D14

| ID | File / section | Proposed change | Reason | Depends on | Acceptance criteria |
|---|---|---|---|---|---|
| **D1** (security master circularity) | Doc 14 §B; Doc 11 Sprint 5; Doc 13 (new row); Decision Record #4 | Define `observed universe` (from batch rows) vs `verified market universe` (needs independent official master). Add "security master" to the source register as `UNVERIFIED` with "route not identified". Sprint 5 builds an *observed* master; reconciliation with an official master is a separate later capability. | Coverage cannot be proven against a master derived from the same batch | Owner D-G | Doc 14 §B and Sprint 5 no longer describe self-comparison; register row exists; UI/report wording rule `OBSERVED_ONLY` stated in Docs 02 §4, 03 §1 |
| **D2** (stale maps, QA overclaim) | `V0.7_SUPERSESSION_MAP.md`, `V0.8_SUPERSESSION_MAP.md`; `QA_STATIC_RESULTS.json` | Move both to `archive/` with ARCHIVE ONLY banner; remove pointers to `CODEX_START_HERE_v0.9.md`; re-run static checks and update results. | Stale instruction sources; QA claim contradicted | none | No root file points to archived v0.9 prompts; QA results regenerated and dated |
| **D3** (role boundary) | `CLAUDE.md`, `AGENTS.md`, `README.md`, Doc 10 §7, `prompts/*`, `CLAUDE_START_HERE_v0.9.2.md` | See **N1** (roles changed by owner). Also: list `CLAUDE_START_HERE_*` in README; define where audit/proposal deliverables may be written (`reports/` outside canonical docs). | Role text contradicts task instructions | N1 | One consistent role table across files |
| **D4** (suspect URL / terms URL) | Doc 13 §1 (Disclosure row `block.idx.id`; Terms URL); Doc 02 §3 | Mark both URLs `UNVERIFIED` until confirmed from IDX's main site; remove the suspect domain from active guidance if it cannot be confirmed. | Unrecognized domain; path differs from a secondary citation | none | Each URL carries "verified on <date> from <route>" or "unverified" |
| **D5** (route observation provenance) | Doc 13 §1 status column; Doc 02 §3; Doc 16 §2 | Record who observed the route, when and how. For IDX Ringkasan Saham: upgrade export status from `UNVERIFIED` to **`SAMPLE_VERIFIED (structure/integrity only; terms, coverage, semantics not verified)`**. | Observation lacked an artifact; sample now exists | Contract doc | Register row cites the SHA-256, the date, and the three "not verified" qualifiers |
| **D6** (manifest) | `MANIFEST.sha256` | Regenerate after P0-docs, including `CLAUDE_START_HERE_v0.9.2.md`, active prompts and new reports. | Two active instructions unhashed | all patches applied | `sha256sum -c` passes; every active instruction file listed |
| **D7** (changelog order) | `CHANGELOG.md` | Newest-first order; add v0.9.3 entry for P0-docs. | Ordering | all patches | Entries descending; v0.9.3 lists changed files |
| **D8** ("Sprint -1") | Doc 10 §1, §6 | Rename to "Gate -1 / environment check in Sprint 0". | Naming | none | No "Sprint -1" remains |
| **D9** (raw sample in repo) | `gate_minus_1_samples/README.md`; `.gitignore` (Sprint 0) | State that real samples live under the local data root, not the repo; add ignore rules (see Sprint 0). Remove the sample folder from the tracked tree. | Redistribution/terms risk; raw-data policy | Owner D-H | `git check-ignore` confirms rule; `git ls-files` shows no `.xlsx` sample |
| **D10, D11** | — | **Unassigned** in the audit report (numbering gap). No action. | Traceability | none | Recorded here |
| **D12** (history sufficiency) | Doc 11 (new gate before Sprint 7 and again Sprint 12/13), Doc 09 §8, Doc 18 §7/§9, Decision Record, README constraints | Add the history-sufficiency gate per `GATE_STRUCTURE_PROPOSAL.md` §4. Metric contracts declare minimum sessions; below it → `INSUFFICIENT_DATA`. Real-data exits for Sprints 7, 8, 12, 13 apply only after the gate; until then labelled synthetic fixtures verify the arithmetic only. | One-day history cannot meet current exit criteria | Owner D-C, D-D | Each affected sprint exit cites the gate; Doc 09 §1 metric contract gains a "minimum history" field; QA row for gate behavior |
| **D13** (corporate actions / raw vs adjusted) | Doc 13 (new row `UNVERIFIED`), Doc 03 §2.B, Doc 09 §2/§3/§6, Doc 18 §3.B, Doc 02 §4 | State: the IDX sample carries no adjustment indicator and no corporate-action information. Price basis = `UNKNOWN`. Windows spanning a possible corporate action are flagged and excluded from return/MA features until a verified corporate-action source exists. | Raw vs adjusted unknown; no source | Contract IC-62 | Docs state the limitation; features requiring continuity list "corporate-action basis" as required input; QA row for flagged window |
| **D14** (LLM default-off) | Doc 07 §6, Doc 10 §9, README constraints, config template (Sprint 0) | Add rule: external LLM disabled by default; no personal note/rant/journal content leaves the machine without explicit per-use consent that names what is sent and to which service; consent logged locally; extraction without LLM remains the baseline. | Silence on external processing | Owner D-E | One rule appears in Docs 07 and 10; config template default `external_llm_enabled = false`; QA row |

## 3. New items from the sample inspection and owner instructions

| ID | File / section | Proposed change | Reason | Depends on | Acceptance criteria |
|---|---|---|---|---|---|
| **N1** (roles) | `AGENTS.md`, `CLAUDE.md`, `README.md` (read order/prompt hygiene), Doc 10 §7, `prompts/`, both START_HERE files, `CLAUDE_REVIEW_PROMPT_*`, `CODEX_AUDIT_PROMPT.txt` | New role table: Claude = primary development agent and specification author; Codex = independent reviewer (read-only on the author's branch); ChatGPT = owner's advisor on architecture, scope and acceptance (no repository authority); owner approves decisions and does not design database internals. Replace "Codex sole builder" and "Claude read-only reviewer". Keep the one-bounded-ticket workflow. Update prompts so the reviewer is Codex. | Owner changed roles | none | No file calls Codex the builder or Claude the read-only reviewer; workflow in Doc 10 §8 unchanged except actor names |
| **N2** (intended use / terms) | Doc 02 §12 (approval gate), Doc 13 §1 (Terms row), Doc 16 §2, new "intended-use statement" in README | Insert the D-H statement. Terms row status: `TERMS_REVIEWED (secondary evidence only)` until a primary read is recorded. | Terms not cleared for storage/derived use | Owner D-H | Statement present in README and Doc 02; register states evidence type |
| **N3** (acquisition provenance) | Doc 02 §4 (batch contract), Doc 14 §A (`acquisition_batch`), Doc 16 §7 | Add: declared scope vs observed scope; owner-declared filters; page-reported total as corroboration only; retrieval time source label; embedded generator metadata; vintage identity. Reference the IDX contract. | Sample showed what provenance matters | Contract §2 | Doc 14 §A lists the fields; Doc 02 §4 cross-references contract |
| **N4** (missing-data policy) | Doc 09 §6; Doc 02 §10 fallback table; Doc 12 §2 | Add rules: **source-encoded zeros** (zero meaning "not reported" or "no quote") become null with reason; **ambiguous zero** is preserved with a flag, never used in metrics; **no-activity** is a status distinct from suspended; carried-forward close is flagged. | Sample showed 238 traded rows with Open=0 | Contract §5–§7 | Doc 09 §6 lists the three rules; QA rows added for each |
| **N5** (field semantics / units) | Doc 09 §5, §11; Doc 03 §2.C | Add unit-status vocabulary `CONFIRMED / PROVISIONAL_CROSS_CHECK / UNIT_UNKNOWN`; cross-check evidence never promotes a unit to CONFIRMED. Metrics requiring semantics stay unapproved until confirmed or owner-approved as provisional with labels. | Units only cross-checked | Contract §9 | Doc 09 §5 contains the vocabulary; example metric contract gains a `unit_status` input line |
| **N6** (observed vs verified universe wording) | Docs 02 §4, 03 §1, 14 §B, 17 §6, 12 §2 | Replace unqualified "universe/coverage" wording with the §0 vocabulary; forbid coverage percentages until `VERIFIED_AGAINST_MASTER`. | 945 rows ≠ market | Owner D-G | No document says or implies "whole IDX" for batch rows; QA row exists |
| **N7** (gate structure) | Decision Record, Doc 11 Gate -1 section, Doc 16 §9, README, CODEX/CLAUDE START files, QA_REVIEW_REPORT | Replace the single Gate -1 with sub-gates A–D (`GATE_STRUCTURE_PROPOSAL.md`). Preserve history of the old gate in the Decision Record. | Gate bundles unrelated readiness | Owner approval | Doc 11 lists A–D with entry/exit; Decision Record cites statuses |
| **N8** (sample handling) | Doc 16 §2/§7, `gate_minus_1_samples/` | Record sample manifest entry (name, SHA-256, size, date, declared selection). State: the raw file is stored locally outside Git, immutable, not re-uploaded unless the hash changes. | Evidence trail | N2, D9 | Manifest entry exists; sample absent from Git |

## 4. Order and review

1. Owner decisions (D-A…D-H).
2. Codex independent review of the five reports.
3. P0-docs applied in this order: N1 and D3 (roles), C1→C3→C2→C4→C5→C6, D1/D13/N5/N6 (source and universe wording), N2/N3/N4/N8/D5/D9, D12/N7 (gates), D14, then D2/D4/D6/D7/D8 (housekeeping and manifest).
4. Codex reviews the diff; owner accepts; Sprint 0 may then be approved.

## 5. Not changed by this proposal

Product invariants, Rp0, batch-first, no scraping/crawling, selected-issuer intraday, Setup/Episode/Discovery concepts, probability safeguards, point-in-time rules.
