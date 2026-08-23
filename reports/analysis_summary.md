# Yield triage analysis summary

Generated: 2026-08-22T18:33:54.296233+00:00

## Primary chronological result

The final 20% holdout contains 314 records and 17 failures. The regularized logistic model achieved average precision **0.206** versus failure prevalence **0.054**. At a fixed 10% review budget, it captured **5 of 17 failures** (29.4% recall) with 15.6% precision and 2.9× lift over prevalence.

The 95% stratified-bootstrap interval for chronological logistic average precision was **0.108–0.424**, reflecting the small number of held-out failures.

## Split sensitivity

Model ranking reversed across validation designs. Logistic regression led on the chronological holdout (AP 0.206 versus forest 0.075), while the constrained forest led on the stratified-random holdout (AP 0.189 versus logistic 0.124). This is evidence that conclusions depend materially on the train/test assumption; it is not evidence that either model is production-ready.

## Interpretation boundary

Coefficient and record-level contribution tables identify anonymous variables associated with model scores. They do not establish physical root cause, controllability, or causal leverage.

## Deployment implication

A defensible next experiment would require newer labeled cohorts, known process/tool context, a predefined engineering review budget, calibration data, and prospective monitoring before any operational use.
