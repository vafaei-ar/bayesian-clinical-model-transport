# Bayesian Clinical Model Transport

**Research concept:** Robust information borrowing and decision-aware recalibration of clinical risk models at hospitals with limited local outcome data.

**Status (22 November 2026):** Research design and feasibility assessment. No proposed method has been evaluated and no study results are claimed.

## Research question

When a risk model is used at a new hospital with few outcome labels, can information about calibration at other hospitals improve local risk estimates without causing harmful adaptation?

The study will compare standard recalibration, empirical-Bayes and hierarchical Bayesian approaches, robust meta-analytic predictive (MAP) priors, and optional conflict- or tension-informed update decisions. The aim is to find out **whether a new decision policy adds value beyond strong existing methods**.

## Documents

- [Research plan](docs/RESEARCH_PLAN.md): questions, hypotheses, statistical formulation, design, outcomes, and decision criteria.
- [Pilot protocol](docs/PILOT_PROTOCOL.md): executable simulation requirements, fair baselines, and acceptance checks.
- [Data feasibility](docs/DATA_FEASIBILITY.md): APACHE IVa in eICU, selection rules, site counts, and validation limitations.
- [Prior work and novelty](docs/PRIOR_WORK_AND_NOVELTY.md): closest methods, literature links, and claims that remain unverified.

## Proposed study sequence

1. Run a simulator pilot using corrected frequentist comparators and a deliberately nonexchangeable hospital population.
2. Check the public eICU demonstration data for APACHE IVa table joins, definitions, and basic code functionality.
3. With authorized access, count hospitals, eligible patients, mortality events, and viable calibration/evaluation splits in full eICU.
4. Finalize a prespecified comparison and evaluation plan based on pilot feasibility, not on a desired positive result.
5. Run independent simulations and multicenter validation, then determine the manuscript's actual contribution.

**Important boundaries:** The study is separate from the 2026 Mauritius workshop. Workshop material should not be altered for this project. Public repository contents must never include credentialed eICU patient records, patient-level Penn State EHR data, access tokens, or other restricted material.

## Research integrity

The core assumptions, experimental choices, and failure cases must be explicit. We will report null or negative comparisons, including situations where robust MAP or simple penalized updating performs as well as the proposed tension-based policy. Bayesian inference itself is not claimed as novel.

## Planned venue

*Statistics in Medicine* is a provisional first target if a genuine methodological result emerges. This is not a submission commitment.
