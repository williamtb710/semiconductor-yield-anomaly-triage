# Semiconductor Process Yield & Anomaly Triage

> **Status:** Checkpoint 1 complete — project scaffold and data-quality audit. Modeling has intentionally not started.

## Honest elevator pitch

This is a compact manufacturing-analytics case study built on the public **UCI SECOM** dataset donated in 2008. It asks whether anonymous process measurements contain **predictive associations** with a rare pass/fail outcome, while treating the real analytical hazards—small sample size, many variables, missing values, class imbalance, time ordering, leakage, threshold tradeoffs, and uncertainty—as first-class problems.

It is **not** a representation of a current production fab, a physical process model, or a causal root-cause system. The variables are anonymous, so a high coefficient or importance score can identify a variable worth investigating but cannot identify a physical mechanism.

Official source: [UCI SECOM dataset](https://archive.ics.uci.edu/dataset/179/secom)  
Dataset DOI: [10.24432/C54305](https://doi.org/10.24432/C54305)

## Why this project exists

The goal is a technically honest conference conversation piece for SEMICON Taiwan—not a leaderboard exercise. A useful result can be negative or uncertain if the analysis makes the operating tradeoffs and data limitations clear.

## Current checkpoint

Run [`notebooks/01_data_audit.ipynb`](notebooks/01_data_audit.ipynb). It:

- downloads the official UCI archive and verifies its SHA-256 hash;
- parses the measurement matrix, labels, and timestamps;
- verifies row alignment, label encoding, time ordering, missingness, constants, and duplicates;
- surfaces a source-metadata discrepancy: UCI describes 591 features, while the raw `secom.data` file parses to 590 numeric columns;
- writes a reproducible data-quality report, feature-quality table, manifest, and two diagnostic figures;
- makes **no modeling or feature-selection decisions**.

Expected core output:

```text
Measurement matrix: (1567, 590)
Passes (-1): 1463
Failures (1): 104
Failure rate: 6.64%
Timestamp range: 2008-07-19 11:55:00 to 2008-10-17 06:07:00
Missing cells: 41,951 / 924,530 (4.54%)
```

## Master plan

| Checkpoint | Scope | Status |
|---|---|---|
| 1 | Charter, cloud workflow, repository scaffold, official-source data audit | **Ready to run** |
| 2 | Chronological and stratified-random validation designs; leakage discussion | Blocked on Checkpoint 1 confirmation |
| 3 | Always-pass baseline and training-only imputation/scaling/regularized logistic regression | Planned |
| 4 | One justified nonlinear contrast, PR-focused evaluation, operating thresholds, uncertainty | Planned |
| 5 | Stable interpretability, ranked triage view, drift/monitoring demonstration | Planned |
| 6 | One-page executive summary, three-slide talk track, five conference questions, 60-second demo | Planned |

The detailed charter, MVP cutline, and four-day schedule are in [`docs/PROJECT_CHARTER.md`](docs/PROJECT_CHARTER.md). Exact browser setup instructions are in [`docs/CLOUD_SETUP.md`](docs/CLOUD_SETUP.md).

## Repository layout

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── docs/
│   ├── PROJECT_CHARTER.md
│   └── CLOUD_SETUP.md
├── notebooks/
│   └── 01_data_audit.ipynb
├── data/
│   ├── README.md
│   ├── raw/                 # generated locally; ignored by Git
│   └── processed/           # generated locally; ignored by Git
└── reports/
    ├── data_quality_report.md
    ├── data_quality_summary.csv
    ├── feature_quality.csv
    ├── checkpoint_1_manifest.json
    └── figures/
        ├── class_balance.png
        └── top_feature_missingness.png
```

Later checkpoints will add `02_modeling_evaluation.ipynb`, `03_triage_monitoring.ipynb`, final figures, and presentation artifacts. An `app/` folder will be added only if the core analysis is solid by the end of Day 3.

## Reproduce Checkpoint 1

### Google Colab

Open the notebook from this repository in Colab, choose a standard CPU runtime, and select **Runtime → Run all**. No GPU, API key, Drive mount, or secret is required.

### Local Jupyter

Use Python 3.12, then:

```bash
python -m venv .venv
source .venv/bin/activate        # Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
jupyter lab
```

Open `notebooks/01_data_audit.ipynb` and run all cells.

## Analytical guardrails

1. The failure class is `1`; pass is `-1` in the source and will become `0` only in an explicitly named binary target.
2. Time is metadata for splitting and monitoring unless a later experiment explicitly justifies using time-derived predictors.
3. Imputers, scalers, feature filters, selection, calibration, and threshold tuning are fit on training data only.
4. Full-dataset data-quality summaries are descriptive; they will not be used to preselect features before splitting.
5. Accuracy is not a primary metric. Evaluation will emphasize precision-recall behavior and an explicit review budget or false-alarm constraint.
6. Anonymous feature importance means predictive association, not root cause or controllable process leverage.
7. Chronological validation is the deployment-like primary view if class counts remain usable; stratified random validation is a sensitivity comparison, not a substitute for time-aware validation.
8. Simulated drift will be labeled simulated. Observed time-bucket changes will be labeled descriptive, not causal.

## Data license and attribution

The dataset is licensed by UCI under CC BY 4.0. Cite:

> McCann, M. & Johnston, A. (2008). SECOM [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C54305

Raw data are downloaded from UCI at runtime and are not committed to this repository.
