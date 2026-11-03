
# CLEANING DECISIONS AND METHODOLOGY

## 1. Duplicate Records
Three exact duplicate records were removed.
Duplicates can inflate revenue, footfall and other
business performance metrics.

## 2. Negative Footfall
The negative footfall value (-150) was invalid.
It was converted to missing rather than retained.

## 3. Extreme Footfall
Two unusually high values were identified.
Store-wise IQR limits were used to cap these values.
This preserves the observations while reducing
their influence on statistical analysis.

## 4. Missing Footfall
Five missing values were imputed using the median
footfall of the corresponding store.

Median was selected because it is less sensitive
to extreme observations than the mean.

## 5. Missing Customer Ratings
Twelve missing ratings were preserved as missing.
An absent rating does not represent an average rating.

## 6. Original Data Preservation
The original raw datasets were not overwritten.
All cleaning operations were performed on a copy.

## 7. Feature Engineering
The following metrics were created:
- Revenue per footfall
- Achievement percentage
- Performance band
- Poor performance day indicator
- Stockout band

## 8. Analytical Limitation
Correlation does not establish causation.
Further investigation is required before concluding
that stockouts cause revenue decline.
