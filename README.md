# Credit Risk Classification

**Binary credit-risk classification in R with tidymodels and Quarto**

This educational project compares logistic regression and Random Forest models for classifying credit status (`bad` or `good`). It also examines how three preprocessing strategies affect model performance on the `credit_data` dataset supplied by the R package [`modeldata`](https://cran.r-project.org/package=modeldata).

**[View the rendered report](index.html)** · [Read the Quarto source](credit_risk_classification.qmd)

## Dataset

The analysis uses `modeldata::credit_data`; no external data file is required. The dataset has **4,454 observations** and a binary `Status` outcome:

| Credit status | Observations |
| --- | ---: |
| `good` | 3,200 |
| `bad` | 1,254 |

Predictors include financial and personal characteristics such as income, assets, debt, loan amount, employment seniority, and credit records. Some variables contain missing values, which are handled within the preprocessing recipes.

## Methodology

1. Explore variable types, missing values, and the target-class distribution.
2. Make a stratified 70/30 training/test split.
3. Compare three `recipes` preprocessing strategies: imputation and standard encoding; engineered financial ratios, rare-category grouping, and correlation filtering; and a reduced domain-motivated predictor set.
4. Combine the three recipes with logistic regression (`glm`) and Random Forest (`ranger`) to create six workflows.
5. Compare workflows with 5-fold stratified cross-validation using accuracy, ROC AUC, sensitivity, and specificity.
6. Evaluate the accuracy-leading workflow on the held-out test set and inspect its confusion matrix. The report also compares model-specific feature-importance plots.

Preprocessing is defined with `recipes`, so recipe steps are estimated within resampling rather than applied to the full dataset before cross-validation.

## Results

The following are **previously recorded cross-validation results**, not guaranteed results from every rerun or independent test-set scores:

| Selection metric | Leading workflow | Approximate CV score |
| --- | --- | ---: |
| Accuracy | `rec_1_model_log` | 0.796 |
| ROC AUC | `rec_2_model_rf` | 0.834 |

For the accuracy-leading model, previously recorded sensitivity for the `bad` class was approximately **0.50**, compared with specificity of approximately **0.91**. This illustrates why overall accuracy alone may not be sufficient for credit-risk use. The rendered report calculates held-out test metrics separately when it is rendered.

## Requirements and running the analysis

You need [R](https://www.r-project.org/) and [Quarto](https://quarto.org/docs/get-started/).

Install the R packages used by the analysis from an R console:

```r
install.packages(c(
  "tidyverse", "broom", "modeldata", "skimr", "vip",
  "rsample", "recipes", "parsnip", "workflows", "workflowsets",
  "dials", "tune", "yardstick", "ranger", "glmnet", "patchwork"
))
```

From the repository directory, render the report:

```bash
quarto render credit_risk_classification.qmd
```

Quarto writes `credit_risk_classification.html` by default. To render directly to the repository's current report path, use:

```bash
quarto render credit_risk_classification.qmd --output index.html
```

Rendering reruns the analysis, including cross-validation, and may take some time. The committed `index.html` is a rendered report snapshot; rerender it after changing the Quarto source if you want the snapshot to reflect those changes.

## Repository structure

```text
.
├── README.md
├── credit_risk_classification.qmd  # Analysis source and report content
└── index.html                      # Rendered report snapshot
```

The repository currently has no `docs/` directory, `install_packages.R`, or `requirements.txt`. The report link above points to the tracked `index.html`. GitHub Pages is not configured in the repository at present; if you enable it, configure the publishing source to match the location of the rendered `index.html`.

## Limitations

This is an educational modelling study, **not a production credit-scoring system**. The sample dataset and analysis do not establish model fairness, probability calibration, or suitability for real lending decisions. The recorded cross-validation scores should be reproduced and reviewed before being relied on; in particular, the relatively low sensitivity for `bad` cases deserves attention. Consider threshold selection, class imbalance, calibration, fairness, and additional validation before any real-world application.

## Author

Natalia Jona
