# Income Classification Using Demographic and Socio-Economic Features

**MSc Data Science Final Year Project — 7PAM2002**
Department of Physics, Astronomy and Mathematics, University of Hertfordshire

## Overview

This project is a supervised binary classification study using the UCI Adult (Census Income) dataset to estimate the probability that a person earns more than 50,000 USD per year. The data comes from the 1994 US Census Bureau and contains 32,561 records with 14 demographic and socio-economic features including age, education, occupation, marital status and capital gain.

Three classifiers from different algorithmic families — Logistic Regression, Decision Tree and Random Forest — are trained in a reproducible scikit-learn pipeline, compared against a majority-class baseline, tuned with grid-search cross-validation, and examined through a multi-method feature importance analysis.

## Research Questions

- **RQ1:** How accurately can demographic and socio-economic features predict income class (above or below $50K)?
- **RQ2:** Among Logistic Regression, Decision Tree and Random Forest, which performs best?
- **RQ3:** Which features most strongly drive each model's predictions, and are those drivers consistent across models?

## Dataset

- **Source:** UCI Machine Learning Repository — Adult (Census Income) dataset
- **URL:** https://archive.ics.uci.edu/dataset/2/adult
- **DOI:** https://doi.org/10.24432/C5XW20
- **Licence:** Creative Commons Attribution 4.0 International (CC BY 4.0)
- **Size:** 32,561 rows × 15 columns (14 features + binary income target)
- **Target:** `income` — `<=50K` (76%) or `>50K` (24%)
- **Numeric features:** age, fnlwgt, education_num, capital_gain, capital_loss, hours_per_week
- **Categorical features:** workclass, education, marital_status, occupation, relationship, race, sex, native_country

## Methodology

1. **Pre-processing pipeline** (`ColumnTransformer` + `Pipeline`):
   - Numeric: median imputation → `StandardScaler`
   - Categorical: mode imputation → `OneHotEncoder(handle_unknown="ignore")`
2. **Train/test split:** 80/20 stratified on the target, `random_state=42`.
3. **Models:** Logistic Regression, Decision Tree and Random Forest, all trained inside the same pipeline to prevent data leakage.
4. **Evaluation metrics:** Accuracy, Precision, Recall, F1-score, ROC-AUC, plus per-class classification reports, confusion matrices, ROC curves and Precision-Recall curves.
5. **Hyperparameter tuning:** `GridSearchCV` with 5-fold `StratifiedKFold` on the best base model (Random Forest), scored by F1 to directly optimise minority-class performance. Grid explored `n_estimators ∈ {150, 250}`, `max_depth ∈ {None, 12, 20}`, `min_samples_split ∈ {2, 5}`, `min_samples_leaf ∈ {1, 2}` — 24 combinations × 5 folds = 120 fits.
6. **Feature importance:** logistic-regression coefficients, Gini impurity importance (Random Forest) and permutation importance across all three models.

## Key Results

| Model | Accuracy | F1 | ROC-AUC |
|---|---|---|---|
| Majority-class baseline | 0.761 | 0.000 | — |
| Logistic Regression (base) | — | — | 0.908 |
| Decision Tree (base) | — | 0.619 | 0.769 |
| Random Forest (base) | — | 0.672 | 0.895 |
| **Random Forest (tuned)** | **0.869** | **0.702** | **0.921** |

Capital gain, marital status, occupation, age and education level were consistently identified as the most important predictors across all three importance methods.

## File Structure

```
vinodh1/
├── README.md                                  # This file
├── Vinodh_FYP_Report.docx                     # Final project report
├── Vinodh_Code_Appendix.txt                   # Code appendix (plain text)
├── adult_income_FINAL_1.ipynb                 # Jupyter notebook with full analysis
├── Income_Classification_Presentation.pptx    # Presentation slides
├── Income Classification — Project Summary.html  # HTML summary
├── Project Handbook Sem B MSc Data Science 2025.docx  # Module handbook
└── report_figures/                            # Figures used in the report
```

## Requirements

- Python 3.9+
- numpy
- pandas
- matplotlib
- scikit-learn
- jupyter

Install with:

```
pip install numpy pandas matplotlib scikit-learn jupyter
```

## How to Run

1. Download `adult.data` from https://archive.ics.uci.edu/dataset/2/adult and place it in the working directory.
2. Open the notebook:

   ```
   jupyter notebook adult_income_FINAL_1.ipynb
   ```

3. Run all cells top to bottom. All code uses `random_state=42` for full reproducibility.

## Ethics

The project uses publicly available secondary data and does not involve human participants, so no ethics committee submission was required. The dataset is released under CC BY 4.0, which permits academic reuse with attribution. Features such as sex and race correlate with income in this 1994 snapshot; these reflect historical structural inequality and must not be used for discriminatory individual-level decisions.

## References

A full list of references with DOIs and URLs is provided at the end of `Vinodh_FYP_Report.docx`. Key sources include:

- Becker, B. and Kohavi, R. (1996) Adult Data Set. UCI Machine Learning Repository. https://doi.org/10.24432/C5XW20
- Breiman, L. (2001) 'Random forests', Machine Learning, 45(1), pp. 5–32. https://doi.org/10.1023/A:1010933404324
- Pedregosa, F. et al. (2011) 'Scikit-learn: Machine learning in Python', JMLR, 12, pp. 2825–2830. https://www.jmlr.org/papers/v12/pedregosa11a.html
- Hosmer, D.W., Lemeshow, S. and Sturdivant, R.X. (2013) Applied Logistic Regression. 3rd edn. Wiley. https://doi.org/10.1002/9781118548387
- James, G., Witten, D., Hastie, T. and Tibshirani, R. (2021) An Introduction to Statistical Learning. 2nd edn. Springer. https://doi.org/10.1007/978-1-0716-1418-1

## Author

MSc Data Science student, University of Hertfordshire — Semester B, 2025.
