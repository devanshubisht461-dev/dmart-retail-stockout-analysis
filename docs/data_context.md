# Data Context

## 1. Purpose

This document describes the three datasets used in the DMart Retail Stockout Analysis project. It explains their structure, data quality, relationships and the business questions they can support.

## 2. Dataset: store_daily

- Raw row count: 453
- Date range: 2026-04-01 to 2026-06-29
- One row represents a store's daily operating performance.

| Column | Data type |
|---|---|
| date | object |
| store_id | object |
| store | object |
| footfall | float64 |
| revenue_inr | float64 |
| daily_target_inr | int64 |
| marketing_spend_inr | int64 |
| stockout_hours | float64 |
| discount_pct | float64 |

### Data quality facts
- 4 missing footfall values.
- 3 exact duplicate rows.
- 1 negative footfall value.
- 2 extreme high footfall values.
- Other listed columns have no missing values.

### Questions this dataset supports
- What is each store's average daily revenue?
- How does revenue compare with the daily target?
- How many stockout hours does each store experience?
- How does footfall relate to revenue?
- Does store performance vary across the date range?

## 3. Dataset: inventory

- Row count: 40
- 5 stores and 8 products per store.
- One row represents the inventory position of a product at a store.

| Column | Data type |
|---|---|
| store_id | object |
| store | object |
| product_id | object |
| product | object |
| stock_units | int64 |
| reorder_threshold | int64 |

### Data quality facts
- No missing values.
- No duplicate rows.

### Questions this dataset supports
- Which products are below their reorder threshold?
- What is the available stock for a particular product at a store?
- Which stores may need inventory replenishment?

## 4. Dataset: feedback

- Row count: 200
- One row represents one customer feedback record.

| Column | Data type |
|---|---|
| feedback_id | object |
| store_id | object |
| store | object |
| date | object |
| rating | float64 |
| theme | object |

### Data quality facts
- 12 missing rating values.
- No missing values in the other columns.
- No duplicate rows.

### Questions this dataset supports
- What customer feedback themes appear at each store?
- What is the average available customer rating?
- Which feedback themes may indicate customer concerns?

## 5. Relationships Between Datasets

- store_daily and inventory are connected through store_id.
- store_daily and feedback can be related through store_id and date.
- inventory records are identified by store_id and product_id.
- feedback records are identified by feedback_id.

These datasets provide store-level operating performance, product-level inventory positions and customer feedback.

There is no direct transaction-level product sales table in the supplied data. Therefore, the datasets cannot establish which specific products caused daily revenue changes.

## 6. Important Limitations

- Missing feedback ratings must not be treated as zero.
- Correlation between stockout hours and revenue does not prove causation.
- Daily store-level data does not identify the exact product responsible for a revenue change.
- Inventory represents the supplied inventory snapshot, not a complete daily inventory history.
- Findings should be limited to the fields and records actually supplied.