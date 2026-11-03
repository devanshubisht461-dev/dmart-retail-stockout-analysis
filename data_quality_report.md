# DATA QUALITY REPORT

## 1. Dataset Overview

### Store Daily
- Rows: 453
- Columns: 9
- Missing values: 4
- Duplicate rows: 3

### Inventory
- Rows: 40
- Columns: 6
- Missing values: 0
- Duplicate rows: 0

### Feedback
- Rows: 200
- Columns: 6
- Missing values: 12
- Duplicate rows: 0


## 2. Store Daily Column Data Types

date                    object
store_id                object
store                   object
footfall               float64
revenue_inr            float64
daily_target_inr         int64
marketing_spend_inr      int64
stockout_hours         float64
discount_pct           float64


## 3. Footfall Analysis

- Raw minimum: -150.0
- Raw maximum: 9800.0
- Cleaned minimum: 1293.0
- Cleaned maximum: 2986.0


## 4. Store-wise IQR Boundaries


### Ahmedabad
- Q1: 1846.00
- Q3: 2018.50
- IQR: 172.50
- Lower bound: 1587.25
- Upper bound: 2277.25


### Bengaluru
- Q1: 1952.00
- Q3: 2211.00
- IQR: 259.00
- Lower bound: 1563.50
- Upper bound: 2599.50


### Hyderabad
- Q1: 2051.00
- Q3: 2351.00
- IQR: 300.00
- Lower bound: 1601.00
- Upper bound: 2801.00


### Mumbai
- Q1: 2247.25
- Q3: 2541.75
- IQR: 294.50
- Lower bound: 1805.50
- Upper bound: 2983.50


### Pune
- Q1: 1434.00
- Q3: 1593.50
- IQR: 159.50
- Lower bound: 1194.75
- Upper bound: 1832.75


## 5. Cleaning Summary

- Three duplicate rows removed.
- One negative footfall converted to missing.
- Two extreme footfall values capped.
- Five missing/invalid footfall values imputed.
- Twelve missing feedback ratings preserved.
