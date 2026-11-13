# 05 — Pattern Learning, Gameplay & Adaptive Playbooks

## 1. Gameplay is downstream of open-world research, not the discovery engine

Open-world candidate discovery is defined only in `17_OPEN_WORLD_OPPORTUNITY_DISCOVERY_AND_CONVICTION_SUPPORT.md`. That engine discovers issuer/theme/sector/cluster/event leads outside the user's radar. This document begins after research observations and episodes exist; it defines how repeated patterns, detectors, gameplay hypotheses and playbooks are evaluated. Do not duplicate candidate-generation or conviction-brief rules here.

MBI learns market behavior first. Gameplay is one possible use of that learning. There are three peer sources of gameplay hypotheses:

1. **User-originated:** the user's own specific, informal or unconventional ways of reading/playing the market.
2. **Data-discovered:** patterns proposed by historical market episodes, issuer behavior, external context and response studies.
3. **Hybrid:** a user's gameplay is tested and refined using data-discovered conditions, counterexamples and market regimes.

The system must not be limited to a predefined list of setups or require the user to formalize every idea before it can be researched.

## 2. Separate Pattern, Detector, Gameplay, Setup and Playbook

Use the detailed cross-issuer setup/parameter specification in `18_SETUP_INSTANCE_AND_PARAMETER_LEARNING.md`. This document owns conceptual lifecycle and governance; Doc 18 owns setup occurrence data and research evaluation.

- **Pattern:** a recurring measurable market behavior found in data. It may be non-actionable.
- **Detector:** a versioned rule/model that identifies a pattern or condition.
- **Gameplay hypothesis:** an idea about how to respond to a market situation. It may begin as informal language.
- **Setup family:** a reusable family of entry/market conditions studied across issuers and time.
- **Setup definition/version:** exact, versioned, testable conditions and parameters for detecting one setup family.
- **Setup instance:** one occurrence for a specific security and timestamp under a setup version, whether or not the user traded it.
- **Entry scenario:** a hypothetical, explicitly defined entry/management alternative evaluated separately from the setup itself.
- **Playbook:** a versioned operational decision procedure that may combine setup eligibility, entry, exit, risk, execution and invalidation rules.
- **Signal:** one timestamped detector output.

A detector does not automatically create a playbook. A pattern can be informative without being tradable. A Setup Instance is not a trade. A playbook may combine several detectors and qualitative context, but only supported rules can be evaluated automatically. A parameter experiment is research-only and does not alter an active playbook.

## 3. Open-ended gameplay intake

Preserve the exact original description. The system may propose:
- purpose and intended horizon;
- universe/issuer eligibility;
- market/sector/cluster regime conditions;
- issuer Stock Character conditions;
- event/macro/ownership context;
- setup structure and trigger;
- execution, adds/trims, exit and invalidation;
- risk/sizing rules;
- exceptions and no-trade conditions;
- evidence/examples/counterexamples;
- metrics needed to test the idea.

Classify every proposed rule as:
- `HARD_GATE`;
- `SOFT_EVIDENCE`;
- `CONTEXT_ONLY`;
- `EXECUTION_RULE`;
- `OBSERVATION_ONLY`;
- `UNKNOWN_OR_NEEDS_CLARIFICATION`.

Vague ideas such as "broker terlihat nahan" remain observation-only until a defensible measurable proxy is defined. Do not invent thresholds from prose.

## 4. Pattern and playbook lifecycle

A market pattern:
`OBSERVED → HYPOTHESIS → OPERATIONAL_DEFINITION → HISTORICAL_TEST → OUT_OF_SAMPLE_TEST → SHADOW → VALIDATED / REJECTED / UNRESOLVED`

A user/data-derived playbook:
`RAW_GAMEPLAY → CLARIFICATION → DRAFT → EVIDENCE_COLLECTION → HISTORICAL_TEST → SHADOW → ACTIVE_BY_APPROVAL → PAUSED / RETIRED`

These lifecycles are related but not identical. A pattern can be validated and still not be a suitable gameplay. A playbook can remain a discretionary user method without automatic detector support.

Each detector/playbook has immutable versions. A change in source requirement, rule, timeframe, universe, entry/exit or hard gate creates a new version. Existing decisions and prior signals retain the version used at the time.

## 5. Learning loops

### A. Setup-instance and parameter learning

For every declared gameplay/setup version, MBI evaluates the eligible covered security/time population, not merely the user's trades. Preserve matched occurrences, non-matches, exclusions and unavailable observations. Each Setup Instance links to a point-in-time feature snapshot including technical values, valid fundamental facts, issuer personality, market regime, sector/cluster and dated ownership/UBO relationships where available. Test entry-distance and other parameter variants as separate research scenarios. Full rules, worked examples, outcome semantics, cohort comparison and anti-overfit controls are specified only in Doc 18.

### B. Descriptive market learning

Build rolling issuer/market/cluster baselines and describe how behavior differs by regime, liquidity, event type and time. Update only from validated observations and preserve feature versions.

### C. Pattern discovery from episodes

Search scoped market episodes and historical time series for repeated structures, event-response relationships, pre-event anomalies, leader/follower behavior, liquidity migration and combinations not yet represented in a playbook. Broad candidate generation outside the user's radar is specified in Doc 17. Pattern discovery here produces testable market-pattern candidates, never facts or immediate orders.

### D. Gameplay evaluation

For user-defined, discovered and hybrid playbooks, compare:
- eligibility frequency and missed opportunities;
- setup validity vs actual execution quality;
- market/issuer context;
- entry and exit methods;
- risk, reward and path outcomes;
- counterexamples and failure conditions;
- out-of-sample performance.

### E. Personal edge learning

Evaluate whether the user's selection, timing, sizing and management are effective in conditions where a market pattern exists. See `06_PERSONAL_JOURNAL_AND_EDGE_LEARNING.md`.

## 6. What self-learning means

Self-learning is a controlled data/research loop, not an LLM silently changing code:
1. ingest new valid data;
2. calculate deterministic features under named metric versions;
3. create/update market-state and episode records;
4. mature outcome labels only after their horizons finish;
5. update descriptive statistics and candidate models;
6. compare candidate findings against baselines and counterexamples;
7. run out-of-sample/walk-forward and shadow evaluation;
8. preserve old predictions and model snapshots;
9. require review/approval before active detector/playbook version changes.

No automatic production promotion. Model drift or deteriorating performance can trigger a review or pause proposal.

## 7. Recommendations and probabilities

A recommendation is a conditional research reading, not an instruction. It should say: what was observed, why the condition may matter, comparable episodes, confirmation/invalidation, data gaps and whether any user playbook appears relevant. When the lead originates outside the user's radar, use the research brief defined in Doc 17. The system supports the user's analysis; it does not replace the user's judgement.

A probability can be displayed only when all are defined:
- exact outcome event and horizon;
- eligible population/cohort and exclusions;
- independent sample unit (episodes, not duplicated ticker-detector rows);
- baseline/base rate;
- no-look-ahead data rules;
- time-based out-of-sample evaluation;
- calibration and uncertainty interval;
- current data coverage and applicability limits.

Otherwise display descriptive counts/distributions and `DESCRIPTIVE_ONLY` or `INSUFFICIENT_DATA`. A broader cohort fallback may be used only with visible label (e.g. same setup family across issuers, not this issuer's own history). Report raw Setup Instance count separately from effective independent sample size where cases share an event/date/cluster. Similarity score and a best-performing parameter value are not probability.

## 8. Avoid overfitting and false discovery

Do not search thousands of combinations and report only the best. Start with interpretable features, predeclared parameter ranges and simple baselines. Record every hypothesis/variant tested, including negative results. Prefer robust parameter plateaus across nearby values and time periods over a sharp isolated optimum. Control multiple comparisons, overlapping episodes, correlated tickers, selection bias, survivorship bias and regime dependence. Keep a final untouched time period or use proper walk-forward validation with purging/embargo where outcome windows overlap. Small samples remain exploratory.

## 9. Market mechanics evolve

New market behaviors can be added without rewriting the core:
- new hypothesis or category: data/configuration and review;
- new feature: metric contract + tests;
- new detector operator: engineering change + tests;
- new source: source/terms/format review + adapter contract;
- new workflow/storage entity: schema impact analysis and migration only if necessary.

No arbitrary code execution from gameplay config. YAML/JSON is declarative configuration, never Python, SQL, shell or `eval` input.
