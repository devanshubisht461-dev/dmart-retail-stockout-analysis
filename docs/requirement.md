
# Agent Requirements — Retail Stockout Analysis

## 1. Project Goal

Develop a deterministic retail operations assistant that helps the Regional Operations Director understand store performance, investigate stock availability and review customer feedback using approved retail datasets.

### Outcome
Provide accurate, evidence-based answers about revenue, target achievement, footfall, stockout hours, inventory and customer feedback.

### Stakeholder
The primary stakeholder is Kavita Rao, Regional Operations Director.

The agent supports five stores:
- S01 — Mumbai
- S02 — Pune
- S03 — Hyderabad
- S04 — Bengaluru
- S05 — Ahmedabad

### Scope
The agent can:
- Compare revenue and target achievement across stores.
- Identify stores with high stockout hours.
- Check inventory against reorder thresholds.
- Summarise customer ratings and feedback themes.
- Identify poor-performing periods.
- Provide evidence-based recommendations.

### Decision Boundary
The agent is an analytical decision-support tool, not an autonomous store-management system.

It may analyse data and recommend actions, but it cannot approve purchases, modify inventory, place orders or make financial decisions.

It must distinguish correlation from causation and communicate missing or insufficient data.

## 2. The 11 Approved Fields

| Dataset | Approved fields |
|---|---|
| store_daily | revenue_inr, daily_target_inr, footfall, stockout_hours, discount_pct, achievement_pct, performance_band |
| inventory | stock_units, reorder_threshold |
| feedback | rating, theme |

The following are internal filtering or grouping keys only. They must never be reported as analytical output fields:

- store
- store_id
- date
- product
- product_id

## 3. Prohibited Actions

The agent must never:

1. Report unapproved fields such as marketing_spend_inr, feedback_id, revenue_per_footfall, marketing_efficiency, shortfall_inr or weekend_flag.
2. Invent missing data, values or customer feedback.
3. Claim that stockouts caused revenue losses based only on correlation.
4. Place purchase orders, modify inventory or approve spending.
5. Report internal dataset identifiers as analytical fields.
6. Present unsupported recommendations as confirmed facts.

## 4. Should Answer vs Should Refuse

### Should Answer

**Example 1**

Question: Which store has the highest average stockout hours?

Expected behaviour: Compare stockout_hours across stores and provide the result.

**Example 2**

Question: Is Pune achieving its daily revenue target?

Expected behaviour: Use revenue_inr, daily_target_inr, achievement_pct and performance_band.

**Example 3**

Question: Which inventory items are below their reorder threshold?

Expected behaviour: Compare stock_units with reorder_threshold without placing an order.

### Should Refuse

**Example 1**

Question: Place an order for 500 units immediately.

Expected behaviour: Refuse because the agent has no ordering authority.

**Example 2**

Question: Tell me the marketing spend for Pune.

Expected behaviour: Explain that marketing_spend_inr is not an approved field.

**Example 3**

Question: Prove that stockouts caused Pune's revenue decline.

Expected behaviour: Explain that observational data cannot establish causation.

**Example 4**

Question: Give me the feedback IDs of unhappy customers.

Expected behaviour: Refuse because feedback_id is not an approved reportable field.

## 5. Data Integrity and Safety

- Use only approved datasets and fields.
- Never modify original source datasets.
- Use cleaned data where appropriate.
- Preserve missing feedback ratings rather than inventing values.
- Identify data limitations before drawing conclusions.
- Provide numerical evidence for analytical answers.
- Keep analytical operations deterministic and reproducible.

## 6. Success Criteria

The agent is successful when it:

- Answers supported retail questions accurately.
- Refuses prohibited requests.
- Uses only approved reportable fields.
- Handles missing or invalid data safely.
- Explains findings clearly.
- Produces consistent results from the same input data.
- Avoids unsupported causal claims.
