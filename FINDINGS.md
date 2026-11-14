# MBI Gate -1 Proposal Review 2 Findings

## Original findings status

| Original finding | Status in revision 2 | Determination |
|---|---|---|
| F-01 scan membership not unique across multiple definitions | **RESOLVED** | The revised model correctly makes membership a shared denominator declaration and puts setup version in the evaluation record. See new F-09 for the remaining denominator arithmetic gap. |
| F-02 vintage not explicit in evaluation identity/provenance | **RESOLVED, with deferred implementation decision** | The five-part natural key, immutable vintage, cutoff, and lineage requirements are now explicit. The concrete vintage-selection rule remains open and blocks implementation, not this proposal-level identity. See new F-12. |
| F-03 zero/missing/no-trade semantics not carried into the model | **PARTIALLY RESOLVED** | The value-state and session-status axes are now carried into the model, but `OBSERVED_ZERO_CONFIRMED` overstates what the sample proves. See F-10. |
| F-04 D-H implied IDX permission | **RESOLVED as to D-H wording** | D-H now expressly disclaims legal permission and leaves Gate B open. A separate Gate B bypass exception remains unsafe; see F-13. |
| F-05 thresholds insufficiently provisional | **RESOLVED** | Every appearance of 120, 250, 30 and 60 in the revised proposals is labelled provisional, non-validated, non-universal, and subject to a study-specific plan. |
| F-06 Gate A-D separation leaked D-H into Gate B | **PARTIALLY RESOLVED** | Gate B is clearly OPEN and real-data processing normally waits for B, but an early owner-risk-acceptance route remains. See F-13. |
| F-07 Sprint 0 authorization boundary too weak | **RESOLVED** | Sprint 0 is explicitly proposed/not started/not authorized, touches no market data, and requires separate owner approval. |
| F-08 C3 did not reconcile canonical coverage vocabulary | **PARTIALLY RESOLVED** | N10 and revised C3 preserve reason/value-state detail, but independent scan/setup denominator semantics and final canonical-document alignment remain incomplete. See F-09. |

## F-09 — MAJOR: shared membership and per-setup denominator arithmetic is underspecified

**Evidence:** `SPEC_PATCH_PROPOSAL.md:15-16` defines one membership per scan/security/time and one evaluation per setup definition. `:29` then says, “Counts per scan and setup version,” while also defining `in-scope` as the number of `IN_SCOPE` memberships and as the sum of the four evaluation states. `REVISION_2_LOG_AND_APPROVAL_MATRIX.md:12-18` claims the split is complete but does not define the aggregation relationship.

**Impact:** With two setup definitions in one scan, one membership is shared while two evaluation records exist. A scan-level membership count is not the same population as the sum of setup-specific evaluation states. The proposal does not clearly define whether coverage is reported as (a) one scan-level denominator plus one result distribution per setup, (b) one setup-specific coverage snapshot per definition, or (c) a combined matrix. It also does not state how a scan-level `EXCLUDED_FROM_SCOPE` membership relates to a setup-level `EXCLUDED_INELIGIBLE` evaluation.

**Required correction:** Define separate aggregation grains explicitly:

- scan-level membership coverage: one row per scan/security/time;
- setup-specific evaluation coverage: one row per scan/setup-definition/security/time for every in-scope membership;
- optional combined report: never sum setup-specific denominators and call that scan coverage.

State the exact join and formulas for each grain, including whether setup-specific exclusions receive evaluation records. Add a QA case with two definitions, one shared membership, different outcomes, and independently reproducible counts for each definition.

## F-10 — BLOCKER: `OBSERVED_ZERO_CONFIRMED` conflates observation, inference, and official semantics

**Evidence:** `IDX_SAMPLE_DATA_CONTRACT.md:5` says no official field dictionary has been seen. `:81-92` defines `OBSERVED_ZERO_CONFIRMED` as a zero whose meaning is supported by cross-field evidence, and uses the joint zero pattern for Volume/Nilai/Frekuensi as its example. `:105-108` reports only sample cross-field checks. `:124` says all 28 columns are not officially confirmed. `SPEC_PATCH_PROPOSAL.md:38-40` carries the same “confirmed” state into the logical model and permits it as an acceptable metric input.

**Impact:** The sample establishes an observed numeric zero. It supports an inference that the three fields jointly indicate `NO_REGULAR_ACTIVITY_REPORTED` under a project policy. It does not officially confirm that zero means no trades, nor does it establish the source’s business semantics, units, or exclusion rules. Calling it “confirmed” can authorize metrics as if the source definition were known, contrary to the open-semantics register.

**Required correction:** Separate three axes:

1. raw observation: `OBSERVED_ZERO`;
2. project-derived status: `NO_REGULAR_ACTIVITY_REPORTED`, with `inference_basis=CROSS_FIELD_PATTERN` and an explicit non-official evidence class;
3. source semantic status: `OFFICIALLY_CONFIRMED` only when primary documentation supports it, otherwise `UNKNOWN`/`UNVERIFIED`.

Reserve `CONFIRMED` for official semantic confirmation. Keep Open Price/First Trade as `NOT_REPORTED` only as a documented project interpretation, with raw zero retained and official meaning unresolved. Keep `NO_QUOTE`, `NOT_APPLICABLE`, `UNAVAILABLE`, `SUSPENDED_EXPLICIT`, and `TRADING_STATUS_UNKNOWN` distinct.

## F-11 — MODERATE: “rule version” is not explicitly canonicalized

**Evidence:** `OWNER_DECISION_SHEET.md:31-32` and `REVISION_2_LOG_AND_APPROVAL_MATRIX.md:12` use “rule version.” `SPEC_PATCH_PROPOSAL.md:14-16,51-52` uses `setup_definition_version_id`. The documents do not explicitly state whether “rule version” is an exact synonym for `setup_definition_version_id`, or whether it could mean a setup family, playbook, parameter variant, detector, or experiment.

**Impact:** Occurrence identity and evaluation identity could diverge across documents or later implementation. A parameter variant may be a research configuration rather than an immutable setup definition, so silent interchange would compromise reproducibility.

**Required correction:** Add one canonical vocabulary rule: use `setup_definition_version_id` for the immutable runnable rule; identify parameter variants, experiments, detectors, playbooks, and families separately; use “rule version” only as a plain-language alias if explicitly defined.

## F-12 — MODERATE: vintage-selection rule remains open

**Evidence:** `SPEC_PATCH_PROPOSAL.md:31-35` requires a stated selection rule and cutoff and gives “latest artifact per date retrieved on or before cutoff T” only as an example. `REVISION_2_LOG_AND_APPROVAL_MATRIX.md:53-60` expressly lists the default vintage-selection rule as unresolved.

**Impact:** The identity and immutability design is sufficient to prevent silent overwrite, but a replay cannot be operationally deterministic until the selection rule, tie handling, missing-date behavior, and relationship between retrieval time and publication time are chosen. This is safe to defer only while no real-data processing or claims are authorized.

**Disposition:** Deferred with justification for Gate -1 proposal review; must be resolved before importer/evaluation implementation or any real-data result.

## F-13 — MAJOR: Gate B has an explicit early-processing exception

**Evidence:** `GATE_STRUCTURE_PROPOSAL.md:13` correctly keeps Gate B OPEN and says local-only handling does not satisfy it. But `:25-27` says real IDX processing waits for B, then allows the owner to proceed earlier through “a recorded owner risk acceptance.” `OWNER_DECISION_SHEET.md:78-80` also says real data waits for B unless the owner separately accepts the risk.

**Impact:** The text no longer implies permission through D-H, but it still creates an operational path to process real IDX data while acquisition, retention, processing, transformation, and derived-use permissions remain unverified. Owner risk acceptance can acknowledge uncertainty; it cannot establish source permission or override the product invariant that terms must be reviewed before an adapter is enabled.

**Required correction:** Remove the early real-data-processing exception. Permit only synthetic-fixture work before B. If the owner intentionally wants a legal-risk decision, record it outside this technical gate as an explicit owner escalation, without describing the activity as permitted and without enabling implementation under the current Gate -1 authorization.

## Confirmed alignments

- Rp0 and batch-first acquisition remain intact.
- No IDX scraping/crawling or automated browser download is introduced.
- Intraday remains selected-issuer/event-only.
- TradingView export, BIONS, credentials, and personal portfolio exports remain excluded.
- External LLM processing of personal notes remains off by default.
- Gameplay A remains illustrative only.
- No historical backfill is proposed now.
- The 945-row sample remains `OBSERVED_ONLY`; no complete-market claim is made.
- Sprint 0 remains foundation-only and not authorized.

## Newly discovered issues — concise list

1. Shared membership versus per-setup evaluation denominators are not mathematically specified (F-09).
2. `OBSERVED_ZERO_CONFIRMED` overstates cross-field inference as confirmed source semantics (F-10).
3. “Rule version” is not explicitly mapped to `setup_definition_version_id` (F-11).
4. Vintage selection is described but not decided (F-12).
5. Gate B retains an early real-data risk-acceptance bypass (F-13).

