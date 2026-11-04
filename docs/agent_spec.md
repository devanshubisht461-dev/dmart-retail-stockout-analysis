# Agent Specification

## 1. Purpose

The DMart Retail Operations Assistant helps the Regional Operations Director
investigate store performance, stock availability and customer feedback.

The assistant uses deterministic Python logic and SQLite queries.
It must not use an LLM to generate answers or make decisions.

## 2. Eight-Step Agent Pipeline

| Step | Stage | Description |
|---|---|---|
| 1 | Receive request | Accept the user's question. |
| 2 | Identify intent | Determine what business question is being asked. |
| 3 | Validate request | Check required store, date, product and other parameters. |
| 4 | Check permissions | Ensure requested fields are approved. |
| 5 | Select data source | Choose store_daily, inventory or feedback. |
| 6 | Execute query | Run a parameterised SQLite query. |
| 7 | Validate result | Check whether the query returned valid and sufficient data. |
| 8 | Respond or escalate | Return a factual answer, ask for clarification or refuse. |

## 3. Supported Intents and Routes

| Intent | Example question | Dataset / route |
|---|---|---|
| store_revenue | What is Pune's average revenue? | store_daily |
| store_comparison | Compare revenue across stores. | store_daily |
| target_performance | Did Pune meet its daily target? | store_daily |
| footfall_analysis | Which store has the highest footfall? | store_daily |
| stockout_analysis | Which store has the most stockout hours? | store_daily |
| inventory_status | Is Amul Milk below its reorder threshold? | inventory |
| product_stock | How many units of a product are available? | inventory |
| customer_feedback | What feedback themes appear in Pune? | feedback |
| rating_analysis | What is the average customer rating? | feedback |
| combined_analysis | Compare Pune's revenue and stockout hours. | store_daily |

## 4. Approved Field List

The assistant may report only these 11 approved fields.

### store_daily
- revenue_inr
- daily_target_inr
- footfall
- stockout_hours
- discount_pct
- achievement_pct
- performance_band

### inventory
- stock_units
- reorder_threshold

### feedback
- rating
- theme

### Key fields

The following fields may be used for filtering, grouping and joining,
but must never be reported as standalone business metrics:
- store
- store_id
- date
- product
- product_id

### Prohibited fields

The assistant must not report:
- marketing_spend_inr
- feedback_id
- revenue_per_footfall
- marketing_efficiency
- shortfall_inr
- weekend_flag

The assistant must also avoid reporting any unapproved derived metric.

## 5. Handling Rules

1. Use only the supplied datasets and approved fields.
2. Never invent values or fill missing business information with guesses.
3. Treat missing feedback ratings as missing, not zero.
4. Use parameterised SQL queries.
5. Validate store names, dates and product names before querying.
6. Explain when no matching records are found.
7. Clearly distinguish observations from possible explanations.
8. Do not claim that correlation proves causation.
9. Do not expose prohibited fields or internal identifiers as reported metrics.
10. Use the cleaned daily dataset for performance analysis and preserve the raw data.

## 6. Confidence Threshold

The minimum intent confidence threshold is 0.70.

| Confidence | Action |
|---|---|
| 0.70 to 1.00 | Continue with the selected intent after validation. |
| Below 0.70 | Ask the user to clarify the question. |

For a deterministic implementation, confidence may be represented by
the strength of matched intent rules. It must not be presented as a
statistically calibrated probability unless it has been evaluated as such.

## 7. Status Table

| Status | Meaning |
|---|---|
| RECEIVED | Request has been received. |
| CLASSIFIED | An intent has been identified. |
| NEEDS_CLARIFICATION | Required information is missing or ambiguous. |
| VALIDATED | Intent, fields and parameters are valid. |
| RUNNING | Database query is executing. |
| SUCCESS | A valid result has been produced. |
| NO_DATA | Query completed but no matching records were found. |
| REFUSED | Request is outside the approved scope. |
| ERROR | An unexpected processing error occurred. |
| ESCALATED | Request needs human review. |

## 8. Retry and Escalation Rules

- If a query fails because of a temporary database error, retry once.
- Do not retry invalid SQL or an invalid user request without correction.
- If the required dataset or field is unavailable, explain the limitation.
- If the request requires an unapproved field, refuse that part.
- If results conflict or cannot be validated, escalate for human review.
- Never fabricate a result to avoid an error.

## 9. Prohibited Actions

The assistant must not:

1. Invent sales, inventory or feedback figures.
2. Modify source datasets without authorisation.
3. Recommend purchases or replenishment orders as if approved.
4. Claim stockouts caused a revenue decline without sufficient evidence.
5. Reveal prohibited fields or customer-identifying feedback IDs.
6. Execute arbitrary SQL supplied by a user.
7. Make operational decisions on behalf of the Regional Operations Director.
8. Present missing values as zero.
9. Use external or unapproved data as though it came from the supplied datasets.

## 10. Should Answer vs Should Refuse

| Request | Expected behaviour |
|---|---|
| What was Pune's average revenue? | SHOULD ANSWER using store_daily. |
| Which store had the highest stockout hours? | SHOULD ANSWER using store_daily. |
| Is Amul Milk below its reorder threshold in Pune? | SHOULD ANSWER using inventory. |
| What feedback themes appear in Bengaluru? | SHOULD ANSWER using feedback. |
| Tell me the marketing spend for Pune. | SHOULD REFUSE because the field is not approved. |
| Show me feedback IDs. | SHOULD REFUSE because feedback_id is prohibited. |
| What will revenue be next year? | SHOULD REFUSE or explain that forecasting is outside the defined scope. |
| Place an inventory purchase order. | SHOULD REFUSE because the assistant cannot execute purchases. |
| Prove that stockouts caused Pune's low revenue. | SHOULD REFUSE the causal claim and explain the evidence limitation. |

## 11. Success Criteria

The agent is successful when it:
- Correctly identifies supported intents.
- Uses the correct dataset and approved fields.
- Produces accurate and reproducible database results.
- Requests clarification when information is insufficient.
- Refuses prohibited requests.
- Clearly communicates missing data and limitations.
- Does not invent results or claim unsupported causation.

## 12. Completion States

Every request must finish in one of these states:
- SUCCESS
- NO_DATA
- NEEDS_CLARIFICATION
- REFUSED
- ESCALATED
- ERROR

## 13. Test Categories

The agent must be tested for:
- Correct intent identification.
- Correct dataset selection.
- Correct calculations.
- Store and date filtering.
- Product and inventory lookups.
- Missing data handling.
- Empty query results.
- Prohibited field requests.
- Ambiguous questions and clarification.
- Database errors and retry behaviour.
- Unsupported actions and escalation.