# Credit Risk Classification

**Binary classification in R using tidymodels and Quarto**

This project compares logistic regression and Random Forest models for predicting credit status (`bad` vs. `good`). It examines whether different preprocessing strategies affect predictive performance and highlights the importance of evaluating errors for the higher-risk class—not just overall accuracy.

**[View the HTML report](docs/index.html)** · [Explore the Quarto source](credit_risk_classification.qmd)

> Once GitHub Pages is enabled, the HTML report can also be viewed directly in a browser through the repository's published website.

## Dataset

The analysis uses `credit_data` from the R [`modeldata`](https://cran.r-project.org/package=modeldata) package. The dataset contains **4,454 observations** with a binary outcome variable, `Status`:

| Credit status | Observations |
| --- | ---: |
| `good` | 3,200 |
| `bad` | 1,254 |

Predictors describe financial and personal characteristics, including income, assets, debt, loan amount, employment seniority, and credit records. The dataset is loaded directly from the R package; no external CSV is required.

## Methodology

1. **Data exploration:** Inspect variable types, missing values, and class distribution.
2. **Train/test split:** Create a stratified 70% training / 30% test split.
3. **Preprocessing:** Compare three `recipes` pipelines:
   - **Recipe 1:** Missing-value imputation, categorical encoding, and numeric normalization.
   - **Recipe 2:** Additional financial ratios, rare-category grouping, and correlation filtering.
   - **Recipe 3:** A smaller, domain-motivated predictor subset.
4. **Modelling:** Evaluate logistic regression (`glm`) and Random Forest (`ranger`) across all three recipes, producing **six workflows**.
5. **Validation:** Compare workflows using **5-fold stratified cross-validation**, with accuracy, ROC AUC, sensitivity, and specificity.
6. **Final evaluation:** Fit the accuracy-leading workflow and evaluate it on the held-out test set; examine a confusion matrix and model-specific feature importance.

## Results

The original cross-validation analysis reported the following leading configurations:

| Selection metric | Leading workflow | Approximate CV score |
| --- | --- | ---: |
| Accuracy | `rec_1_model_log` | **0.796** |
| ROC AUC | `rec_2_model_rf` | **0.834** |

For the accuracy-leading model, sensitivity for the `bad` class was approximately **0.50**, compared with specificity of approximately **0.91**. This illustrates the trade-off between overall accuracy and identifying higher-risk customers.

These are cross-validation results from the original analysis, **not independent test-set scores**. The rendered report contains the full evaluation, including held-out test metrics and a confusion matrix.

## Tools

- **R** — analysis and model development
- **tidymodels** (`recipes`, `parsnip`, `rsample`, `workflows`, `workflowsets`, `tune`, `yardstick`) — preprocessing, modelling, and evaluation
- **ranger** and **glm** — classification algorithms
- **tidyverse**, **skimr**, **vip**, **patchwork** — exploration and visualisation
- **Quarto** — reproducible HTML report

## Run locally

Install R and Quarto, then install the required R packages from the repository directory:

```r
source("install_packages.R")
```

Render the report in RStudio or run this command in a terminal:

```bash
quarto render credit_risk_classification.qmd
```

Open the generated `credit_risk_classification.html` in a web browser. Rendering the report reruns the analysis and may take some time because it evaluates multiple models using cross-validation.

## Repository structure

```text
credit-risk-classification/
├── README.md
├── credit_risk_classification.qmd
├── install_packages.R
├── requirements.txt           # Informational list of R packages
├── .gitignore
└── docs/
    └── index.html            # Rendered Quarto report for GitHub Pages
```

To publish the report, copy the generated HTML to `docs/index.html`, then enable GitHub Pages from the `main` branch and `/docs` folder in the repository settings. The Quarto source is configured to embed its resources into the HTML output.

## Limitations

This is an educational classification study, **not a production credit-scoring system**. The dataset is a sample dataset, and the analysis does not establish model fairness, probability calibration, or generalisability to other populations. Threshold tuning and cost-sensitive evaluation would be valuable next steps but were not performed in this project.

## Author

**Natalia Jona**
