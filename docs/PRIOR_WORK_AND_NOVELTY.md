# Prior work and novelty assessment

**Status:** focused starting bibliography. Not a systematic review. Citations, method details, and the absence of closer competitors require full-text and forward-citation verification before manuscript submission.

## Established work that constrains novelty

| Topic | Closest precedent | Implication |
| --- | --- | --- |
| Clinical prediction-model update selection | Vergouwe et al., 2017, *Statistics in Medicine*. doi:10.1002/sim.7179 | A sequence of testing decisions among unchanged, recalibration and revision is established; a simple Wald-like disagreement test is not automatically new. |
| Bayesian logistic recalibration/revision | Feng et al., 2022, *JAMIA*. doi:10.1093/jamia/ocab280 | Bayesian recalibration is not novel on its own; BLR/MarBLR are important comparators. |
| Historical borrowing and robust MAP | Schmidli et al., 2014, *Biometrics*. doi:10.1111/biom.12242 | Mixture priors to mitigate prior-data conflict already exist. |
| Bayesian prior-data conflict | Biziaev et al., 2025, *Statistics and Computing*. doi:10.1007/s11222-025-10582-1 | Tension as disagreement between data and prior is an established statistical problem. |
| Calibration heterogeneity and performance transport | Debray et al., 2019, *Statistical Methods in Medical Research*. doi:10.1177/0962280218785504 | Bayesian multicenter calibration meta-analysis is established. |
| Recalibration across populations | Ensor et al., 2021, *Statistics in Medicine*. doi:10.1002/sim.8959 | Using multicenter evidence to judge prediction and recalibration across new settings is established. |
| Prediction-model validation sample size | Sadatsafavi et al., 2026, *Statistics in Medicine*. doi:10.1002/sim.70389 | Bayesian assurance and value-of-information already address limited validation labels. |
| Multihospital eICU generalizability | Singh et al., 2022, *PLOS Digital Health*. doi:10.1371/journal.pdig.0000023 | eICU has prior evidence of heterogeneous cross-site performance and sample-size constraints. |
| Data-scarce clinical recalibration | Brown et al., 2026, *Journal of Biomedical Informatics*. doi:10.1016/j.jbi.2026.105012 | Limited local sample size is not an undiscovered problem. |
| Cosmological parameter discordance | Handley and Lemos, 2019, suspiciousness; Raveri and Hu, 2019, parameter-shift methods | Quantifying dataset disagreement is established in cosmology; verify exact formulations before importing them. |

Other relevant works: Strobl et al. 2015 doi:10.1016/j.jbi.2015.05.001; Davis et al. 2019 nonparametric drift updating (PMC6857513); AutoAdapt preprint arXiv:2610.11162, verify version and relationship to the intended parametric calibration problem.

## Specific contribution to test

1. Learn a **joint predictive distribution of calibration intercept and slope** from hospitals not used for testing at the target.
2. Evaluate hierarchical and robust MAP posteriors under severe target label scarcity and deliberately incompatible hospitals.
3. Formally compare a tension-informed *update action* against robust MAP and a simple posterior expected-loss action with updating costs.
4. Establish where the added tension diagnostic succeeds, fails or adds no value. A negative result can support a careful comparative paper but is not automatically a novel method.

The novelty claim is **conditional**. We have not established that point (1), its combination with point (2), or the proposed gate is absent from prior work. The initial literature review should be followed by full-text inspection, references cited by each close paper, and forward citation searches.

## Distinctions to retain

- Bayesian posterior predictive minimization of squared error already supplies an optimal no-cost decision under a correctly specified model. Any extra rule must address misspecification, nonexchangeability, a relevant cost, or a different action set.
- A discrepancy statistic is not identical to the full Vergouwe closed-testing procedure.
- Borrowing from one original source model is not the same as estimating a distribution of variation across hospitals.
- Astrophysics routinely uses hierarchical population modeling; cross-disciplinary framing should not suggest otherwise.
- A publication venue is chosen after results. *Statistics in Medicine* is a plausible methods-focused target, not a guarantee of novelty or suitability.

## Literature review tasks

- Read Feng, Vergouwe, Schmidli, Debray, Ensor, and Sadatsafavi in full.
- Search for predictive calibration priors learned from multicenter risk-model cohorts and their prospective use on new sites.
- Search for empirical Bayes or hierarchical site-specific calibration of fixed risk scores under low event counts.
- Review suspiciousness/prior predictive checks for non-Gaussian low-information posteriors.
- Distinguish established preprints from peer-reviewed papers and confirm all exact bibliographic identifiers.
