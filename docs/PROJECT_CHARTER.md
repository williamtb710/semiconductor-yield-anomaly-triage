# Project Charter

## Decision statement

Build a compact, explainable case study showing how a manufacturing analytics practitioner could rank rare-failure risk for review while being explicit about data quality, leakage, operating thresholds, uncertainty, drift, and the boundary between prediction and physical root cause.

The project succeeds if William can explain the choices and limitations clearly to semiconductor EDA, smart-manufacturing, process-control, yield-analytics, inspection, and digital-twin practitioners at SEMICON Taiwan.

## Scope

### MVP — must be defensible by August 25

1. **Provenance and audit:** official UCI acquisition, hash, shape/label/timestamp checks, missingness and constant-feature report.
2. **Validation design:** chronological holdout as the deployment-like primary view if viable, plus stratified-random sensitivity analysis. State why neither proves future-fab generalization.
3. **Baselines:** an always-pass/naive reference and a leakage-safe pipeline using training-only imputation, scaling, regularized logistic regression, and class weighting.
4. **Limited comparison:** at most one nonlinear contrast (planned default: a constrained random forest) after the linear baseline works. No broad hyperparameter sweep.
5. **Evaluation:** confusion matrix, precision-recall curve, average precision, recall and precision at a stated review budget (planned default: top 10% risk) and/or false-alarm constraint. Include class prevalence and raw counts.
6. **Interpretation:** coefficients and/or held-out permutation importance; stability across training resamples/folds if time permits. Always use anonymous names such as `sensor_123`.
7. **Triage output:** ranked records with timestamp, observed label, risk score, decision at chosen threshold, and a small set of top contributing anonymous variables.
8. **Monitoring:** descriptive time-bucket prevalence/missingness checks and one clearly labeled simulated shift demonstration.
9. **Conference package:** polished README, clean notebooks, environment file, saved figures, one-page executive summary, three-slide talk track, five technically informed questions, and a 60-second demo script.

### Optional — cut before weakening the MVP

- Probability calibration if failure counts support a separate calibration procedure.
- Bootstrap uncertainty intervals on final holdout metrics.
- A small Streamlit or Gradio threshold slider.
- More elaborate coefficient-stability graphics.
- Packaging reusable helpers under `src/`.

## Explicit non-goals

- Claiming that this 2008 public dataset represents a current fab or a specific tool/process.
- Inferring physical meanings for anonymous variables.
- Claiming causality, control limits, virtual-metrology accuracy, or production readiness.
- Optimizing a leaderboard metric through many models or repeated peeking at a held-out set.
- Treating a high-risk prediction as a root-cause diagnosis.

## Four-day schedule

The departure date is August 26, 2026. Work ends August 25 with a hard packaging cutoff; no late dashboard detour.

| Date | Time box | MVP output | Optional only if ahead |
|---|---:|---|---|
| **Sat Aug 22 — data and validation** | 3 hours | Run/confirm Checkpoint 1; inspect time distribution and failure counts; lock chronological and stratified comparison splits; build naive and logistic baselines | Draft time-bucket drift table |
| **Sun Aug 23 — evidence and operating point** | 3–4 hours | Finalize the limited model comparison; PR curve/AP; confusion matrices; top-10%-review operating point; error analysis | Calibration if a leakage-safe procedure is supportable |
| **Mon Aug 24 — interpretation and triage** | 3 hours | Coefficients/permutation importance, stability check, ranked triage table, descriptive/simulated drift section | Maximum 60 minutes for a dashboard after the notebook is complete |
| **Tue Aug 25 — conference package** | 3 hours | One-page summary, three slides, README cleanup, five questions, 60-second script, clean-run verification from top to bottom | Visual polish only; no new models |

**MVP cutline:** by Sunday night, a leakage-safe logistic baseline plus a clear PR/threshold story is already a valid project. If anything slips, remove the nonlinear model, calibration, bootstrap, and dashboard in that order.

## Evidence standard

- Report exact sample counts and prevalence alongside rate metrics.
- Keep one untouched final test view per split design; tune only within training data.
- Keep all transformations in a scikit-learn `Pipeline` or equivalent training-only workflow.
- Use the chronological result as primary when viable and the random result to show sensitivity to the split assumption.
- Describe importance as association with predictions/outcomes, never as physical root cause.
- Label uncertainty estimates, simulated drift, and assumptions visibly.

## Checkpoint log

### Checkpoint 1 — starter audit

- **Source:** official UCI dataset page and archive, dataset ID 179.
- **Known catalog facts:** 1,567 examples; 104 fails; missing values; labels `-1` pass and `1` fail; separate test-point timestamps.
- **Raw-file fact surfaced by audit:** `secom.data` contains 590 numeric columns even though the UCI page and `secom.names` describe 591 features. Preserve and report the discrepancy instead of silently forcing the data to match the metadata.
- **Current decision:** no modeling, imputation, feature removal, or split selection before William runs and confirms the audit.

## Conference-ready framing

“I used an old, anonymized public dataset on purpose, so I treated it as a case study in analytical discipline rather than a claim about a modern fab. The core question was whether I could build a leakage-safe rare-event triage workflow and explain the operational tradeoff between catching failures and creating engineering review load. Feature rankings are hypotheses for investigation, not root causes.”
