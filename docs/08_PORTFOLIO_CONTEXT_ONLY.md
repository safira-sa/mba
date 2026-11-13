# 08 — Portfolio as Supporting Context (Not the Main Product)

## 1. Product priority

Portfolio functionality is deliberately secondary. MBI is not primarily an accounting, tax, budgeting or broker reconciliation application. Portfolio data is included mainly to understand the user's decision context, position sizing, risk concentration and execution behavior.

## 2. Minimum useful model

Portfolio is **manual-first and local-only** in the current scope. BIONS exports, brokerage credentials and broker integrations are not required for Gate -1 or the initial MBI release. The user may create local account aliases and enter opening position snapshots and subsequent transactions manually. An opening snapshot is `USER_ENTERED_STARTING_DATA`; it cannot imply prior fills, tax lots, decision history or historical realized P&L that were not provided.

Keep three concepts distinct:
- **Local account alias:** a user-defined label for a portfolio bucket; do not store broker credentials, login IDs or account identifiers.
- **Portfolio view:** a user-defined way to aggregate accounts for analysis.
- **Strategy sleeve:** a virtual attribution label (e.g. core, swing, event-driven) that does not create a separate brokerage asset.

The same local account alias can appear in more than one view. Do not add overlapping views together. A single actual fill is stored once and referenced by views/sleeves.

## 3. Learning-relevant portfolio fields

Where reliably available, use:
- account and portfolio-view reference;
- position size and value at decision time;
- planned risk and actual risk proxy;
- issuer/sector/cluster exposure;
- cash/buying power timestamp (only if the broker report provides it);
- staged entry/add/trim behavior;
- average cost or lot basis with named method;
- realized/unrealized outcome and fees basis;
- impact of correlated positions on drawdown/concentration.

The main questions are behavioral: does the user oversize correlated names, add when thesis quality deteriorates, average down successfully only in specific regimes, cut winners early, or take more risk than planned?

## 4. Multi-account rules

- Preserve each local account alias's manually entered events separately.
- Consolidated views aggregate positions only through explicit membership rules.
- Internal cash/security transfers between accounts are not external deposits/withdrawals and must not duplicate investment performance.
- Strategy sleeves are virtual tags/allocations, not extra assets.
- If transfer matching/cost basis is unresolved, flag it; do not silently treat as a buy/sell. Manual-entered provenance and timestamps must be retained.
- Same ticker in two accounts remains two account-level positions and one consolidated exposure view.

## 5. Average cost and sizing scope

If implemented, cost-basis method is configured and labelled per account (e.g. weighted average or FIFO). Actual fills and fees are source records; estimates are clearly labelled. Corporate actions require explicit handling or reconciliation hold.

Position Planner is a scenario tool only. It may compare no-add, add, staged entry, average-up/down, partial exit and full exit. It must show the constraints that bind: risk, cash, issuer/cluster cap and liquidity. It must not claim a uniquely optimal lot without an explicit objective and validated inputs.

Risk-to-stop is not guaranteed maximum loss: gap, suspension, price limits, spread and execution failure can worsen outcome. Stale cash/equity, unknown fee/lot/tick rules or missing exit liquidity block definitive size.

## 6. Deferred features

BIONS import, broker statement ingestion, full bookkeeping, tax reporting, household budgeting, broker login, automatic order routing and detailed accounting statements are outside the current scope unless separately approved. Do not request the user's BIONS exports or personal account data for product development.
