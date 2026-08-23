# Reproducibility and Execution Environment

## Execution model

The analysis is designed for a standard CPU runtime in Google Colab or a local Python 3.12 environment. GPU acceleration is unnecessary at this dataset size.

Each notebook is self-contained:

- raw files are downloaded from the official UCI static archive;
- the archive SHA-256 digest is verified before parsing;
- generated data and figures are written beneath `data/` and `reports/`;
- no API keys, credentials, or mounted cloud drives are required;
- random operations use a documented seed of `42`.

The Colab runtime is ephemeral. Generated output bundles should be downloaded after a run or committed separately after inspection. Raw UCI files remain excluded from Git because acquisition is deterministic.

## Environment

The tested environment is pinned in `requirements.txt`. To reproduce locally:

```bash
python -m venv .venv
source .venv/bin/activate        # Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
jupyter lab
```

Run notebooks in order:

1. `notebooks/01_data_audit.ipynb`
2. `notebooks/02_modeling_triage.ipynb`

## Provenance controls

- Dataset page: https://archive.ics.uci.edu/dataset/179/secom
- Dataset DOI: https://doi.org/10.24432/C54305
- Expected archive SHA-256: `eea568baf3c2229096d7d294cf0b096b5502bd96d92c0b80a65b84714059be8e`

A hash mismatch is treated as a source-version change and stops execution. The analysis also asserts the observed schema, label counts, missing-cell count, and timestamp ordering.

## Leakage controls

For each validation design, preprocessing is encapsulated in a scikit-learn pipeline and fit on the training partition only. This includes:

- removing constant or more-than-50%-missing columns;
- median imputation and missingness indicators;
- standardization for logistic regression;
- model fitting.

The top-10% review policy is capacity-defined rather than tuned against held-out labels. Full-dataset quality tables are descriptive and are not used as pre-split feature-selection lists.
