# MBI Gate -1 Owner Approval Matrix

**Review outcome:** Do not approve all as written. Approve only the bounded decisions below, with the stated conditions.

| Decision | Safe to approve now? | Review disposition |
|---|---:|---|
| D-A — defer issuer-document and macro samples | **Yes, conditionally** | Approve deferral as scope management. Keep route/terms/sample status explicitly `UNVERIFIED` or `DEFERRED`; no implication that the sources are cleared. |
| D-B / C1 — canonical `universe_coverage_snapshot` naming | **Yes, conditionally** | Approve the naming direction and observed-versus-verified distinction. Apply only with an alias/migration wording that does not erase historical traceability. |
| D-B / C2 — setup occurrence, scan membership, vintage | **No** | Revise the identity. A multi-definition scan requires setup-definition/variant context in scan membership, plus an unambiguous vintage/evaluation context. |
| D-B / C3 — four mutually exclusive scan states | **No, not as written** | The top-level-state idea is approvable after preserving required-vs-optional missingness, no-activity, ambiguous-zero, and feature-quality reasons as separate provenance/status dimensions. |
| D-B / C4 — optional playbook link for discretionary playbooks | **Yes** | Consistent with the canonical distinction between runnable setups and discretionary playbooks. |
| D-B / C5 — manual-first local account aliases | **Yes** | Consistent with privacy and portfolio invariants; keep broker imports deferred. |
| D-C — history-sufficiency gate | **Yes, conditionally** | Approve the existence of the gate and the `INSUFFICIENT_DATA` behavior. Threshold numbers must remain provisional policy defaults, not validated statistical requirements. |
| D-D — no backtesting/“works” claims before point-in-time sufficiency | **Yes** | Consistent with look-ahead and unsupported-probability safeguards. |
| D-E — external LLM processing of personal notes off by default | **Yes** | Consistent with local-first privacy; retain explicit per-use consent and local logging. |
| D-F — no historical backfill now | **Yes** | Consistent with the approved operating strategy. Later cross-date stability sampling remains manual and separately authorized. |
| D-G — observed-universe versus verified-market-universe split | **Yes** | Consistent with the 945-row evidence ceiling. No coverage percentage or whole-market wording before independent-master reconciliation. |
| D-H — intended-use statement for IDX data | **No as worded** | Rewrite as an internal handling restriction and unresolved terms status. It must not say that approval permits importer use or closes Gate B. |
| Gate A — sample/import-contract readiness | **Yes, conditionally** | The contract may be accepted as a sample-derived, schema-agnostic draft with unknown semantics labelled. It does not authorize real-data processing. |
| Gate B — intended-use/source-terms readiness | **No** | Correctly remains open until primary-source terms are reviewed and restrictions recorded. |
| Gate C1 — observed-universe readiness | **Yes, conditionally** | Approve observed-row processing terminology only; retain `OBSERVED_ONLY` and no completeness claim. |
| Gate C2 — verified-universe readiness | **No** | Correctly blocked pending an independent official security master and reconciliation. |
| Gate D — historical sufficiency | **Yes, conditionally** | Approve as a research-claim gate, with provisional thresholds and explicit gap/calendar uncertainty. |
| Sprint 0 proposal | **No authorization from this review** | Scope is foundation-only and acceptable in principle, but it requires a separate owner approval after proposal corrections. Current Gate -1 instructions still prohibit starting it. |

## Required pre-approval corrections

1. Fix scan-membership identity for multiple setup definitions.
2. Make vintage/evaluation context unambiguous and preserve feature dependency provenance.
3. Carry zero/missing/no-trade status semantics into the evaluation/feature model.
4. Rewrite D-H so it cannot be read as IDX permission or Gate B clearance.
5. Rename history thresholds as provisional policy defaults and remove “validated floor” language.
6. Add the explicit Sprint 0 non-authorization sentence.

