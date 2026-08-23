# Analytical Scope and Evaluation Plan

## Decision question

Can anonymous process measurements rank rare downstream failures well enough to support a constrained engineering-review queue, and how sensitive is that conclusion to time ordering and model choice?

The output is a triage aid: a ranked queue for investigation. It is not a process-control rule, virtual-metrology claim, causal model, or physical root-cause diagnosis.

## Dataset constraints

- 1,567 production examples and 104 failures.
- 590 numeric columns in the raw measurement file, despite 591 features in UCI metadata.
- Anonymous variables with no tool, step, unit, or controllability metadata.
- 4.54% missing cells overall, with highly uneven feature missingness.
- Only 17 failures in the final 20% chronological holdout.
- Data donated in 2008; results are not benchmarks for a current fab.

## Validation designs

### Primary: chronological holdout

The first 80% of timestamp-ordered observations form training data; the last 20% form the held-out evaluation period. This approximates training on the past and scoring later records. It cannot reproduce the complexity of a true deployment backtest, but it avoids mixing future records into training.

### Sensitivity: stratified-random holdout

A fixed-seed 80/20 stratified split preserves class prevalence while mixing time. It estimates a different question: discrimination when train and test are exchangeable. Comparing the two exposes dependence on the split assumption.

Neither split supports a claim of cross-fab, cross-product, or future-process generalization.

## Models

1. **Always-pass reference:** demonstrates why plain accuracy is inadequate.
2. **Regularized logistic regression:** interpretable linear baseline with class weighting, median imputation, missingness indicators, and standardized inputs.
3. **Constrained random forest:** one nonlinear contrast with limited depth and minimum leaf size. It is included to test whether simple interactions help, not to pursue a leaderboard.

Hyperparameters are fixed before viewing held-out results. There is no broad search over the test set.

## Operating policy

The primary operating point sends the highest-scored 10% of held-out records to review. Reported quantities include:

- records reviewed;
- failures captured and missed;
- precision, recall, and false-positive rate;
- lift over the holdout failure prevalence;
- full confusion-matrix counts.

This budget-based policy is more interpretable than treating `0.5` as a universal threshold. Scores from class-weighted models are used for ranking and are not presented as calibrated failure probabilities.

## Metrics and uncertainty

- Average precision and the full precision-recall curve are primary discrimination measures.
- ROC-AUC and accuracy are reported only as context.
- Stratified bootstrap intervals quantify sampling uncertainty on held-out average precision and budget-level precision/recall.
- With 17–21 held-out failures, uncertainty is expected to be wide.

## Interpretation standard

Logistic coefficients are reported on standardized transformed inputs. Training-bootstrap selection frequency provides a basic stability check. Record-level contribution summaries identify anonymous variables that increased a score.

All interpretations are predictive associations. Physical root-cause work would require feature semantics, process sequence, control context, maintenance history, product mix, and designed confirmation with domain engineers.

## Monitoring view

The analysis reports descriptive time-bucket changes in failure prevalence and missingness, train-to-test shifts in retained inputs, and one explicitly simulated one-standard-deviation feature shift. The simulated example demonstrates a monitoring mechanism; it is not evidence that the historical process experienced that intervention.
