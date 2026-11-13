# 01 — End-to-End Market Learning Flow

## 1. The central flow

The product runs two connected loops:

**Open-world discovery loop:**

`Broad External Context + Broad Issuer/Market Observations → Discovery Run → Candidate Generation → Exposure Mapping / Market Validation → Research Lead`

**Market-learning loop:**

`Candidate or User Research Question → Market Episode / Research Study → Response Analysis → Historical Comparison → Pattern Discovery → Gameplay/Playbook Evaluation → Personal Edge → Evidence Brief → Updated Research`

A discovery candidate is a lead to investigate, not automatically a Market Episode. It becomes an episode/study only when scope, membership, chronology and research question are explicit.

Market Episode & Response Learning is the research core. Open-World Opportunity Discovery is a first-class front door that must find leads beyond user-selected tickers and questions. **Market learning is independent of whether the user traded.** User gameplay is one hypothesis source, not a boundary on discovery. Portfolio/accounting is supporting context, not the main product. See Doc 17 for the exact discovery funnel and conviction-support output.

## 2. Stage A — External context and information availability

Possible inputs:
- Macro releases and revisions (BI, BPS, Kemenkeu, global sources).
- Policy, regulation, political/fiscal developments, commodity changes.
- IDX disclosure/announcement, PUBEX, financial statements, annual reports and corporate actions.
- Issuer IR documents and credible news.
- User-captured public rumors or market observations, explicitly unverified.

For each item, separate:
- `event_occurred_at`: when the underlying thing happened, if known.
- `published_at`: when the source says it was published.
- `first_observed_public_at`: earliest public timestamp MBI has actually found, not a claim that no earlier source existed.
- `retrieved_at`: when MBI imported/captured it.
- `effective_from/to`: period when a rule/ownership/contract/status applies, if known.
- `reporting_period`: period described by a macro/financial statistic.

Do not conflate publication date with event date, fiscal period, effective date or retrieval date. Keep revisions/vintages.

## 3. Discovery must not wait for user prompts or watchlists

After each accepted broad-market EOD batch, permitted macro batch or selected document/capture import, MBI should be able to run:
- catalyst-led top-down discovery from macro/policy/news/commodity/regulation/issuer documents;
- anomaly-led bottom-up discovery from issuer, sector, cluster and broad-market observations;
- relationship/network expansion using source-backed ownership, business, financing and project links;
- lifecycle/cohort discovery such as post-IPO behavior when the required data exists;
- cross-sectional pattern discovery across the covered universe.

Every run records its trigger, universe, securities scanned/excluded, field-level coverage, last source timestamps and scan-rule version. The system must show when discovery is only watchlist-scoped or event-scoped. It must not claim to scan the whole IDX unless coverage proves it.

The candidate funnel maps external developments to a possible transmission mechanism, then to sectors/issuers, issuer-specific evidence and observed market state. Keyword matches alone are not exposure evidence. Candidate ranking is a research-queue aid, not an expected-return or probability score. See `17_OPEN_WORLD_OPPORTUNITY_DISCOVERY_AND_CONVICTION_SUPPORT.md`.

## 4. Stage B — Market state before and after an event

Capture market state at suitable time buckets and data granularities:
- IHSG and broad market: return, breadth, turnover, volatility, sector/cluster performance where data exists.
- Issuer: price structure, volume, value, frequency, turnover, volatility and gap/range behavior.
- Flow: transaction/broker/foreign data only when the source explicitly provides it.
- Orderbook: time-stamped queue snapshot only; never convert into executed trades.
- Macro/commodity/FX/rates: series values with publication/vintage metadata.
- Issuer exposure: links to revenue, assets, project, financing, ownership or policy exposure supported by documents.

Pre-event baseline must use data available before the event/publication cutoff. Post-event outcome is measured separately.

## 5. Stage C — Construct market episodes

Create an episode when a research question connects a context/event with observed market behavior. It can be:
- event-led (e.g. regulation or financial result);
- anomaly-led (e.g. unexplained turnover/price/flow behavior);
- issuer-led (e.g. recurring behavior around suspensions or corporate actions);
- cluster-led (e.g. leader/follower rotation);
- user-led (e.g. incident captured while watching the market).

Episode membership is explicit and reviewable. Do not automatically join every same-day move into one episode. Record competing explanations and unresolved links.

## 6. Stage D — Response analysis

For each relevant issuer/cluster/market benchmark, measure pre-defined windows, for example:
- pre-event baseline window;
- immediate event window;
- post-event follow-through/reversal window;
- longer persistence window.

Windows must be configurable per event type and data frequency, not hardcoded as universal truths. Compute return, abnormal return under a stated benchmark model, turnover/volume anomaly, range/volatility, breadth, flow changes and outcome metrics where source data supports them.

A market-adjusted return is a descriptive benchmark, not proof that the event caused the return. Event studies can be confounded by overlapping announcements, macro shocks, index rebalances, corporate actions, suspensions and pre-existing trends.

## 7. Stage E — Historical matching and learning

Find prior episodes with comparable:
- event type and issuer exposure;
- macro/market regime;
- issuer Stock Character and liquidity profile;
- pre-event market state;
- price/volume/flow structure;
- cluster and sector behavior.

Compare successful, failed, muted and ambiguous responses. Match criteria and distance/similarity calculations must be versioned. Show which dimensions matched and which did not. Similarity is not probability.

## 8. Stage F — Gameplay and personal edge

Learning can produce:
- descriptive market patterns;
- Setup Instances: each occurrence of a versioned setup across the declared covered universe, whether traded or not;
- cross-issuer parameter/condition studies using point-in-time technical, fundamental, issuer-character, market, cluster and ownership context;
- candidate detectors (recognition rules);
- user gameplay refinement (conditional operating rules);
- personal edge results (what works for the user's selection/execution);
- research questions and monitoring conditions.

Setup-instance and parameter-learning requirements are owned by `18_SETUP_INSTANCE_AND_PARAMETER_LEARNING.md`. A Market Episode is a context/research container; a Setup Instance is a security/time occurrence. They are different units and may link to one another, but must not be merged. Market-wide setup outcomes are calculated even when the user did not trade; Personal Edge analyzes the user's selected/executed subset separately.

Keep these populations separate. A pattern may exist in the market but not suit the user's horizon, capital, risk tolerance or execution. A user trade can lose even when the market pattern was valid.

## 9. Stage G — Feedback and model governance

After each new session/event/outcome:
1. append valid source data;
2. update deterministic features and baselines under versioned contracts;
3. mature outcomes only when the specified horizon is complete;
4. update descriptive statistics and candidate research;
5. archive any forecast/signal with the exact model version and information available then;
6. evaluate against later outcomes;
7. propose new/changed detectors or playbooks for shadow review;
8. require explicit approval before a production version changes.

No past prediction or decision snapshot is rewritten after later data arrives.

## 10. Update cadence in a local-first application

MBI does not assume a permanent live feed or hidden background learning. Its update loop is explicit and triggered by data availability:

- **On every import/capture:** archive the raw input, validate it, append staging/canonical records after review, and enqueue affected derived features.
- **After a broad-market EOD batch import:** update all valid issuer/session features represented in the file, market/cluster state and dossier/universe coverage for that trading date. The actual rows—not the website's advertised universe—define observed coverage.
- **After an intraday/manual flow capture:** update only the corresponding intraday observation layer; do not backfill missing minutes or pretend the capture is continuous.
- **After a new document/news/macro item:** create extraction proposals, event chronology and issuer-exposure review tasks. A human review is required for material claims/links.
- **When an outcome horizon matures:** calculate outcome labels using the specified source resolution and ambiguity rules.
- **Research refresh:** recalculate descriptive statistics when new eligible observations arrive. Candidate pattern/model evaluation runs on demand or in a controlled batch, not after every candle.
- **Production update:** a new detector/playbook/model version requires shadow evidence and explicit approval.

The UI must show last successful acquisition batch, last import, last feature calculation, last research evaluation and stale sources. Acquisition may be user-triggered even though processing after intake is automatic. If the laptop is off or no file is imported, MBI does not claim to have observed the market.

## 11. What the user sees

A useful reading should follow this order:
1. **Observed:** what changed and what data proves it.
2. **Context:** what external/issuer/market information may be relevant.
3. **Historical comparison:** which episodes resemble this and how they resolved.
4. **Interpretation:** supported explanations plus alternatives/unknowns.
5. **Counter-evidence and uncertainty:** what contradicts the reading or remains unknown.
6. **Conditions:** what would confirm, weaken or invalidate the reading.
7. **Personal relevance:** whether the user's validated playbooks historically fit these conditions.

Never jump directly from a headline to a trade recommendation. The user remains the analyst; MBI supports conviction with traceable evidence, alternatives and historical context rather than an opaque conviction score.


## 12. Setup-learning distinction

Do not use `Market Episode`, `Setup Instance`, `Trade Idea` and `Execution Fill` interchangeably:
- Market Episode: a scoped context/event/anomaly research container, potentially involving multiple issuers.
- Setup Instance: one occurrence of a specific setup version on one security at one as-of timestamp.
- Trade Idea/Decision: what the user considered or decided.
- Execution Fill: an actual transaction supported by a broker record or explicit user entry.

A setup may generate many instances across unrelated issuers and ownership groups. Each instance links to an immutable as-of feature snapshot. Entry alternatives are research scenarios, never actual fills. See Doc 18.
