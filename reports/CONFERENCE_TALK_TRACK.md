# Conference talk track and discussion prompts

## 60-second project explanation

I framed the SECOM case study as an operational ranking problem: if engineers can inspect only 10% of records, can a model concentrate failures there? The public data are old and anonymous, so I treat this as a methods demonstration rather than a claim about a current fab.

My primary test is chronological: the first 80% of records train the models and the last 20% form a forward holdout. Every learned preprocessing step is fit only on training data. A regularized, class-weighted logistic regression reached 0.206 average precision against 0.054 holdout prevalence. Reviewing the top 32 of 314 records captured 5 of 17 failures, or 29% recall and 2.9 times lift.

The central finding is that the model ranking reverses under a stratified random split: a constrained random forest looks stronger there but weakens sharply on the chronological holdout. That makes split design part of the evidence. Anonymous feature associations can prioritize investigation, but they are not physical root causes. A credible next step needs newer forward cohorts, process context, and prospective reviewer feedback.

## Three-slide narration

1. **Problem.** Define a fixed review budget, establish the rare-event base rate, and state that the data are a limited public proxy.
2. **Method and evidence.** Contrast chronological validation with random sensitivity analysis; report average precision and failures captured at the capacity threshold.
3. **Limits and next step.** Separate predictive association from root cause; name the operational and contextual data required before deployment.

## Five questions for SEMICON Taiwan conversations

1. **Smart manufacturing:** When a yield-risk model produces a ranked queue, how do your teams translate engineer review capacity and false-alarm cost into an operating threshold—and who owns that decision?
2. **Virtual metrology:** How do you align delayed or downstream labels to upstream sensor traces without introducing temporal leakage, especially when process routes and rework create ambiguous timestamps?
3. **Root-cause analysis:** What tool, chamber, recipe, product, maintenance, and genealogy context must be joined before a predictive signal is considered a credible root-cause hypothesis?
4. **EDA/manufacturing co-optimization:** Where have you seen design or layout context materially improve manufacturing prediction, and how do teams control confounding and protect sensitive design IP across that boundary?
5. **Deployment constraints:** In production, which constraint is usually hardest—data latency, on-premise compute, model approval, calibration drift, retraining governance, or integration with existing fault-detection workflows?

## Evidence boundary to state aloud

- The dataset contains anonymized variables, so no variable is assigned a physical interpretation.
- The analysis shows predictive association, not causality or root cause.
- The chronological holdout contains only 17 failures; uncertainty is therefore material.
- Scores are rankings, not calibrated failure probabilities.
- The drift perturbation is explicitly simulated and is not evidence of a historical fab event.

## Official references

- UCI SECOM dataset: <https://archive.ics.uci.edu/dataset/179/secom>
- Dataset DOI: <https://doi.org/10.24432/C54305>
- Scikit-learn model evaluation: <https://scikit-learn.org/stable/modules/model_evaluation.html>
- Scikit-learn decision thresholds: <https://scikit-learn.org/stable/modules/classification_threshold.html>
