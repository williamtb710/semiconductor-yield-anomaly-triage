# Data-quality report

Generated: 2026-08-22T18:32:06.161675+00:00

## Provenance

- Official dataset page: https://archive.ics.uci.edu/dataset/179/secom
- Official archive: https://archive.ics.uci.edu/static/public/179/secom.zip
- Archive SHA-256: `eea568baf3c2229096d7d294cf0b096b5502bd96d92c0b80a65b84714059be8e`
- Source labels: `-1 = pass`, `1 = fail`

## Structural findings

- Measurement matrix: **1,567 rows × 590 raw numeric columns**.
- Source metadata lists 591 features, while the raw numeric file parses to 590 columns. This discrepancy is preserved and documented.
- Outcome: **1,463 passes** and **104 failures** (6.64% fail).
- Timestamps: **2008-07-19 11:55:00** through **2008-10-17 06:07:00**, in nondecreasing file order.
- Repeated timestamps after first occurrence: **33**. A timestamp collision alone does not prove duplicate production entities.

## Missingness and low-information columns

- Missing cells: **41,951 / 924,530 (4.54%)**.
- Features with any missing value: **538**.
- Features over 20% missing: **32**.
- Features over 50% missing: **28**.
- Constant or empty features when inspected descriptively: **116**.
- Exact duplicate measurement rows: **0**.
- Median row missingness: **4.07%**; maximum: **25.76%**.

## Implications for the next checkpoint

1. A plain accuracy result can hide complete failure blindness because the pass class is 93.36% of the data.
2. Chronological validation is possible because timestamps parse and are nondecreasing, but the failure count in each proposed split must be checked before locking it.
3. Missingness handling, constant filtering, scaling, feature selection, calibration, and threshold tuning must be fit using training data only.
4. Full-data feature-quality rankings above are descriptive and must not become a pre-split feature-selection list.
5. Anonymous-variable coefficients or importances will be described as predictive associations, never physical root causes.

No model was fit in this checkpoint.
