# 17 — Open-World Opportunity Discovery & Conviction Support

**Status:** Canonical product requirement. This document owns the definition of discovery outside the user's existing watchlist and the evidence brief used to support the user's independent analysis.

## 1. Purpose

MBI must not wait for the user to name a ticker, sector, conglomerate, theme, news item or gameplay before it can find something worth investigating.

Its discovery system must search the broadest **legitimately accessible and explicitly covered universe** for potentially relevant developments, connect external context to issuer/sector/cluster exposure, detect unusual market behavior, compare historical responses and narrow the results into explainable research leads.

The goal is to reduce blind spots and expand the user's radar. It is not to guarantee discovery of every unknown opportunity, predict winners, issue automatic buy/sell instructions or replace the user's own analysis.

**Product distinction:**
- User-led research answers: "What happens if I investigate this?"
- Open-world discovery answers: "What has changed that I may not have noticed, and why might it matter?"
- Conviction support answers: "What evidence supports, contradicts or qualifies my thesis?"

## 2. Two parallel discovery directions

Both directions are required. Neither may be reduced to a watchlist scanner.

### A. Top-down, catalyst-led discovery

Start from external information, then identify possible transmission paths into the Indonesian market:

`Macro / Policy / Political-Fiscal / Commodity / Regulation / Global Event / Public News → Economic or Industry Mechanism → Sector / Subsector → Exposed Issuers / Clusters → Issuer-Specific Evidence → Market State / Response → Historical Analogue → Research Lead`

Examples of source categories (not automatic conclusions):
- BI, BPS, Kemenkeu, OJK, ESDM and other public institutions;
- IDX announcements, disclosure, PUBEX, financial statements and annual reports;
- issuer investor-relations material;
- commodity, FX, rates and global macro data;
- public news and user-captured reports;
- policy calendars, public project/procurement announcements and regulatory changes where accessible.

For each catalyst, MBI must identify a **transmission mechanism**, not merely keyword overlap. Examples of mechanisms include revenue exposure, input cost, capex, financing cost, export/import access, regulatory eligibility, project award, demand, supply, inventory, currency sensitivity or index/market-structure effects.

The system must map the mechanism to issuers using source-backed business facts and relationships. A sector headline alone is not sufficient to label every issuer in that sector as a beneficiary.

### B. Bottom-up, anomaly-led discovery

Start from broad issuer/market observations, including securities outside saved watchlists:

`Issuer / Sector / Cluster / Market Anomaly → Baseline Comparison → Related Issuer / Relationship Expansion → Search for Public Context → Historical Analogue → Research Lead`

Candidate triggers can include:
- unusual turnover, volume, frequency or range relative to that issuer's own baseline;
- abnormal relative strength or weakness;
- price/turnover divergence;
- compression/expansion or unusual gap behavior;
- leader/follower sequence or cross-sectional breadth change;
- unusual cluster liquidity share or sector dispersion;
- issuer behavior inconsistent with its normal character;
- a pre-event or unexplained anomaly with no known public catalyst;
- changes around IPO lifecycle, suspension/reopening or corporate actions, if relevant data exists.

An anomaly is an observation, not proof of accumulation, distribution, manipulation, insider activity or a specific actor's intent. If no explanation is found, retain `UNEXPLAINED_ANOMALY` and the bounded search record.

## 3. Additional discovery routes

The system should support several routes that can converge on the same candidate:

1. **Relationship/network expansion:** expand from a known issuer, owner/controller, affiliate, supplier/customer, lender, project, commodity or policy exposure into related entities. Every edge must have a type, source, effective period and verification status. Correlation alone is not a relationship.
2. **Lifecycle discovery:** discover issuer-specific behavior around IPO age, lock-up/holding-period events, free-float changes, major-shareholder disclosures, capital use, first financial reports, suspension/reopening and corporate actions. Only use fields actually disclosed or sourced; do not invent lock-up dates, allocations, beneficial owners or free float.
3. **Cross-sectional pattern discovery:** compare behavior across issuers and cohorts to find recurring structures that the user has not named as a gameplay.
4. **Calendar/forward watch:** maintain known future dates/events (earnings, corporate actions, index reviews, policy implementation dates, lock-up dates where verified) and identify which issuers may be exposed. A calendar item is not a forecast of market reaction.

Post-IPO is an example of a lifecycle cohort, not a hardcoded strategy. Possible features include sessions since listing, IPO offer price where sourced, early return path, turnover/volume, volatility, free-float/ownership disclosures, lock-up details if disclosed, use-of-proceeds updates and first results. Missing elements stay unknown. Cohort comparisons must handle survivorship, delisting, suspension, IPO regime and changing market conditions.

## 4. Candidate types and lifecycle

Candidate types are extensible but should begin with a controlled taxonomy:
- `EXTERNAL_CATALYST`;
- `ISSUER_ANOMALY`;
- `SECTOR_OR_CLUSTER_ROTATION`;
- `RELATIONSHIP_NETWORK_LEAD`;
- `ISSUER_LIFECYCLE_OR_POST_IPO`;
- `CROSS_SECTIONAL_PATTERN`;
- `USER_SEEDED_RESEARCH_LEAD`.

A candidate may have more than one route/type when the evidence supports it. Preserve the original discovery route and later-added links rather than overwriting the origin.

Suggested workflow states:
`NEW → TRIAGE → INVESTIGATING → MONITORING → PROMOTED_TO_EPISODE_OR_STUDY`

Terminal or paused alternatives: `REJECTED`, `DEFERRED`, `EXPIRED`, `UNRESOLVED`. These states describe research workflow, not truth or investment quality. Reopening a candidate appends a status event; it does not erase prior decisions.

## 5. Candidate narrowing pipeline

A discovery run follows this sequence. A stage may return no candidate; it must not force a result.

1. **Trigger intake:** accepted EOD batch import, permitted macro/API batch, selected official document batch, selected intraday capture or user incident. A local scan may run after valid imports; it does not imply continuous source polling.
2. **Normalize evidence:** identify source, publisher, source span, publication/event/effective times, entities, topic, stated facts/claims and uncertainty. Keep source claim separate from verified fact.
3. **Generate candidate pathways:** propose affected mechanisms, sectors, subsectors, issuers, clusters or lifecycle cohorts. Preserve why each edge was proposed.
4. **Verify issuer relevance:** test the proposed business/financial/ownership/project exposure against issuer-specific evidence. Mark weak or missing links.
5. **Check market state:** inspect available issuer, peer, cluster and market observations before/after the relevant timestamp. Do not imply a layer exists if it is missing.
6. **Search history:** retrieve comparable episodes, matched issuer behavior, positive/negative/neutral cases and base rates when methodologically valid.
7. **Create research lead:** produce an evidence-backed candidate brief with open questions, competing explanations and next conditions to monitor.
8. **Human triage:** user may investigate, monitor, reject, defer or link to an existing thesis/gameplay. This action is feedback, not proof that the hypothesis is true.
9. **Episode/study promotion:** only sufficiently connected, scoped candidates become formal Market Episodes or Research Studies. A discovery candidate and a Market Episode are distinct entities.

A catalyst-led candidate need not already show a price move. A bottom-up anomaly candidate need not already have a news explanation. Requiring both too early would miss early-stage leads.

## 6. Coverage and universe discipline

Every discovery run must record:
- intended universe and its definition/as-of date;
- eligible securities, including suspended/delisted historical members where relevant;
- securities actually scanned;
- securities excluded and reason;
- data layers available for each scanned segment;
- source last-updated/as-of times;
- failures, gaps and stale inputs;
- scan rules, versions and run timestamp.

Coverage labels must be explicit, for example `BROAD_EOD_COVERAGE`, `PARTIAL_INTRADAY_COVERAGE`, `WATCHLIST_ONLY`, `EVENT_SCOPED`, or `MANUAL_RESEARCH_ONLY`. These labels describe actual run coverage, not product aspirations.

The engine must not say "whole IDX scanned" unless the imported cross-sectional batch and security-master comparison prove eligible-universe coverage for the fields used. A batch may cover many issuers but still omit rows/segments; EOD universe coverage never implies broker/orderbook/intraday coverage. A ticker excluded for missing data is not a negative result.

At Rp0, discovery is constrained by source access, user-triggered exports, local imports, manual captures, public-download permissions and the laptop being on. MBI must not claim continuous background monitoring when no data was collected. It may still perform broad EOD or document-driven sweeps after relevant data is imported.

## 7. Research lead and evidence brief

Each candidate should explain, in plain language:

- **Why it surfaced:** exact trigger, scan route and novelty/context.
- **What changed:** source-backed external development or measured market anomaly.
- **Potential transmission path:** source-backed chain from event/mechanism to sector/issuer/cluster.
- **Issuer relevance:** company-specific evidence, not merely a broad sector label.
- **Market confirmation:** price, volume, turnover, frequency, flow, cluster and benchmark behavior that are actually available.
- **Historical analogues:** comparable episodes, matching dimensions and differences.
- **Counter-evidence:** facts/measurements that weaken the thesis, failed analogues and alternative explanations.
- **Unknowns:** unavailable ownership, market data, timing, business exposure or source details.
- **What to investigate next:** documents, metrics, dates or conditions that could materially change the reading.
- **What to monitor:** observable confirmation, deterioration and invalidation conditions.
- **Personal relevance:** only if the user's journal demonstrates a validated fit; otherwise `NOT_ESTABLISHED`.

The brief is a research aid. It must link every important statement to its source or metric and display as-of times, coverage and quality flags.

## 8. Research prioritization is not a prediction score

MBI may help sort a large candidate queue, but it must not hide its reasoning in one opaque "opportunity score" or label candidates as guaranteed upside.

Show separate dimensions, where evidence exists:
- novelty relative to prior research/watchlist;
- issuer/exposure relevance;
- evidence quality and source authority;
- market confirmation or divergence;
- historical analogue availability and comparability;
- catalyst/event timing;
- counter-evidence and unresolved uncertainty;
- data coverage and freshness.

A candidate can be important to research even when market confirmation is absent. Another can have unusual market activity but no known catalyst. Missing dimensions remain `UNKNOWN`, not zero. A sort order is a queue-management aid, not an expected-return ranking.

## 9. Conviction support without replacing the analyst

MBI must support the user's independent thesis work, not decide the user's conviction. It should provide an evidence matrix with separate sections:

1. User's thesis statements (preserved as authored and time-stamped).
2. Supporting evidence.
3. Contradicting evidence and counterexamples.
4. Issuer-specific exposure and mechanism.
5. Current market confirmation / non-confirmation.
6. Historical comparable episodes and limitations.
7. Key uncertainties and alternative explanations.
8. Confirmation, deterioration and invalidation conditions.
9. User's own conviction note/decision, recorded separately from system measurements.

Do not collapse source quality, statistical confidence, market confirmation, personal playbook fit and analyst conviction into one number. They answer different questions. MBI may show calibrated probabilities only under the rules in `09_METRIC_CONTRACTS_AND_VALIDATION.md`; otherwise show counts, ranges, distributions and `DESCRIPTIVE_ONLY`/`INSUFFICIENT_DATA`.

The user remains the analyst and decision-maker. MBI supplies additional evidence, alternative views, historical context and blind-spot discovery. It does not place orders or claim that a candidate should be bought/sold.

## 10. Discovery quality and feedback

Evaluate the discovery engine itself, not only the candidate outcomes:
- candidates later investigated by the user;
- candidates judged relevant/irrelevant with reason;
- duplicate/redundant candidates;
- candidates outside saved watchlists;
- false alarms and unsupported exposure paths;
- candidates that matured into useful research episodes;
- coverage by sector, market-cap/liquidity cohort and data layer;
- known missed cases discovered retrospectively (recorded as a separate audit sample, not silently added to historical scans);
- lead time between first observable trigger and public catalyst, where timestamps can be established.

User acceptance is not evidence of correctness. User rejection is not evidence that the candidate was objectively wrong. Evaluate both model quality and human utility, and guard against feedback loops that only surface ideas matching the user's existing thesis.

## 11. Open-world safeguards

- A source can mention an issuer without proving economic materiality.
- A conglomerate/relationship graph must not infer control or coordinated intent from price correlation.
- Political or policy context is described neutrally and sourced; no ungrounded motive attribution.
- News volume or sentiment is not a substitute for an issuer transmission mechanism.
- A stock move before a headline does not prove information leakage.
- No news match in the searched corpus does not prove no public information existed.
- A top-down catalyst is not a bullish/bearish call until issuer exposure and market response are examined.
- A bottom-up anomaly can remain unexplained.
- Search and discovery coverage is bounded by the sources and securities actually observed.
- Historical analogues are not guarantees; post-IPO and event cohorts require special survivorship and regime checks.

## 12. Logical data concepts

The logical model should support, subject to sample-informed schema design:
- `discovery_run`: trigger, run type, time, universe definition and rule version;
- `discovery_coverage_snapshot`: intended/eligible/scanned/excluded universe and field-level coverage;
- `discovery_candidate`: candidate subject/type, origin route, novelty, status and rationale;
- `candidate_evidence_link`: support/contradict/context link to a source, fact, claim, observation, metric or episode;
- `candidate_open_question`: unresolved field, missing evidence request or research question attached to the candidate;
- `exposure_path`: typed event/mechanism/sector/issuer/relationship chain with source evidence and verification state;
- `candidate_review`: user's triage, reasons and timestamp;
- `candidate_monitor_condition`: observable future condition and expiry/status;
- `research_brief_snapshot`: immutable generated evidence brief and input versions;
- `thesis_evidence_link`: links a user-authored `hypothesis` of type/origin `USER_THESIS` to supporting/contradicting evidence without rewriting the original thesis.

Use the existing canonical `hypothesis` concept for a user thesis; do not create a parallel thesis entity unless schema analysis proves the existing model cannot represent it. A candidate may be explicitly linked to one or more formal episodes/studies through a typed promotion/link record. Do not create these tables mechanically without checking existing concepts for duplication. One canonical evidence record should be linked, not copied into every candidate.

## 13. Acceptance tests

At minimum, tests must demonstrate:
1. A new policy/macro document can generate sector/issuer research candidates beyond the saved watchlist, but only where a supported exposure path exists.
2. A market anomaly can generate a candidate without any known news catalyst and remain `UNEXPLAINED_ANOMALY`.
3. A known issuer relationship can expand the research graph, while an inferred/unverified edge is visibly weaker and cannot be presented as verified.
4. A post-IPO cohort study uses only available point-in-time facts, includes failed/delisted/suspended cases when eligible, and flags missing lock-up/free-float details.
5. Candidate briefs show both supporting and contradicting evidence, historical analogues, uncertainty and data coverage.
6. Candidate ranking never presents a probability or expected return unless separately validated; research priority is not an investment recommendation.
7. A candidate discovered outside the user's watchlist remains linked to the discovery run and the universe actually scanned.
8. User triage changes research status but cannot verify facts or alter immutable source observations.

## 14. Boundary with adjacent documents

- `01_END_TO_END_LEARNING_FLOW.md` defines when discovery enters the overall learning loop.
- `02_DATA_SOURCES_AND_INGESTION.md` defines how sources are acquired and validated.
- `03_ISSUER_DOSSIER_AND_STOCK_CHARACTER.md` defines issuer observations and baselines.
- `04_MARKET_EPISODES_AND_RESPONSE_RESEARCH.md` defines formal episode construction and event-response studies.
- `05_DISCOVERY_AND_ADAPTIVE_GAMEPLAY.md` defines pattern/gameplay lifecycle after discovery/research.
- `06_PERSONAL_JOURNAL_AND_EDGE_LEARNING.md` defines personal decision evaluation.
- This document alone defines open-world candidate generation, evidence briefs and conviction support.
- Doc 18 defines how a gameplay/setup is scanned across issuers, converted into Setup Instances, and tested across technical/fundamental/issuer/ownership context and parameter variants. A discovery candidate may seed setup research, but is not itself a Setup Instance.
