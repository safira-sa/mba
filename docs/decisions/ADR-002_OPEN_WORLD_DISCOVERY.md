# ADR-002 — Open-World Opportunity Discovery Is a First-Class Product Requirement

- **Status:** Proposed for user approval before implementation.
- **Date:** 13 November 2026.
- **Related:** ADR-001 Market Episodes & Response Learning Are the Core.

## Context

The original product goal is broader than answering the user's current ticker or research question. MBI must also find potentially relevant developments, issuers, sectors, clusters, conglomerate/business relationships and unusual behavior that may not yet be on the user's radar. Examples include a new policy creating a possible industry transmission path, an issuer's unexplained activity, a post-IPO cohort behavior or a cross-sector liquidity shift.

## Decision

Make Open-World Opportunity Discovery a first-class front door to Market Episode & Response Learning. Require two complementary routes:

1. Top-down catalyst-led discovery: external context → mechanism → sector/issuer exposure → market validation → historical analogues.
2. Bottom-up anomaly-led discovery: broad issuer/market observations → issuer/cluster/relationship expansion → external-context search → historical analogues.

Add network expansion, issuer lifecycle/post-IPO and cross-sectional pattern routes. Record exact scan universe and field coverage. Candidates remain research leads until formally scoped into an episode/study. The system produces evidence briefs and counter-evidence, not automatic trade instructions or conviction scores.

## Consequences

- A discovery run and coverage snapshot are required logical concepts.
- Candidate, exposure path, evidence links, user review, monitoring conditions and immutable research brief concepts are required before physical schema design.
- Broad discovery can run only for data/source layers actually available and permitted.
- No claim of complete IDX coverage or unknown-opportunity omniscience.
- Discovery quality is evaluated separately from candidate outcome and user acceptance.
- Conviction support must expose evidence on both sides and preserve the user's own judgement.

## Rejected alternatives

1. **Watchlist-only scanning:** fails to expand the user's radar.
2. **News keyword → ticker mapping:** confuses mention with economic exposure/materiality.
3. **One black-box opportunity score:** conflates novelty, source quality, market confirmation, expected return and uncertainty.
4. **Automatic buy/sell recommendations:** exceeds the system's evidence and user-support role.
