# Research plan: population-informed clinical risk model recalibration

**Version:** initial study design, 22 November 2026  
**Status:** Proposed work. Neither an advantage nor novelty of a proposed method is established.

## 1. Problem and intended contribution

A risk model developed or maintained elsewhere may be poorly calibrated at a new hospital. Local recalibration can overfit when only a small sample of outcomes is available. Prior hospitals contain useful information about typical calibration differences, but the target may differ substantially from that reference population.

**Central research question:** When and how can evidence from prior hospitals improve risk prediction at a target hospital with limited local labels, and when does borrowing produce harmful updates?

This project will evaluate whether population-informed hierarchical and robust Bayesian calibration improves upon adequately regularized classical updating and whether a cosmology-inspired discordance diagnostic offers additional decision value. The proposed *tension gate* is a hypothesis, not an established new method.

## 2. Hypotheses (prospective)

- **H1 (borrowing):** When hospitals are exchangeable, a calibrated population prior can improve average predictive loss relative to local-only estimates, particularly with few outcomes.
- **H2 (prior conflict):** Naive narrow priors can harm predictions at hospitals outside the reference distribution. Robust MAP priors may reduce this harm.
- **H3 (incremental gate value):** A separately specified tension statistic may improve decision performance relative to robust MAP and posterior expected-loss updating under misspecification or nonexchangeability. The null hypothesis of no incremental value is credible.
- **H4 (information budget):** Expected improvement depends on event counts, nonevent counts, baseline risk distribution, and the type and extent of shift, not only the total audit sample.

## 3. Clinical prediction target

Primary empirical demonstration: fixed APACHE IVa hospital-mortality predictions and observed hospital mortality in eICU-CRD. Treat this as a retrospective multi-hospital validation, not evidence of prospective clinical benefit or use of an intervention.

The cohort, available hospitals, coverage and potential APACHE exclusion criteria must be checked against the released data and documentation before analysis. A second outcome/model is optional and must use compatible prediction horizons and features; a stroke-readmission cohort is not automatically an external validation of APACHE ICU mortality.

## 4. Statistical framework

For the existing prediction p_i at hospital h, set z_i = logit(p_i), with a prespecified numerical clipping rule for probabilities at 0 and 1.

Model observed binary outcomes by

    Y_ih ~ Bernoulli(q_ih)
    logit(q_ih) = alpha_h + beta_h * z_ih

The unchanged prediction corresponds to alpha=0, beta=1. Intercept-only updating holds beta=1. Estimate intercept and slope jointly only where data support it; a positive-slope reparameterization may be evaluated as a sensitivity analysis.

For exchangeable hospitals, use a joint hierarchical distribution for (alpha_h, beta_h), allowing intercept-slope covariance and uncertainty in the population parameters and heterogeneity. Fit the distribution to **prior hospitals only** when evaluating a held-out target hospital. A robust MAP prior mixes the predictive distribution of the historical hospital population with a broad proper component. Specify mixture weights, scales and hyperpriors in the design stage and examine their sensitivity; never choose them on final held-out evaluation labels.

For each posterior fit, compute posterior predictive risk by averaging the inverse-logit response over posterior draws, not merely plugging in posterior mean coefficients.

**Comparison to frequentist methods:** Penalize the target intercept deviation alpha and slope deviation (beta-1), or an empirically learned population center, with penalty scales that do not accidentally depend on audit size. Report penalty definition relative to summed vs averaged likelihood. Include intercept-only and two-parameter fits, and test one-class audits.

## 5. Updating as a decision

Let a denote the set of available actions: unchanged, intercept-only update, two-parameter update, and optionally request additional labels. Under a posterior predictive distribution, evaluate expected loss for each candidate action and any fixed updating cost. A generic normalized objective is

    posterior expected mean Brier(a) + c * I(action changes deployed model).

The cost c is on a Brier-equivalent scale in this analysis, not dollars. Clinical interpretation requires a separate action-based utility or cost model with justified units.

For posterior predictive q_i, choosing q_i over unchanged p_i reduces posterior expected squared-error loss by (q_i-p_i)^2 for that observation under the same correctly specified predictive model. This means an additional tension threshold does not have a guaranteed benefit under correct specification. Its incremental value must be evaluated under conflict, model misspecification or other assumptions that make a simple model or decision unreliable.

Cosmology-inspired parameter-disagreement or suspiciousness metrics will be investigated as **candidate diagnostics**. Their calibration and stability with small, non-Gaussian local samples must be tested. A Gaussian parameter disagreement is not identical to the sequential closed-testing procedure for updating described by Vergouwe et al.

## 6. Simulation design

Start with a fixed source risk model. Generate multiple reference hospitals, then independent target hospitals with known shifts. Simulate:
1. No shift and ordinary exchangeable hospital heterogeneity.
2. Target hospital outliers relative to historical calibration distribution.
3. Baseline-risk and slope shifts separately and together.
4. Calibration-function misspecification (e.g., nonlinear distortions or subgroup-specific effects).
5. Rare outcomes, including one-class labeled audits.
6. Variation in the number and composition of reference hospitals.

Use prospective design values n=25, 75, 150 labeled patients as the main sparse-label experiments; additional sizes can be sensitivity analyses. Use paired calibration and evaluation samples for all methods in each replication. Pilot with a limited number of independent repetitions to estimate Monte Carlo error and runtime; determine the final simulation count from precision requirements, not a predetermined round number.

Prespecify a small set of conditions as confirmatory and use separate development scenarios for choosing tuning parameters. Random seeds must not be used to select favorable conclusions.

## 7. Comparators and endpoints

**Required comparators:** unchanged model; well-centered penalized intercept recalibration; well-centered penalized two-parameter logistic recalibration; empirical-Bayes shrinkage; full hierarchical Bayesian updating; robust MAP updating; closed-testing procedure where its implementation and sample conditions are justified. Include fixed-cost posterior expected-loss decisions and tension-informed decisions only after their specifications are complete. Bayesian model averaging is a possible additional comparator, not automatically required for the initial pilot.

**Primary endpoint:** mean paired difference in independent-test Brier score between methods, measured within each scenario/label-budget setting. Define delta = Brier(method) - Brier(unchanged), with negative values indicating improvement. For comparing two adaptive methods use the paired difference between their scores, not two unrelated comparisons to the unchanged model.

**Safety and calibration endpoints:** fraction of negative-transfer replicates, upper-tail harm magnitude, logarithmic score, calibration-in-the-large, slope, calibration curves, and results by event count. Evaluate posterior coverage under correctly specified simulations. Report false-update and missed-update rates only against explicitly defined oracle/clinical decision targets. AUROC is secondary and is invariant under increasing transformations of the original risk score.

Report uncertainty for the *mean paired method contrast across independent replicates*, separately from the between-replicate distribution of harm. Include uncertainty across target hospitals for multicenter comparisons.

## 8. eICU study design

Use the **same** APACHE IVa outcome definition and version for predicted and observed hospital mortality. Join patient/hospital and APACHE tables using documented encounter keys and verify prediction coverage, units, duplicate encounters and exclusions. Define one analysis unit per patient or account explicitly for correlated stays. Do not assume all 208 hospitals are sufficiently large.

Partition hospitals into historical-prior development and held-out target roles. For every evaluated target hospital, exclude that hospital from population-prior learning. Split eligible target data into separate local audit and final evaluation samples at the patient level. Repeat audit subsampling within each target with clear accounting for dependence.

**Temporal caution:** eICU lacks globally recoverable patient admission dates. ICU-offset times cannot establish ordering of admissions. Investigate the year field as a limited sensitivity analysis but **do not describe the main random split as prospective or chronological validation**.

First deliver an aggregate feasibility table: hospitals by eligible encounters, events, probability coverage, and the counts supporting specified audit/evaluation thresholds. Do not choose a threshold that leaves only large tertiary sites without reporting selection effects. Assess site-level heterogeneity and sampling uncertainty together.

Access to full eICU requires appropriate PhysioNet credentials and use under its data-use requirements. Public eICU demo data can establish parsing and joins but cannot support a valid multicenter empirical conclusion. No restricted patient-level data will be committed to GitHub.

## 9. Go/no-go decision after pilot

Advance a **tension-based methodological claim** only if it offers a reproducible incremental advantage over a well-specified robust MAP and posterior expected-loss comparator in relevant challenging scenarios, including a meaningful improvement in the distribution of harmful updates. Effects should survive sensitivity to prior scales, reference-hospital composition, update cost, and misspecification.

If the gate is equivalent or inferior, retain tension as a diagnostic and focus the paper on robust population borrowing or an honest comparative benchmark. If no approach yields a credible new scientific result, stop or reframe rather than presenting a repackaging of prior methods as novel.

No arbitrary Brier improvement threshold is declared here. The team should specify a meaningful effect and precision target before confirmatory runs.

## 10. Immediate milestones

1. **Method specification:** define fair likelihood penalties and validate fitting with zero-event audits.
2. **Minimal simulator pilot:** compare hierarchical, empirical-Bayes, robust MAP, posterior expected-loss rule, and tension-based candidate under ordinary heterogeneity and nonexchangeability.
3. **Data check:** validate the eICU demonstration-table extraction and produce a checklist for full-data feasibility counts.
4. **Design review:** finalize hypotheses, comparator implementations, simulation count, and leakage protections before full evaluation.
5. **External empirical study:** conduct hospital-held-out evaluation with independently scored patients.
6. **Manuscript:** select claim and journal based on findings, with *Statistics in Medicine* as a provisional venue.

## 11. Limitations to document

Uncertain hospital exchangeability; small local event counts; hospital selection and APACHE eligibility bias; outcome-label ascertainment; model misspecification; sensitivity to prior specification; inability to establish clinical safety from Brier improvement; limited eICU admission chronology; missing data and partial overlap of development/validation populations.

## 12. Related project and permissions

The Mauritius workshop repository is an educational starting point only. This research must not modify its teaching notebooks. Research-specific code and figures must be derived from actual simulations or authorized datasets; do not create illustrative results and present them as measured findings.
