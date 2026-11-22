# Real-data feasibility: eICU APACHE IVa

**Status:** access, completeness and site-level sufficiency have not yet been established.

## Rationale

The eICU Collaborative Research Database provides APACHE predictions and hospital outcomes. The `apachePatientResult` table is a candidate source for APACHE IVa predicted hospital mortality and hospital mortality outcomes. This permits a fixed, already-developed risk score to be examined across multiple hospitals without immediately mapping feature sets from MIMIC-IV.

Documentation:
- https://eicu.mit.edu/eicutables/apachepatientresult/
- https://eicu.mit.edu/tutorials/apache/
- https://physionet.org/content/eicu-crd-demo/2.0.1/
- Pollard et al. 2018, doi:10.1038/sdata.2018.178

## First check using public demonstration records

Confirm table/field names, version value `IVa`, hospital mortality prediction scale and outcome coding. Verify one patient-unit encounter joins as expected and test extraction and missingness reporting. Do not treat demo statistics as estimates for the full 208-hospital database.

## Full-data counts, with authorized access only

Construct an aggregate table per hospital with:
- candidate hospital-unit encounters and unique patients;
- available, nonmissing APACHE IVa hospital mortality predictions;
- valid probability values and resolved hospital discharge outcomes;
- prediction-version duplicate rows and data discrepancies;
- excluded APACHE eligibility categories and their frequency when identifiable;
- observed deaths/events and nonevents;
- number eligible for a 25-, 75- or 150-patient audit plus prespecified independent test cohort;
- hospital year coverage, if documented, to assess coarse annual sensitivity splits.

Determine eligibility thresholds after viewing the available distribution and report the selected-hospital fraction and selection effects. Do not assume 208 hospitals have sufficiently informative samples or that all admissions have APACHE predictions.

## Patient and hospital partition

Use hospital-held-out prior learning, with no target hospital contributing to its own predictive prior. Within the target hospital, define patient-disjoint calibration and evaluation samples. Repeated random sparse-audit draws can improve precision but do not create independent hospitals; analyze repeated draws as correlated within a hospital.

**Time limitation:** The database records admission-relative offsets. They do not generally recover chronological ordering across patient stays within a calendar year. Do not call a random audit/test split temporal or prospective. Any hospital discharge year split must be explicitly evaluated for sufficient site coverage.

## Validation and interpretation

Check differences between model's original target population and selected APACHE-eligible eICU population. At roughly 10% outcome risk, 25 independent patients have probability 0.9^25 ≈ 0.072 of zero events, illustrating the need to retain one-class audits; actual events are clustered, and a global rate does not describe every hospital.

Brier and log-loss improvement demonstrate predictive performance, not improved care or safety. Hospital-level calibration uncertainty must not be confused with heterogeneity in true hospital calibration.

## Data protection

The repo is public. Never commit eICU patient records, data extracts, row-level identifiers, restricted Penn State data, or credential files. Commit only code and permitted synthetic data and non-disclosive aggregate summaries consistent with the database's data-use terms.

## Next output

A script specifying the precise extraction/eligibility decision, plus a count-only multicenter feasibility report generated after authorized access.
