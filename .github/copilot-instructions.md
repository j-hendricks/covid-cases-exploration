# Copilot Instructions for `covid-cases-exploration`

## Project context
This is a DS 5003 course project focused on COVID-19 healthcare capacity analysis and prediction in the United States. The most important deliverables in this repository are:
- `prediction.ipynb`
- `README.md`
- concise, report-ready charts/tables that support the final paper and presentation

The Tableau dashboard already exists and is generally out of scope unless the user explicitly asks to modify it.

## Audience and tone
Write for a healthcare decision-maker such as a hospital operations leader or public health official.

Preferred tone:
- concise
- academic but accessible
- action-oriented
- decision-focused, not overly technical

Always emphasize the practical question:
**Can we predict high inpatient bed utilization early enough to support staffing and surge planning?**

## Priorities
When helping in this repo, prioritize work in this order:
1. Make `prediction.ipynb` reproducible and submission-ready
2. Improve `README.md`
3. Keep dependencies and setup instructions accurate
4. Support the final report/presentation with clear findings

## Notebook guidance
When editing notebooks:
- keep the workflow clean and linear so it runs top-to-bottom
- avoid duplicate modeling blocks unless they add clear value
- add short markdown explanations before major steps
- prefer a few strong visuals over many weak ones
- keep outputs report-ready and easy to interpret

## Modeling guidance
For prediction work:
- use time-series-aware validation only
- avoid data leakage from future dates
- compare against at least one simple baseline
- prefer interpretable, defensible models over unnecessary complexity
- report metrics clearly, especially `F1`, `precision`, `recall`, and balanced accuracy when classes are imbalanced
- include a short interpretation of what the best model means operationally

## Documentation guidance
When updating `README.md` or report-support text:
- explain the objective, data source, methods, and findings clearly
- include setup and run instructions
- summarize key results in plain language
- note important limitations honestly

## Constraints
Keep project work aligned with the class brief:
- strong objective and scope
- technically sound methods
- polished communication
- avoid unnecessary scope expansion
