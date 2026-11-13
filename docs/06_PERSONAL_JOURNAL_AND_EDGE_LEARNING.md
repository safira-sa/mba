# 06 — Personal Decision Journal & Edge Learning

## 1. Purpose

The journal is a learning dataset, not a win/loss diary. It records why the user considered a trade, what was known at that moment, what decision was made, how it was executed and what happened afterward.

Personal learning must remain separate from market learning. MBI first learns what the market did across the covered universe, including Setup Instances never noticed or traded by the user; it then evaluates how the user selected and handled a subset of those opportunities in comparable conditions. Setup-instance requirements are defined in Doc 18.

## 2. Decision types

Record all meaningful decisions:
- `ENTERED`;
- `WATCHED_NO_ENTRY`;
- `REJECTED`;
- `MISSED_OR_UNAVAILABLE`;
- `EXITED_OR_REDUCED`;
- `NO_TRADE_BY_RULE`;
- `UNCLASSIFIED_DECISION`.

No-trade and missed-opportunity outcomes are not actual portfolio P&L. Keep them in separate analytic populations.

## 3. Immutable decision-time snapshot

At decision time, capture what was available then:
- ticker/security ID, date/time and intended horizon;
- original thesis in user's words;
- strategy/gameplay/playbook version(s), if any;
- linked `setup_instance_id` where the idea corresponds to a detected/researched setup (optional; do not force-fit discretionary trades);
- setup/trigger/invalidation/target and planned entry range;
- issuer Daily Character and rolling Stock Personality versions;
- market/sector/cluster regime;
- available macro/event/disclosure information;
- observed price/volume/turnover/flow/orderbook fields and exact source availability;
- planned size/risk and portfolio context, if supplied;
- confidence as the user's subjective confidence, separate from model probability;
- attachments and source IDs.

The snapshot is immutable. Later changes create timestamped revisions; do not replace the original thesis with hindsight.

## 4. Execution record

For actual entries/exits, record fills separately from ideas:
- account reference (optional for early MVP);
- execution timestamp, side, quantity, price;
- partial fills;
- fees/taxes if known;
- source (`USER_ENTERED` manual entry in the current scope; no broker statement import is required or assumed);
- link to idea and decision snapshot.

An order or chat plan is not a fill. An actual fill must not be invented from chart candles.

## 5. Outcome model

Define outcome horizons before evaluating an idea, e.g. 1/3/5/10 IDX sessions or a user-defined horizon. Calculate where input resolution supports it:
- net realized/unrealized P&L for actual trades;
- R-multiple against the planned risk basis;
- MAE and MFE from the decision/entry reference;
- target/invalidation sequence where intraday order is known;
- time-to-target/invalidation;
- follow-through, reversal and opportunity cost;
- planned vs actual entry/exit deviation.

If daily OHLC touches both target and stop in the same bar and no finer data resolves order, outcome is `AMBIGUOUS_INTRABAR`. If historical data is missing, label the horizon incomplete, not zero return.

## 6. Connect personal decisions to market-wide Setup Instances

When a user's idea or trade corresponds to an MBI Setup Instance, link to that immutable instance and its as-of context rather than copying the market feature vector into the journal. If no Setup Instance exists, retain the decision as discretionary/unclassified and allow later research linkage without rewriting the original decision snapshot.

This permits three separate populations:
- all eligible market Setup Instances, including user-unnoticed cases;
- instances the user noticed, watched, rejected or missed;
- instances actually traded, with real fills and execution.

Do not infer that the user noticed a setup merely because a later chart shows it. Store explicit user observation/decision time.

## 7. Separate the sources of edge

MBI should report at least:
1. **Opportunity discovery/selection:** Did the user notice/select a candidate that had evidence of an opportunity?
2. **Setup quality:** Did the setup condition historically have favorable outcomes in the defined cohort?
3. **Execution quality:** Was entry/exit/size consistent with the plan and historical behavior?
4. **Risk management:** Did actual risk/portfolio exposure remain within the intended constraints?
5. **Outcome:** What actually happened, independent of whether the decision process was good?

A profitable outcome does not prove a good decision; a losing outcome does not by itself prove a bad process.

## 8. Useful analyses

- expectancy, payoff distribution, win/loss rate and profit factor;
- R-multiple, MAE/MFE and capture ratio;
- entry timing and distance to invalidation;
- performance by gameplay/setup version, issuer character, regime, event, cluster, liquidity band, entry-distance band and supported fundamental/ownership cohorts;
- holding duration, scaling/averaging behavior and exit decisions;
- planned vs actual execution;
- no-trade/rejected/missed opportunity follow-through;
- concentration and cross-account exposure as contextual risk.

Each report displays sample size, period, missing fields, fees basis and cohort definition. Do not draw conclusions from tiny or selected samples.

## 9. Bias controls

- Avoid hindsight editing of thesis/context.
- Avoid only journaling trades that were executed; include watch/reject/no-trade cases.
- Keep the market-wide Setup Instance universe separate from the user's selected trades to identify selection bias. Do not treat multiple related issuers responding to one event as independent opportunity samples.
- Do not compare different horizons or strategy definitions as if identical.
- Do not assume fills from a theoretical price or candle touch.
- Do not expose other people's account or identity data.
