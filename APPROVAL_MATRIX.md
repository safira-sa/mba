# MBI Gate -1 Review 2 Approval Matrix

**Verdict:** **REVISE**. Do not approve all decisions as written.

| Decision | Safe to approve now? | Review disposition |
|---|---:|---|
| D-A — defer issuer-document and macro samples | **Yes, conditionally** | Approve the deferral. Keep all routes and terms explicitly `UNVERIFIED`/`DEFERRED`; this grants no source permission. |
| D-B / C1 — canonical `universe_coverage_snapshot` | **Yes, conditionally** | Approve the naming direction and observed-versus-verified distinction, subject to preserving aliases and updating canonical docs in the later docs-only patch. |
| D-B / C2 — occurrence, membership, evaluation, vintage | **No** | The identity split is now directionally correct, but coverage aggregation is not defined for multiple setup definitions, and “rule version” needs canonical mapping. Resolve F-09 and F-11. |
| D-B / C3 — mutually exclusive evaluation states | **No, not as written** | Per-evaluation states are sound, but the `OBSERVED_ZERO_CONFIRMED` input state is semantically overclaimed and denominator formulas need correction. Resolve F-09/F-10. |
| D-B / C4 — discretionary playbook link optional | **Yes** | Consistent with the runnable-setup versus discretionary-playbook distinction. |
| D-B / C5 — manual-first local account aliases | **Yes** | Consistent with privacy and portfolio invariants; broker imports remain deferred. |
| D-C — history-sufficiency gate | **Yes, conditionally** | Approve the mechanism and `INSUFFICIENT_DATA` behavior. 120/250/30/60 may remain only as `PROVISIONAL_DEFAULT` references, not validated requirements. |
| D-D — no backtesting/“works” claims before point-in-time sufficiency | **Yes** | Consistent with look-ahead and unsupported-claim safeguards. |
| D-E — external LLM processing off by default | **Yes** | Consistent with local-first privacy and explicit per-use consent. |
| D-F — no historical backfill now | **Yes** | Consistent with the approved operating strategy; later cross-date sampling remains manual and Gate B-dependent. |
| D-G — observed versus verified universe | **Yes** | Consistent with the 945-row evidence ceiling; no coverage percentage or whole-market wording before independent-master reconciliation. |
| D-H — internal IDX data-handling policy | **Yes, as internal policy only** | The rewritten policy is not legal clearance and does not pass Gate B. It must not be used to authorize real-data processing. |
| Gate A — sample/import-contract readiness | **Yes, conditionally** | The contract is usable as a schema-agnostic proposal if `OBSERVED_ZERO_CONFIRMED` is replaced with an observation/inference/source-confirmation separation. No real-data processing authorization follows. |
| Gate B — source-terms and permission readiness | **No** | Correctly OPEN; primary terms and per-activity permissions remain unresolved. Remove the early-processing exception. |
| Gate C1 — observed-universe readiness | **Yes, conditionally** | Approve observed-scope terminology only, with no completeness claim and independent coverage arithmetic. |
| Gate C2 — verified-universe readiness | **No** | Correctly blocked pending an independent official security master and reconciliation. |
| Gate D — historical sufficiency | **Yes, conditionally** | Approve the gate as a governance mechanism, with all numeric references provisional and study-specific plans controlling. |
| Sprint 0 proposal | **No authorization from this review** | Foundation-only and not started, but still requires separate owner approval after the Gate -1 review. |

## Original blocker verdicts

- Scan membership uniqueness: **resolved at identity level; remaining denominator defect blocks approval**.
- Vintage identity/provenance: **resolved at proposal level; selection rule deferred until implementation**.
- D-H permission implication: **resolved in D-H wording; Gate B bypass remains unresolved**.
- Zero/missing/no-trade representation: **partially resolved; “confirmed zero” must be corrected**.
- Provisional thresholds: **resolved**.

## Required corrections before APPROVE

1. Define scan-level and setup-level coverage grains and formulas; do not equate shared membership counts with summed evaluation counts.
2. Replace `OBSERVED_ZERO_CONFIRMED` with separate raw observation, inferred session status, and official semantic-confirmation status.
3. Canonicalize “rule version” as `setup_definition_version_id` or remove the ambiguous alias.
4. Record the vintage-selection rule before any real-data evaluation implementation.
5. Remove the Gate B early real-data risk-acceptance bypass.

