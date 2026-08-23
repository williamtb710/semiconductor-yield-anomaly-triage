# Semiconductor Process Yield & Anomaly Triage

A compact case study in rare-event manufacturing analytics using the public UCI SECOM dataset. The project tests whether anonymous process measurements can support a constrained engineering-review queue while making the consequences of missing data, class imbalance, time ordering, leakage, threshold choice, and uncertainty explicit.

This is not a benchmark for a current semiconductor fab. The dataset was donated in 2008, variables are anonymous, and feature rankings cannot establish physical root causes.

## Executive result

The primary evaluation trains on the first 80% of timestamp-ordered records and holds out the final 20%: 314 records containing 17 failures.

- A class-weighted regularized logistic model achieved **0.206 average precision** against a **0.054 failure prevalence**.
- Reviewing the highest-scored 10% of records captured **5 of 17 failures**: **29.4% recall**, **15.6% precision**, and **2.9× lift** over prevalence.
- The stratified-bootstrap 95% interval for chronological logistic average precision was approximately **0.108–0.424**, reflecting substantial uncertainty.
- A constrained random forest performed worse chronologically (**0.075 AP**) but better under a stratified-random split (**0.189 AP versus logistic 0.124**).

The reversal in model ranking is the central finding: performance depends materially on the validation assumption. A random split alone would have produced a more favorable but less deployment-like story for the nonlinear model.

## Analytical framing

The operating question is not “Which classifier wins?” It is:

> If engineering capacity permits reviewing 10% of production records, how many rare failures can a score concentrate in that queue, and how stable is that conclusion over time and resampling?

Model outputs are used as uncalibrated ranking scores. No probability calibration claim is made.

## Dataset

[UCI SECOM](https://archive.ics.uci.edu/dataset/179/secom) contains 1,567 production examples, pass/fail labels, test-point timestamps, and missing values. The official metadata lists 591 features, while the current raw `secom.data` file parses to **590 numeric columns**. The discrepancy is retained as a source-quality finding.

Observed audit summary:

| Quantity | Value |
|---|---:|
| Records | 1,567 |
| Passes | 1,463 |
| Failures | 104 (6.64%) |
| Raw numeric columns | 590 |
| Missing cells | 41,951 (4.54%) |
| Constant or empty columns, descriptively | 116 |
| Timestamp range | July 19–October 17, 2008 |

Raw files are downloaded from UCI at runtime and verified against a pinned SHA-256 digest.

## Method

### Validation

1. **Chronological holdout — primary:** train on the first 80% of timestamp-ordered records and evaluate on the final 20%.
2. **Stratified-random holdout — sensitivity:** fixed-seed 80/20 split preserving failure prevalence while mixing time.

The random split estimates performance under exchangeability; it is not a substitute for future-period evaluation.

### Leakage controls

For each split, the following operations are fit on training data only inside a scikit-learn pipeline:

- exclusion of constant and more-than-50%-missing columns;
- median imputation;
- missingness indicators;
- standardization for logistic regression;
- model fitting.

The 10% review threshold is capacity-defined rather than optimized against held-out labels.

### Models

- **Always-pass reference:** 93%+ accuracy with zero failure recall, demonstrating why accuracy is inadequate.
- **Regularized logistic regression:** interpretable linear anchor with class weighting.
- **Constrained random forest:** one nonlinear contrast with fixed depth and leaf-size controls.

No broad hyperparameter sweep was performed.

### Evaluation

- Precision-recall curves and average precision
- Exact confusion-matrix counts
- Precision, recall, false-positive rate, and lift at the 10% review budget
- ROC-AUC and accuracy as secondary context
- Stratified bootstrap intervals
- Training-bootstrap coefficient stability

## Interpretation and triage

The chronological logistic model produces:

- standardized coefficient tables;
- top-15 selection frequency across training bootstraps;
- a ranked holdout triage table;
- record-level positive contribution summaries for anonymous transformed inputs.

These outputs explain model behavior and identify variables for follow-up. They do not identify physical mechanisms, controllable parameters, or causal leverage.

## Monitoring view

The case study includes:

- descriptive weekly failure prevalence and row missingness;
- train-to-test standardized mean and missingness shifts;
- a clearly labeled simulated one-standard-deviation shift in `feature_059` to demonstrate how an input change can move the score distribution and review volume.

The simulation is a monitoring demonstration, not evidence of an actual process intervention.

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── docs/
│   ├── PROJECT_CHARTER.md       # analytical scope and evaluation plan
│   └── CLOUD_SETUP.md           # reproducibility and leakage controls
├── notebooks/
│   ├── 01_data_audit.ipynb
│   └── 02_modeling_triage.ipynb
├── data/
│   └── README.md                # provenance policy; raw/processed created at runtime
└── reports/
    ├── analysis_summary.md
    ├── semiconductor_yield_triage_executive_summary.pdf
    ├── CONFERENCE_TALK_TRACK.md
    ├── presentation/
    │   └── semiconductor_yield_triage_talk.pptx
    ├── model_results.csv
    ├── bootstrap_intervals.csv
    ├── logistic_coefficients.csv
    ├── coefficient_stability.csv
    ├── triage_ranked.csv
    ├── drift_summary.csv
    └── figures/
```

Conference-ready outputs:

- [One-page executive summary](reports/semiconductor_yield_triage_executive_summary.pdf)
- [Three-slide talk](reports/presentation/semiconductor_yield_triage_talk.pptx)
- [60-second script and SEMICON discussion questions](reports/CONFERENCE_TALK_TRACK.md)

## Reproduce

Use a standard CPU runtime. Run:

1. `notebooks/01_data_audit.ipynb`
2. `notebooks/02_modeling_triage.ipynb`

For a local environment:

```bash
python -m venv .venv
source .venv/bin/activate        # Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
jupyter lab
```

No secret, API key, GPU, or manually downloaded dataset is required.

## Limitations

- Small sample size and only 17 failures in the chronological holdout produce wide uncertainty.
- The three-month time range is too short to represent long-term process drift.
- Observations may not be independent; no lot, product, tool, chamber, recipe, or maintenance identifiers are provided.
- Anonymous measurements prevent physical interpretation and causal validation.
- Class-weighted scores are not calibrated failure probabilities.
- Results do not establish performance on another fab, product, process generation, or current manufacturing environment.

A credible deployment study would require newer labeled cohorts, process semantics, group-aware validation, a predefined engineering workflow, calibration data, and prospective monitoring.

## Attribution

McCann, M. & Johnston, A. (2008). *SECOM* [Dataset]. UCI Machine Learning Repository. [https://doi.org/10.24432/C54305](https://doi.org/10.24432/C54305). Licensed under CC BY 4.0.
