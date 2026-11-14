# MBI Gate -1 Proposal Review Findings

## F-01 — BLOCKER: scan membership is not unique for multi-definition scans

**Evidence:** `SPEC_PATCH_PROPOSAL.md:12-16` defines setup-instance identity as setup definition version + security + evaluation timestamp, but defines scan membership as `scan run + security + evaluation timestamp`. `SPEC_PATCH_PROPOSAL.md:18-23` then requires exactly one state per security per scan. The canonical model says a scan has a setup/parameter version (`docs/14_LOGICAL_DATA_MODEL.md:89-94`) and that the key candidate for a setup instance includes setup-definition version (`docs/14_LOGICAL_DATA_MODEL.md:172-182`).

**Impact:** A single scan that intentionally evaluates setup definitions A and B for the same security and timestamp cannot store two independent results. One definition can overwrite, collapse into, or be confused with the other. It also makes matched/non-matched counts and re-runs non-reproducible.

**Recommended correction:** Define an explicit setup-evaluation context. At minimum, the scan-membership identity must include `scan_run_id + setup_definition_version_id` (or parameter-variant/evaluation-definition ID) + security identity + evaluation timestamp, with the relevant data-vintage/feature-snapshot reference. If a scan is contractually restricted to exactly one definition, state and enforce that restriction instead; do not imply that one scan can contain multiple definitions while using the coarser key. Update C2/C3, the acceptance criteria, and the QA case for two definitions in one scan.

## F-02 — MAJOR: data vintage is traceable only indirectly, not part of evaluation identity

**Evidence:** `SPEC_PATCH_PROPOSAL.md:15-16` records a vintage on the scan run and feature snapshot, and `IDX_SAMPLE_DATA_CONTRACT.md:119-125` defines revised bytes as new artifact versions and new derived results. But the proposed scan-membership identity in `SPEC_PATCH_PROPOSAL.md:14` omits vintage, and the setup-instance identity in `SPEC_PATCH_PROPOSAL.md:13` also omits it.

**Impact:** A foreign key to a scan run may provide an indirect trail, but the proposed uniqueness statement does not establish which vintage a particular evaluation used. The same setup/security/time can be recalculated on revised input, and the model needs an unambiguous result identity and feature provenance for both results. Feature provenance also needs the exact dependency set, not merely a broad vintage label, when a feature combines multiple source dates/artifacts.

**Recommended correction:** Make the evaluation record reference an immutable evaluation context containing setup-definition/variant, input-as-of, vintage-set ID, normalization-map version, price/corporate-action basis, and feature-snapshot ID. Define whether vintage is part of the physical uniqueness key or is uniquely reached through that immutable context. Require each derived feature to retain source observation/artifact lineage and dependency vintages. Add QA cases for same setup/security/time under two revised artifact vintages.

## F-03 — MAJOR: zero, missing, and no-trade semantics stop at the sample contract

**Evidence:** The contract carefully distinguishes source-encoded zero, ambiguous zero, unavailable fields, and no regular activity (`IDX_SAMPLE_DATA_CONTRACT.md:52-74`, `:88-98`, `:112-125`). It explicitly says no-activity is not suspension and that carried-forward close is flagged (`:90-92`). The proposed four scan states are only `EXCLUDED_INELIGIBLE`, `UNEVALUABLE_REQUIRED_DATA_MISSING`, `EVALUATED_NOT_MATCHED`, and `EVALUATED_MATCHED` (`SPEC_PATCH_PROPOSAL.md:18-23`).

**Impact:** A four-state scan outcome can be acceptable as a top-level evaluation result, but as written it does not require preservation of the underlying quality/status reasons. A no-activity row, an unavailable field, an ambiguous zero, insufficient history, and a source-encoded “not reported” zero could be collapsed into the same “missing” bucket. That would violate the established rule that missing is unknown/null, no-trade is distinct from zero volume, and optional missing context must not erase a valid trigger.

**Recommended correction:** Keep the four mutually exclusive top-level evaluation states only as outcome classification, and require a separate, typed input-quality/status layer on each evaluation/feature dependency. It must preserve `UNAVAILABLE`, `NOT_REPORTED`, `ZERO_AMBIGUOUS`, `NOT_APPLICABLE_NO_ACTIVITY`, `CLOSE_CARRIED_FORWARD`, `NO_REGULAR_ACTIVITY_REPORTED`, `INSUFFICIENT_DATA`, and source/raw-value lineage where applicable. State explicitly that optional missing context does not change a technical evaluation state, while required missingness does.

## F-04 — BLOCKER: D-H implies terms permission that has not been established

**Evidence:** `OWNER_DECISION_SHEET.md:74-79` calls D-H an intended-use statement and correctly says primary-source terms remain open, but then says “importer development on the sample is allowed under this limit” and that rejecting the statement leaves Gate B open. `GATE_STRUCTURE_PROPOSAL.md:12-15,21-35` otherwise correctly separates sample readiness from terms readiness and requires primary-source evidence for Gate B. `docs/02_DATA_SOURCES_AND_INGESTION.md:170-172` says visibility is not permission and unknown terms require user-triggered import or a disabled adapter.

**Impact:** The package could be read as granting legal clearance based on an internal policy. That is not an acceptable inference from secondary evidence. It also contradicts the otherwise correct distinction between local development on fixtures and importing/deriving from real IDX data.

**Recommended correction:** Rename D-H to an internal handling restriction, not an intended-use permission. Replace “allowed” with “the project will not intentionally share/publish/commit data and will defer any use not clearly permitted.” State that this does not interpret or override IDX terms and does not pass Gate B. Separate fixture-only development from real-sample storage/derivation; real-data intake remains blocked until the primary terms review is recorded and restrictions are explicit.

## F-05 — MODERATE: history thresholds are labelled provisional, but later wording makes them look validated

**Evidence:** `OWNER_DECISION_SHEET.md:39-44` and `GATE_STRUCTURE_PROPOSAL.md:49-51` explicitly call the thresholds proposed, judgment-based starting values, and not measured facts. However, `GATE_STRUCTURE_PROPOSAL.md:51` calls them a floor that a study may never go below, and `:60` calls the 250-session condition a “validated floor.” `SPEC_PATCH_PROPOSAL.md:54` also describes the gate as a required minimum.

**Impact:** Readers may mistake policy defaults for statistically validated sufficiency requirements. The approximate six-month/one-year interpretations also depend on a trading-calendar source that the package says is not yet verified.

**Recommended correction:** Use terms such as `PROVISIONAL_POLICY_DEFAULT` or `UNVALIDATED_RESEARCH_FLOOR`. State that thresholds are conservative governance defaults pending empirical/design review, are not claims of statistical power, and may be raised or replaced by a pre-registered study plan. Keep the no-claim gate, but do not call any value validated until evidence exists. Label calendar approximations as estimates until the calendar source is verified.

## F-06 — MODERATE: Gate A-D separation is mostly correct but D-H leaks into Gate B

**Evidence:** `GATE_STRUCTURE_PROPOSAL.md:8-17` cleanly separates sample/import readiness, terms readiness, observed versus verified universe readiness, and historical sufficiency. The same document then says importer development is allowed after A plus D-H while primary terms review is only “recommended” (`:21-27`).

**Impact:** The gate names are clear, but the permission matrix undermines the separation: an internal owner statement is allowed to substitute for terms readiness for real sample work.

**Recommended correction:** Keep Gate A for structural/contract readiness and Gate B for legal/terms readiness. Permit only documentation and synthetic-fixture work before B. If local handling of an already supplied real sample is intentionally allowed, describe it as a separately approved, terms-uncleared exception with no redistribution/derived-use claim—not as Gate B satisfaction.

## F-07 — MINOR: Sprint 0 is bounded foundation work, but its authorization boundary should be sharper

**Evidence:** `SPRINT_0_PROPOSAL.md:4,8,19-35` excludes market data, parsers, schema, SQL, network libraries, scanners, backtests, UI, and historical backfill. Its tasks are repository hygiene, configuration templates, test/linter setup, local directory conventions, and documentation. `:10-17` requires owner approval, review, P0-docs acceptance, and an environment check.

**Impact:** The scope is appropriately foundation-only, but package installation/version pinning and creation of local data directories are still implementation actions. Under the current Gate -1 authorization they must not start merely because this proposal says “allowed on approval.”

**Recommended correction:** Add an explicit sentence: “This proposal is not authorization; no Sprint 0 command runs until the owner separately approves Sprint 0 after the Gate -1 review and any P0-docs corrections.” Keep local directories empty and outside the repository; do not inspect or move the real sample as part of the current review.

## F-08 — MINOR: existing canonical vocabulary is not fully reconciled by the proposed C3 replacement

**Evidence:** The proposal says “scanned” and “evaluated” counts are dropped (`SPEC_PATCH_PROPOSAL.md:18-23`), while current canonical QA still requires eligible, trigger-evaluable, matched, non-matched, excluded and unavailable counts (`docs/12_QA_TEST_MATRIX.md:82-90`) and the logical model still distinguishes trigger evaluability from optional-context completeness (`docs/14_LOGICAL_DATA_MODEL.md:146-152`).

**Impact:** The proposed four-state vocabulary can be a sound simplification only if the lost distinctions are explicitly retained as coverage dimensions or reason codes. Otherwise the patch removes information that the canonical requirements use to prevent optional missingness from erasing a trigger.

**Recommended correction:** Map the old terms to the new top-level states and preserve trigger-data availability, optional-context completeness, and no-activity/data-quality dimensions as separate dimensions. Do not simply delete the old concepts without a replacement mapping and updated QA acceptance criteria.

## Confirmed alignments

- Rp0 and batch-first acquisition are preserved (`OWNER_DECISION_SHEET.md:87-90`; `SPEC_PATCH_PROPOSAL.md:78-80`).
- No scraping/crawling or automated browser downloads are introduced; the contract uses a user-triggered official batch and keeps terms unresolved.
- Intraday remains selected-issuer/event research only; no TradingView export, BIONS import, or personal portfolio dependency is added.
- Gameplay A is explicitly illustrative and not a Gate -1 blocker (`SPEC_PATCH_PROPOSAL.md:38`; canonical `docs/18_SETUP_INSTANCE_AND_PARAMETER_LEARNING.md:37-39`).
- No historical backfill is proposed now, and the five-date stability sample remains deferred and manual-only (`OWNER_DECISION_SHEET.md:60-65`).
- The sample is correctly limited to 945 observed rows, with no completeness ratio or whole-market claim (`OWNER_DECISION_SHEET.md:10-14`; `IDX_SAMPLE_DATA_CONTRACT.md:112-117`).
- Sprint 0 contains no market-data implementation and should remain separately authorized (`SPRINT_0_PROPOSAL.md:33-46`).

