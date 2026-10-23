# When the Cloud Comes to Town: AI Data-Centre Water Demand and Utility Capacity

## WCI and PDLR reproducibility package

**Authors:** Basit A. Akinade, Amobichukwu C. Amanambu, Jonathan M. Frame, and Shaolei Ren

**Correspondence:** Amobichukwu C. Amanambu, Water INtelligence and Geospatial Sensing (WINGS) Laboratory, Department of Geography and the Environment, The University of Alabama, Tuscaloosa, Alabama, USA

This repository provides the data, analysis code, validation tests, and
figure-generation workflow supporting *When the Cloud Comes to Town: AI
Data-Centre Water Demand and Utility Capacity*. It is the authoritative public reproducibility package for the
Water Consumption Impact (WCI) index and its companion Peak Delivery Load
Ratio (PDLR).

## Study overview

The study connects three pathways through which water systems and artificial
intelligence interact:

1. **Burden pathway:** AI data centres create direct and indirect demands on
   water systems.
2. **Constraint pathway:** water availability, infrastructure, regulation, and
   community conditions can influence data-centre siting and development.
3. **Adaptive pathway:** AI-enabled tools can support water monitoring,
   forecasting, treatment, conservation, and system management.

Together, these pathways form the **Water and AI Feedback Loop**. The
quantitative component introduces WCI and PDLR as transparent screening
metrics for comparing estimated peak data-centre water demand with an
explicitly declared host-utility capacity reference.

## Metric definitions

The two dimensionless screening metrics are

```text
WCI  = C_peak / K
PDLR = W_peak / K
```

where `C_peak` is estimated peak consumptive demand, `W_peak` is peak
withdrawal or delivery demand, and `K` is the explicitly declared utility-
capacity reference. Under a shared-peaking-factor or constant-consumptive-
ratio construction, `WCI = r * PDLR`.

| Symbol | Definition |
|---|---|
| `C_peak` | Estimated peak consumptive water demand |
| `W_peak` | Estimated peak withdrawal or delivery demand |
| `K` | Explicitly declared utility-capacity reference |
| `r` | Consumptive ratio, when required by the calculation route |
| `PF` | Applicable peaking factor when demand is not already reported as peak |

The implementation preserves directly reported peak quantities without
applying an additional peaking factor.

WCI and PDLR do not measure firm available headroom, watershed scarcity,
drought probability, distribution-system adequacy, ecological effect, or the
probability of service failure. A ratio of one is arithmetic equality with the
selected capacity reference, not a validated operational threshold.

Results depend on the definition and boundary of `K`. Local, system-wide,
historical, planned, nominal-capacity, allocation, and available-headroom
denominators are not interchangeable.

## Case-study results represented in this repository

The application audit contains ten US cases. All ten primary comparison anchors now have explicitly conditional numerical WCI and PDLR values under declared boundaries. Council Bluffs uses a reconstructed 30 MGD combined nominal potable-treatment capacity; The Dalles uses 8.7 MGD of current reliable peak-season supply; and Douglas County is evaluated on the matched reclaimed-water pathway using the reported 3.0 MGD subsystem capacity and a within-campus consumptive-ratio proxy. Reported Douglas source totals additionally bound reclaimed `r` at `0.8232–0.8402`; conditional on fixed `PF=2.5` and `K=3 MGD`, WCI is `81.85–83.54%` around the `82.15%` point value. This is a deterministic allocation bound, not a confidence interval. The ten numerical WCI anchors span approximately `0.157%` to `134%` after rounding.

PF dependence is reported explicitly for the two reconstructed average-flow cases: Council Bluffs has `WCI(%) = 9.2004 × PF` and `PDLR(%) = 12.8443 × PF`, while The Dalles has `WCI(%) = 11.3498 × PF` and `PDLR(%) = 14.4809 × PF`.

A case-specific deterministic uncertainty check evaluates the four comparison
cases identified for closer examination. PF is varied from `2.0` to `4.0` for
Mayes County, Midlothian, and Henderson, while `r` is varied from `0.70` to
`0.90` for Botetourt County. Botetourt's directly reported 2 MGD maximum is
not multiplied by an additional PF. The numerical order remains Mayes County
`>` Botetourt County `>` Midlothian `>` Henderson for both WCI and PDLR over
the tested ranges; the smallest WCI separation is `0.299` percentage points.
These deterministic envelopes are not probability distributions, confidence
intervals, or hydrological-risk rankings.

The community-context calculation has a separate gate. Six cases have a
defensible FY2024 average-consumption value; four planned or service-maximum
records do not. Repository Figures 3a and 4a include all ten primary WCI/PDLR
cases, while repository Figures 3b and 4b plot the six cases eligible for the
independent community-context calculation. The repository figures use a
reproducibility-focused layout and are not intended to duplicate the article's
typeset figure design.
The tabular files preserve the full ten-case audit and the reason for every
unreported result.

This span describes the retained conditional cases. It is not an empirical
confidence interval, a definitive cross-site ranking, or evidence that one
denominator convention applies across all sites. Alternative-boundary and
failure-mode tests demonstrate that magnitudes and ordering can change with
the selected denominator and other governing assumptions.

## Repository contents

```text
analysis/  Reproducible numerical analysis and figure-generation scripts
data/      Audited model inputs, provenance, and community-context inputs
docs/      Data dictionary and evidence/boundary documentation
results/   Generated tables, validation checks, and file manifests
figures/   Generated vector figures for the public reproducibility package
WCI_10Site_Workbook_v7_0.xlsx  Formula-driven workbook for the current model
```

The repository intentionally excludes internal working notes, superseded
implementations, external source documents, and duplicate figure formats.

## Reproduce the numerical analysis

Python 3.10 or newer is recommended. The numerical analysis uses only the
Python standard library.

Clone the repository and enter its root directory:

```bash
git clone https://github.com/Water-INtelligence-WINGS-Lab/AI-Data-Center-and-Water-Consumption-Impact.git
cd AI-Data-Center-and-Water-Consumption-Impact
```

Then run:

```bash
python analysis/run_wci_pdlr_analysis.py
```

A successful run writes the result tables to `results/` and reports that:

- 14 audited scenarios were calculated;
- the ten-case comparison retains ten conditional numerical WCI/PDLR anchors;
- 48 one-at-a-time sensitivity checks were completed;
- 23 synthetic boundary and failure-mode cases passed;
- no complete unsupported empirical low or high envelope was generated;
- the Douglas source-allocation bound and both PF-dependence equations were reproduced;
- four case-specific deterministic uncertainty envelopes were reproduced and
  their continuous numerical order was verified;
- the 104-source row-level corpus reproduced every Supplementary Table S4 total; and
- all 20 automated validation checks passed.

## Reproduce the figures

Install the plotting dependencies and run:

```bash
python -m pip install -r requirements.txt
python analysis/build_figures.py
```

This creates:

- `figures/Figure_3ab_WCI.pdf`;
- `figures/Figure_4ab_Decomposition.pdf`;
- `figures/Figure_S1_Sensitivity_FailureModes.pdf`; and
- `figures/Figure_S2_Uncertainty_Check.pdf`.

The plotting script checks the ten numerical primary WCI/PDLR anchors, six
community-context-eligible cases, direct-peak calculation routes, PDF/JPG
validity, and text-layout intersections. Figures 3--5 may use a different
visual structure from the article while preserving the same audited inputs,
equations, units, and reported values.

## Principal data and results

- `WCI_10Site_Workbook_v7_0.xlsx` provides a formula-driven Excel version of
  the current ten-site calculations, evidence ledger, community-context
  results, scenario and uncertainty checks, figure data, and validation
  outputs. Runtime provenance and file hashes are maintained separately in
  `results/run_metadata.csv` rather than displayed in the workbook.
- `data/wci_input_provenance.csv` is the authoritative input ledger. Each row
  records one scenario parameter, its evidence class, source, time basis,
  water pathway, capacity convention, and boundary decision.
- `data/community_context_inputs.csv` contains the independently labelled
  jurisdictional comparison inputs used in the contextual figure panels.
- `data/case_specific_uncertainty_inputs.csv` declares the four scenarios,
  varied parameter, and deterministic parameter ranges used for Table S11 and
  Figure S2. Central case inputs are read from the audited comparison results.
- `data/source_corpus_classification.csv` contains one row for each of the 104
  review sources, including its citation key, primary pathway, evidence tier,
  source type, contribution, cross-pathway role, inclusion basis, and source URL
  when available.
- `results/wci_pdlr_comparative_results.csv` contains all ten primary comparison records, including conditional numerical WCI/PDLR values and their calculation status.
- `results/wci_pdlr_scenario_results.csv` contains the comparison anchors and
  documented alternative-boundary scenarios.
- `results/douglas_reclaimed_allocation_bound.csv` reproduces the deterministic
  reclaimed-stream allocation bound conditional on fixed PF and K.
- `results/wci_pdlr_analytic_sensitivity.csv` contains the 48 exact algebraic
  one-at-a-time checks at +/-10%, +/-25%, and +/-50%.
- `results/wci_pdlr_case_specific_uncertainty.csv` contains the exact endpoint
  and central WCI/PDLR values, numerical positions, order-stability result, and
  minimum separation between adjacent cases used in Table S11 and Figure S2.
- `results/wci_synthetic_boundary_tests.csv` contains the 23 constructed
  boundary, invalid-input, and failure-mode cases.
- `results/wci_validation_tests.csv` records automated reproducibility checks.
- `results/run_metadata.csv` records the runtime environment, conversion
  constant, and input and script hashes.
- `results/figure_manifest.csv` and `results/figure_layout_audit.csv` record
  figure inputs, output hashes, and layout-validation status.

See `docs/DATA_DICTIONARY.md` and `docs/EVIDENCE_AND_BOUNDARIES.md` before
reusing or comparing the scenarios.

## Evidence policy

Evidence codes are `M` (measured or directly reported boundary-specific value),
`R` (reconstructed), `T` (transferred), `A` (author assumption), `P` (planned
or permitted), and `U` (unresolved). A numerical result is calculated only when
the declared numerator and denominator pass the analysis-unit, time, water-
pathway, geographic, and capacity-convention checks. Missing or incompatible
ratio fields remain blank. Supported numerator quantities are retained rather
than suppressed when only the denominator gate fails.

No complete source-backed low/high input set was available for any of the ten
comparison cases. Douglas County is the one partial exception: the reported
source totals provide a deterministic allocation bound for reclaimed `r`, but
PF and K remain fixed and the result is not a joint uncertainty interval. The
separate four-case uncertainty check evaluates explicitly stated parameter
ranges rather than source-derived probability distributions. The uncertainty,
sensitivity, and synthetic outputs are therefore not empirical confidence
intervals or site forecasts.

## Sensitivity and failure-mode assessment

The repository separates empirical uncertainty from diagnostic sensitivity
testing:

- **One-at-a-time sensitivity checks** report exact algebraic WCI and PDLR
  responses to `+/-10%`, `+/-25%`, and `+/-50%` perturbations in governing
  inputs.
- **Alternative-boundary scenarios** examine different explicitly labelled
  capacity references for the same numerator.
- **Synthetic failure-mode tests** cover unit and scale invariance, missing or
  invalid inputs, zero direct consumption, extreme peaking, available
  headroom, campus aggregation, growth decoupling, and rank reversal.
- **Case-specific parameter-range checks** evaluate PF from `2.0` to `4.0` for
  Mayes County, Midlothian, and Henderson and `r` from `0.70` to `0.90` for
  Botetourt County, with all other case inputs held fixed.

These tests diagnose construct behaviour and potential misuse. They are not
site observations, empirical uncertainty intervals, operational thresholds,
or forecasts.

## Automated validation

The committed analysis passes checks covering:

- absence of fabricated empirical bounds;
- ten-site anchor coverage, retention of supported numerators, and suppression
  of ratios when denominator eligibility fails;
- leap-year conversion;
- the stated WCI-PDLR identity;
- direct-peak handling without double peaking;
- denominator-specific alternative scenarios;
- the reported rounded WCI span;
- complete sensitivity-design execution; and
- successful execution of all synthetic tests;
- exact Douglas allocation-bound reconstruction;
- exact Council Bluffs and The Dalles PF coefficients;
- exact reproduction and continuous-order validation of the four case-specific
  uncertainty envelopes; and
- 104 unique source-corpus keys with the five-class Table S4 pathway-by-tier matrix.

The GitHub Actions workflow in `.github/workflows/validate.yml` reruns the
numerical analysis and figure workflow whenever the repository is updated.

## Reuse guidance

Users extending the analysis to another site should:

1. declare the facility, campus, development, utility, or regional analysis
   unit;
2. document each parameter and its evidence class in the input ledger;
3. match numerator and denominator boundaries before calculation;
4. distinguish nominal capacity, reliable supply, allocation, and available
   headroom;
5. retain supported inputs while leaving missing or inadmissible derived fields
   blank;
6. avoid applying an additional peaking factor to a directly reported peak;
   and
7. rerun all validation and sensitivity checks after modifying the inputs.

Cross-site comparisons should remain conditional unless evidence classes and
denominator conventions are harmonised.

## Reproducibility and provenance

The numerical workflow uses the fixed conversion
`1 MGD = 3.785411784 ML/day`. Generated metadata include the Python version,
operating platform, conversion constant, and SHA-256 hashes of the
authoritative input ledger and analysis script.

## Citation

Citation metadata are provided in `CITATION.cff`. GitHub users can select
**Cite this repository** on the repository page to export the software
citation.

Suggested software citation:

> Akinade, B. A., Amanambu, A. C., Frame, J. M., & Ren, S. (2026). *WCI and
> PDLR Reproducibility Package* (Version 1.3.0) [Software]. GitHub.
> https://github.com/Water-INtelligence-WINGS-Lab/AI-Data-Center-and-Water-Consumption-Impact

Please also cite the associated article when its final bibliographic
information is available.

## Licence

The analysis and figure-generation code is released under the MIT License.
External datasets, reports, and source documents remain subject to their
original terms and licences. External source documents are not redistributed;
they are identified in the input ledger and cited in the associated article or
Supplementary Information.
