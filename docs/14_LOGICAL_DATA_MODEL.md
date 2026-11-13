# 14 — Logical Data Model & Relationship Map

**Purpose:** define the concepts and ownership boundaries before any SQL schema is proposed. This is a logical model, not a migration or final physical schema. The actual database must be inspected first.

## 1. Data lineage spine

Every canonical or derived record should be traceable through this spine:

`Source → Raw Artifact / Capture → Import or Extraction Run → Staging Proposal → Reviewed Canonical Record → Feature / Episode / Study → Signal or Insight → Outcome / Evaluation`

A canonical record can have multiple source/evidence links. Derived outputs retain the exact input record IDs and contract versions used.

## 2. Logical entity groups

### A. Source, acquisition batch and artifact

- `source_registry`: publisher/platform, source type, access route, acquisition mode, terms/permission note, cadence, quota, expected fields and current status.
- `acquisition_batch`: one user-triggered official download/API batch or document batch, with source route/version, trigger, declared date/filter/universe, terms review, start/end and acquisition status. A batch may contain many original artifacts and/or a single cross-sectional file with many issuers.
- `raw_artifact`: original file/URL capture/note/screenshot, hash, MIME, captured/retrieved time, local path/reference and privacy classification; links to its acquisition batch when applicable.
- `ingestion_run`: adapter/parser version, start/end, result, rows/items seen/accepted/rejected and error log.
- `staging_record`: parsed but not yet canonical data, preserving original headers/values and source location (row/page/sheet/cell/span).

One source can produce many acquisition batches; one batch can contain many artifacts and many issuers; one artifact can have many parsing/extraction runs. Reprocessing creates a new run, not rewritten past extraction. Acquisition metadata and ingestion/processing metadata must remain distinct.

### B. Identity and coverage

- `issuer`: legal/listed company identity.
- `security`: tradable instrument identity (ordinary share, warrant, rights, etc.).
- `security_alias`: ticker/name/code with effective dates.
- `universe_membership`: expected/eligible/observed coverage by universe and date range, effective-dated where historical membership is known.

The broad EOD universe for a given batch is derived from the actual accepted security rows and compared with a separately verified source-backed security master. A page's advertised row count is not a universe snapshot. Missing historical membership remains unknown; do not reconstruct a survivorship-free universe from today's list.
- `entity`: people, company, government body, project, commodity/sector or other named node as needed.
- `relationship`: source-backed, typed, effective-dated link with status and evidence.

Issuer and security are not interchangeable: one issuer may have multiple securities; ticker text can change/reuse.

### C. Claims, facts and unstructured knowledge

- `raw_input`: immutable rant/note/voice transcript/incident text.
- `extraction_proposal`: zero or more candidate records from one raw input/artifact, with source spans and extraction version.
- `claim`: a source/user statement, including type and exact source passage.
- `verified_fact`: reviewed fact with scope, unit/date and evidence; a claim does not become a fact merely through user acceptance.
- `observation`: direct user/system observation at a specific time.
- `hypothesis`: falsifiable proposed relationship or explanation.
- `evidence_link`: connects claims/facts/observations/artifacts to a subject (issuer, event, episode, study, playbook) without copying their content.

If a generic polymorphic link cannot enforce referential integrity, use typed link tables instead. Do not compromise data integrity for a shorter schema.

### D. Time series and market state

- `price_bar`: OHLCV with instrument, timeframe, timestamp/session, market segment, unit and adjustment basis.
- `market_summary_observation`: daily value/frequency/foreign/non-regular/etc. exactly as source defines.
- `flow_observation`: Trade Flow/Broker Flow/Broker Summary/foreign fields, with mode/filter/date range and source semantics.
- `orderbook_snapshot`: capture timestamp, depth levels, price bands and quantities.
- `macro_series` + `macro_observation`: series metadata plus values with reporting period, published date and vintage/revision.
- `market_state_snapshot`: derived market/sector/cluster/issuer features at an explicit as-of time and feature version.
- `metric_contract` / `feature_definition`: formula/version/dependencies/units/missing-data rules.

Time-series tables must preserve source granularity. Do not collapse 1-minute, 15-minute and daily bars into an ambiguous common row.

### E. Event and episode research

- `external_event`: policy/macro/issuer/corporate/news event with occurrence, publication, first-observed-public and effective times kept separately.
- `event_document_link`: documents and claims supporting or contradicting the event record.
- `market_episode`: research container with origin, scope, start/end and status.
- `episode_member_link`: links episodes to issuers/securities/clusters/events/observations/market states, with role and rationale.
- `episode_hypothesis_link`: one or more hypotheses and competing explanations.
- `research_study`: inclusion rules, unit of analysis, windows, benchmark method, feature versions, sample and evaluation period.
- `study_episode_member`: explicit set of episodes included/excluded and reason.
- `study_result`: metrics, distributions, uncertainty, limitations and reproducible run reference.

The independent sample unit must be explicit. Multiple securities affected by one event may belong to one event episode and cannot automatically count as independent events.

### F. Pattern, detector and gameplay

- `pattern_candidate`: recurring behavior proposal supported by episodes/measurements.
- `detector_definition` + immutable `detector_version`: supported condition/rule/model, inputs, parameters, status and tests.
- `gameplay_hypothesis`: original user/data/hybrid idea, preserving exact language and source.
- `playbook` + immutable `playbook_version`: decision process, eligibility, trigger, execution, invalidation, risk and approval state.
- `signal_record`: detector result at a point in time with input snapshot and version.
- `prediction_record`: forecast for explicit event/horizon and model/calibration version; archived before outcome.
- `outcome_record`: matured label and path metrics at pre-defined horizon.

A pattern can exist without a detector; a detector can identify conditions without creating a gameplay; a playbook can remain discretionary if some rules are qualitative.

### G. Setup-instance and parameter learning

- `setup_family`: reusable family of conditions studied across securities/time; it may link to one or more source gameplay hypotheses or discovered patterns; it is not itself a signal or trade.
- `setup_definition_version`: immutable exact conditions, feature semantics, parameters, timeframe, price basis, eligibility and missing-data rules. Draft/non-runnable versions are allowed while semantics are unresolved.
- `setup_scan_run`: reproducible scan with setup/parameter version, input-as-of, time range, intended universe, data versions and run metadata.
- `setup_scan_coverage`: eligible, evaluated, matched, non-matched, excluded and unavailable counts/reasons by universe/data layer.
- `setup_scan_matrix`: full security/time evaluation results. May be stored in immutable Parquet for scale; retain path/hash/schema/version and enough metadata to reproduce denominators. Do not discard non-matches when the study requires them.
- `setup_instance`: one matched occurrence of a setup version for one security/time. It references the scan run and immutable as-of feature snapshot. It is not a trade.
- `setup_occurrence_group`: groups adjacent/overlapping instances for the same security/setup family into one continuing opportunity spell under an explicit gap/re-arm rule; it prevents repeated daily matches from inflating the effective sample.
- `parameter_experiment`: research-only definition of justified ranges/variants, objective, baseline, cohort, time split, constraints and multiple-testing plan.
- `parameter_variant`: one precise configuration tested in an experiment. Testing does not edit an active setup/playbook version.
- `entry_scenario`: explicit hypothetical entry/management assumption attached to experiment/variant/setup occurrence, with fill convention, resolution, costs/slippage and ambiguity status.
- `setup_outcome`: market-path outcome for a setup instance/scenario/horizon, separate from any user execution.
- `study_setup_member`: explicit inclusion/exclusion link between research study and setup instances/scan results, with reason.
- `cohort_definition`: versioned rule for grouping by issuer character, liquidity, fundamentals, sector/cluster, ownership/UBO network, regime, event or lifecycle. Cohorts may overlap.

A playbook version references the exact setup definition version(s) and its separate entry/exit/risk policies; do not duplicate setup rules into another free-text field. A feature snapshot references canonical point-in-time observations and metric versions; do not duplicate the full OHLCV, fundamental or ownership fact into every setup instance. Fundamental/ownership values must be selected by publication/effective time known at the instance's `input_as_of`. Wide scan matrices/features/outcomes may live in Parquet while experiment, instance, coverage and lineage metadata live in SQLite.

A setup instance is an observation, not necessarily an independent statistical sample. The study defines its independent unit and handles issuer/date/event/cluster/network dependence. Ticker count or setup-row count must not be presented as effective independent sample size.

### H. Personal decision learning

- `trade_idea` / `personal_decision`: entered, watch, rejected, no-trade, missed and other decision states.
- `decision_snapshot`: immutable as-of context, thesis, planned setup/risk and available evidence.
- `decision_revision`: subsequent thesis/plan changes with timestamps and reasons.
- `execution_fill`: actual fill only, linked to broker report or user entry and source.
- `trade_outcome`: realized result and horizon-based path result.
- `personal_learning_run`: cohorts and analysis version connecting decisions to market state, episodes, playbooks and outcomes.

Actual trades and hypothetical/no-trade outcomes must remain separate populations.

### I. Open-world discovery and conviction support

- `discovery_run`: run trigger/type, ruleset version, as-of time and intended universe.
- `discovery_universe_snapshot`: eligible/scanned/excluded securities and reasons, plus source/field coverage by run.
- `discovery_candidate`: candidate subject/type, origin route (top-down, bottom-up, network, lifecycle, cross-sectional), rationale, novelty and workflow status.
- `exposure_path`: ordered, typed chain from event/condition through economic mechanism/sector/entity/issuer, with evidence and verification state for every material edge.
- `candidate_evidence_link`: support/contradict/context links to canonical sources, claims, facts, observations, metrics, episodes or studies. Store references, not duplicate facts.
- `candidate_open_question`: unresolved field, missing evidence request or research question attached to a candidate.
- `candidate_research_link`: explicit promotion/association from a discovery candidate to one or more formal Market Episodes or Research Studies, with link type, rationale and timestamp.
- `candidate_review`: user triage status, reasons, timestamp and reviewer note.
- `candidate_monitor_condition`: observable confirmation/deterioration/invalidation condition with source/metric dependency, expiry and status.
- `research_brief_snapshot`: immutable brief version, input IDs, source/metric versions, coverage and generated time.
- `hypothesis` with origin/type `USER_THESIS` + `thesis_evidence_link`: reuse the canonical hypothesis concept for user-authored thesis statements and support/contradiction links; preserve thesis revisions and the user's own conviction note separately from system measurements. Do not create a parallel thesis table without a demonstrated modeling need.

A discovery candidate is not automatically a Market Episode. Promotion requires explicit scope, linked observations/events/issuers, a chronology and a research question. Research-priority dimensions are not a prediction or conviction score.

### J. Supporting portfolio context

- `brokerage_account_ref`: local alias only; avoid storing credentials or sensitive broker identifiers.
- `account_event`: imported fills, cash movements and corporate actions when needed.
- `portfolio_view`: user-defined aggregation of accounts.
- `strategy_sleeve`: virtual tag/allocation for analysis, not a second asset ledger.
- `position_snapshot`: dated account/view position for decision context.

One actual fill is stored once. Views/sleeves reference it; they must not duplicate it.

## 3. Setup-learning integrity rules

- One setup instance refers to one immutable setup definition/version and one security/time occurrence.
- The same issuer/time may match several setup families; preserve separate matched instances but do not treat them as independent events automatically.
- Consecutive matches for one security/setup family may be grouped into one `setup_occurrence_group` under a versioned re-arm rule; preserve all raw matches.
- A setup scan must retain denominator/coverage information, including non-matches, exclusions and unavailable data. Distinguish universe eligibility, trigger-feature evaluability and optional-context feature completeness.
- Entry scenarios are counterfactual research assumptions; they never create execution fills, portfolio holdings or cash movements.
- Market-wide setup outcomes are distinct from the user's trade outcomes.
- Numeric technical/fundamental/ownership features use a versioned as-of snapshot; do not use later-published/revised facts in historical replay.
- Parameter variants belong to a research experiment until explicitly approved as a new immutable setup/playbook version.
- Preserve all tested variants and negative results; do not store only the best parameter set.
- Related issuer instances sharing event/date/cluster/network and repeated adjacent matches within one occurrence group are dependent unless the study justifies otherwise.

## 4. Discovery-specific integrity rules

- One discovery run owns one immutable coverage snapshot; re-running creates a new run, not an overwrite.
- Candidate deduplication must preserve distinct discovery routes and evidence; merge suggestions require review.
- One canonical source/claim/measurement can support multiple candidates through links; do not duplicate the fact.
- A candidate-to-episode/study promotion is an explicit link; do not merge candidate and episode records or erase candidate discovery history.
- Exposure path edges require typed endpoints, source evidence, validity period and verification status.
- A later source may add a new evidence link or brief version but must not rewrite the original candidate snapshot.
- User triage is not source verification. User conviction is not a measurement.
- The universe snapshot must distinguish eligible, scanned, excluded and unavailable securities.

## 5. Key uniqueness and idempotency candidates

Batch acquisition is not a separate data truth: it groups artifacts downloaded together and records the declared batch scope. The actual rows and universe coverage are derived only after inspecting the artifact. Do not assume a batch is complete because its source page displays a large row count.

The exact physical unique keys depend on source samples, but must consider:
- raw artifact: content hash + source identity (same bytes may still be attached to different source URLs; preserve both source links if needed);
- price bar: security ID + timestamp + timeframe + segment + source + price basis;
- EOD market summary: security ID + session date + market segment + source;
- macro observation: series ID + reporting period + publication/vintage timestamp + source;
- execution: account ref + provider transaction/fill ID; if absent, cautious fingerprint plus duplicate review;
- document: source URL/filing ID + version/hash; corrected files are not overwritten;
- signal: detector version + input/as-of snapshot + instrument/scope;
- setup instance: setup definition version + security ID + evaluation timestamp + scan/experiment context;
- parameter variant: experiment ID + canonicalized parameter configuration + feature/data versions;
- setup outcome: setup instance + scenario/variant + horizon + outcome method/version.

Do not finalize uniqueness constraints until real samples and revision behavior are understood.

## 6. Required temporal fields by record type

Not every table needs every timestamp, but use explicit fields where relevant:
- market observation: observed_at, captured_at;
- event: occurred_at, published_at, first_observed_public_at, effective_from/to;
- filing/document: published_at, retrieved_at, reporting_period_start/end, supersedes_ref;
- macro fact: period_start/end, published_at, vintage_at, revised_at;
- decision: decision_at and immutable context_as_of;
- feature/signal/setup instance: computed_at, input_as_of, formula/detector/setup version;
- prediction/outcome: predicted_at, outcome_window_start/end, outcome_matured_at.
- discovery run/candidate: run_started_at/finished_at, input_as_of, discovered_at, candidate_as_of, review_at, brief_generated_at.

Unknown timestamps stay null with a quality flag. Timezone is explicit; IDX local session times use `Asia/Jakarta`.

## 7. Questions for the schema audit

Codex must report whether the existing v0.4 database (if present) supports these concepts, whether a concept is absent or merely named differently, and whether a proposed relationship would create duplication or circular dependencies. Do not generate SQL until the user approves the logical model and sample-informed mappings.
