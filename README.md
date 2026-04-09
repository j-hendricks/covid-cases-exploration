# COVID Cases Exploration: Early Warning for Hospital Bed Strain

## Overview

This DS 5003 course project evaluates whether state-level COVID and hospital capacity indicators can predict **high inpatient bed utilization 7 days in advance**. The intended audience is a hospital operations leader or public health official who needs early warning to support staffing and surge planning.

The repository contains the data pipeline, predictive modeling notebook, and supporting artifacts used for the final report and presentation.

## Repository Structure

- `data_collection.ipynb` — collects and merges source data from the Delphi EpiData APIs
- `prediction.ipynb` — builds and validates the prediction models
- `data/covid_features_all_states_20200101_20251231.csv` — combined analytic dataset
- `dashboards/eda.twbx` — Tableau exploratory dashboard
- `README.md` — project summary and reproduction guide

## Data Source

Data is sourced from Carnegie Mellon’s Delphi EpiData APIs:

- **CovidCast:** [https://api.covidcast.cmu.edu/epidata/api.php](https://api.covidcast.cmu.edu/epidata/api.php)
- **COVID Hospitalization State Time Series:** [https://api.delphi.cmu.edu/epidata/covid_hosp_state_timeseries/](https://api.delphi.cmu.edu/epidata/covid_hosp_state_timeseries/)

### Verified Data Scope

- Raw merged dataset: **111,792 rows × 131 columns**
- Prediction sample used in the validated notebook: **42,891 state-day observations**
- Coverage: **51 states/jurisdictions** (including DC)
- Modeling window: **2022-01-01 to 2024-04-27**

## Prediction Task

We define a high-utilization event as a state’s `inpatient_beds_utilization` exceeding its own **2021 80th percentile threshold**. The model then predicts whether that threshold will be exceeded **7 days later**.

## Modeling Approach

The final notebook uses:

- a **persistence baseline** (“current status persists”)
- **regularized logistic regression**
- **HistGradientBoosting** as the recommended tree-based model

Validation is performed using **5-fold time-series cross-validation** to avoid future-data leakage. Reported metrics include **F1**, **precision**, **recall**, **balanced accuracy**, and **ROC AUC**.

## Verified Results

The following results were reproduced in `prediction.ipynb` from a fresh validation run:

| Model | F1 | Precision | Recall | Balanced Accuracy | ROC AUC |
| --- | ---: | ---: | ---: | ---: | ---: |
| `HistGradientBoosting` | **0.845** | 0.826 | **0.870** | **0.877** | **0.953** |
| `Baseline: current status persists` | 0.832 | 0.832 | 0.832 | 0.863 | 0.863 |
| `Logistic Regression` | 0.802 | 0.795 | 0.835 | 0.855 | 0.932 |

### Operational Takeaway

`HistGradientBoosting` is the recommended model because it achieved the best verified **F1** and **recall**, modestly outperforming the persistence baseline by **+0.013 F1** and **+0.038 recall**. The most influential verified driver was `dist_from_threshold`, followed by current utilization and seasonal timing features.

## How to Reproduce

1. Create and activate a virtual environment:

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Open the notebooks in VS Code or Jupyter.
4. If needed, run `data_collection.ipynb` to regenerate the dataset.
5. Run `prediction.ipynb` top-to-bottom to reproduce the validated model comparison.

## Limitations

- The analysis is **state-level**, not hospital-level.
- Some signals contain substantial missingness and are handled with **median imputation**.
- The improvement over the persistence baseline is real but **modest**, so the model should support operational judgment rather than replace it.

## Project Goal

The central question for this project is:

> **Can we predict high inpatient bed utilization early enough to support staffing and surge planning?**

Based on the current validated results, the answer is **yes, to a useful degree**, especially when using the boosted model as an early-warning aid.
