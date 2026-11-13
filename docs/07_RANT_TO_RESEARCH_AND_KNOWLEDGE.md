# 07 — Universal Intake: Rant-to-Research & Evidence Knowledge

## 1. Purpose

Any useful material can enter MBI through one universal inbox: informal rant, voice transcription, market incident, gameplay thought, chart screenshot, URL, news item, PDF, PUBEX, spreadsheet, trade reflection or research question.

The system must translate unstructured input into **proposed structured records** that other modules can use after review. This is not only a notes feature; it is an intake path to market episodes, metrics, issuer research, hypotheses, gameplay and personal learning.

## 2. Immutable raw input

Store the exact original text/file/URL and:
- captured time and timezone;
- user-provided event time if any;
- attachments and content hashes;
- source channel (typed note, voice transcript, screenshot, URL, imported document);
- parser/extraction version;
- privacy classification.

Never overwrite the raw input. Corrections are new versions linked to the original.

## 3. One input may produce many candidate records

Possible extraction targets:
- `OBSERVATION_CANDIDATE`;
- `SOURCE_CLAIM_CANDIDATE`;
- `HYPOTHESIS_CANDIDATE`;
- `EVENT_CANDIDATE`;
- `ENTITY_OR_RELATIONSHIP_CANDIDATE`;
- `METRIC_OR_FEATURE_REQUEST`;
- `GAMEPLAY_CANDIDATE`;
- `RESEARCH_QUESTION`;
- `TRADE_DECISION_OR_REVIEW_CANDIDATE`;
- `PORTFOLIO_CONTEXT_NOTE`;
- `EMOTIONAL_OR_GENERAL_NOTE` with no market facts.

Each candidate stores exact source span(s), relevant ticker/entities, proposed event/date, extracted fields, unresolved fields, extraction method/model/version and extraction confidence.

**Extraction confidence is not truth confidence.** A model may correctly understand that the user wrote "A owns B" while the claim itself remains unverified.

## 4. Review and promotion

Lifecycle:
`RAW_INBOX → EXTRACTION_PROPOSAL → USER_REVIEW → ACCEPTED_RESEARCH_ITEM / EDITED / REJECTED / DEFERRED`

Acceptance means the user accepts the extracted note/hypothesis as a useful research item. It does not make an ownership/news claim true. To become a canonical `VERIFIED_FACT`, the item still needs source evidence and verification workflow described in the product's evidence rules.

Never allow intake output to directly create:
- an actual trade fill or deposit/withdrawal;
- a verified ownership/control edge;
- a public event timestamp without source;
- a quantitative market observation without measured data;
- an active detector/playbook;
- a portfolio ledger movement;
- an executable code/config rule.

## 5. Linking to the rest of MBI

A reviewed item can also seed an open-world research lead: for example, a policy thought can initiate top-down sector/issuer mapping, while a market observation can initiate bottom-up anomaly investigation. This is not the same as limiting discovery to user prompts; scheduled/on-import broad discovery is separately defined in Doc 17.

A reviewed item can link to:
- issuer/security dossier;
- market observation or incident;
- market episode;
- source document/claim;
- macro series/event;
- hypothesis/research study;
- metric/feature request;
- gameplay/playbook version;
- personal trade idea or review.

Store links as references, not copied facts. One raw note can yield several candidates; one candidate can link to several episodes; a rejected interpretation does not delete its raw source.

## 6. Base pipeline without paid LLM

The application must still work if no LLM is configured:
- raw note and attachment capture;
- manual classification and tagging;
- keyword/ticker extraction as suggestions only;
- local full-text search over imported text;
- user-created links and hypotheses.

An optional LLM may propose structured extraction and summaries, but:
- must cite exact source spans/page/slide/sheet/URL;
- must return `null/unknown` when fields are absent;
- must distinguish source quote from inference;
- must not invent numbers, dates, owners or causal explanations;
- must not autonomously browse/ingest the whole web;
- must be reviewable and replaceable with manual workflow.

## 7. Search and retrieval

Local search indexes only items actually imported. Results show indexed corpus scope, publication dates and source pointers. Manual/assistant web research can be captured with publisher, URL, title, publication time, retrieval time and excerpt. Do not claim exhaustive internet coverage.

## 8. Example

Input: "Volume saham X naik, harga nggak gerak, broker A net buy. Beberapa hari kemudian breakout. Kayaknya ada yang nahan."

Possible proposals:
- observation: relative volume elevated (requires market data to verify; otherwise remains a user statement);
- observation: price range muted (requires OHLCV/timeframe);
- source claim: broker A net buy (requires Stockbit source capture, range, volume/value mode);
- hypothesis: elevated turnover with muted range precedes breakout in a defined issuer cohort;
- research question: which liquidity/regime conditions distinguish follow-through from failure?

The phrase "ada yang nahan" remains interpretation, not a measured fact or identified actor.
