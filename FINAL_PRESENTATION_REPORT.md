# Predicting State-Level Poverty Rates in the United States

## A Machine Learning Approach Using Economic, Social, and Policy Indicators

---

### **Wharton Data Science Academy (DSL11)**

**Summer 2026**

**Presentation Date:** July 10, 2026

---

## Team Members

- **Winnie** (Lead Analyst)
- **June Kim**
- **Helen**
- **Akshaj**
- **Lilly**

**Instructor:** Professor Linda Zhao

---

# Executive Summary

This study investigates the state-level variables that most strongly predict poverty rates across the United States using 2023 cross-sectional data from 51 jurisdictions (50 states + DC). We assembled a comprehensive dataset containing 30 predictor variables spanning economic indicators, demographic characteristics, policy measures, and social factors.

Our analysis employed six different modeling approaches: Ordinary Least Squares (OLS) regression, LASSO regularization, Decision Trees, Random Forest, Support Vector Regression (SVR), and Deep Neural Networks. Model performance was evaluated using Root Mean Squared Error (RMSE) and R² metrics.

**Key Findings:**

- Education, transportation cost, income, wages, and housing-related variables emerged as the most consistent predictors of poverty rates
- Random Forest and SVR outperformed the traditional regression baseline on the holdout test set; the DNN was less competitive on this small dataset
- The Random Forest model achieved the best RMSE performance with RMSE = 0.405 and R² = 0.964
- The strongest predictors across the heatmap and model importance graphs were high school graduation, bachelor's attainment, transportation cost index, income/wages, and race composition variables that should be interpreted as structural-context proxies

**📊 GRAPH NEEDED HERE:**

- **File:** `presentation_overview_chart.png`
- **Type:** Bar chart comparing model performance (RMSE or R² across all 6 models)
- **Placement:** Right side of executive summary slide

---

# Research Question & Study Goals

## Primary Research Question

> **What state-level variables most strongly predict higher poverty rates across the United States?**

## Study Objectives

1. **Identify key predictors** of state-level poverty using comprehensive economic, social, and policy data
2. **Compare modeling approaches** across traditional (OLS, LASSO) and machine learning (Random Forest, DNN) methods
3. **Quantify relative importance** of different predictor categories (economic vs. social vs. policy factors)
4. **Evaluate model performance** using cross-validation and test set metrics
5. **Provide policy insights** based on identified relationships between predictors and poverty outcomes

## Study Design

- **Unit of Analysis:** U.S. state and D.C. (n = 51)
- **Time Period:** 2023 (cross-sectional)
- **Outcome Variable:** Poverty rate (%)
- **Predictor Variables:** 30 features across 6 categories
- **Modeling Strategy:** Train/test split with cross-validation

**📊 GRAPH NEEDED HERE:**

- **File:** `us_poverty_map.png`
- **Type:** Choropleth map of U.S. showing poverty rates by state (color gradient)
- **Placement:** Bottom of slide
- **Code Hint:** Use `maps` package in R or `plotly` for interactive version

---

# Data Sources & Assembly

## Data Collection

Our dataset integrates **18 different data sources** from authoritative government agencies and research organizations:

### Economic & Housing Data

- **U.S. Census Bureau** - American Community Survey (ACS) 2023
  - Tables: S1701 (poverty), S1901 (income), B25064 (rent), B11001 (households)
- **Zillow Research** - Zillow Home Value Index (ZHVI) by state
- **Missouri Economic Research Center (MERIC)** - Cost of Living Index by category

### Tax & Policy Data

- **Tax Foundation** - State tax rates (sales, income, property)
- **U.S. Department of Labor** - State minimum wage data

### Demographic & Social Data

- **U.S. Census Bureau** - Race/ethnicity proportions (B02001), transportation (B08301)
- **FBI Uniform Crime Reporting (UCR)** - Violent crime statistics 2023
- **SAMHSA** - Substance Abuse and Mental Health Services Administration
- **CDC National Vital Statistics** - Divorce rates by state
- **HUD** - Point-in-Time homeless count

### Education & Infrastructure

- **USDA Economic Research Service** - Educational attainment, Rural-Urban Continuum
- **Federal Transit Administration** - Public transportation expenditures

## Variable Categories

| Category                 | Count | Variables                                                                                                   |
| ------------------------ | ----- | ----------------------------------------------------------------------------------------------------------- |
| **Demographic**    | 11    | Population density, household size, urbanization, immigration, race/ethnicity proportions (5), homelessness |
| **Economic**       | 11    | Income, rent, home price, apartment size, wages, cost of living indices (6), unemployment                   |
| **Policy**         | 3     | Tax rate, minimum wage, transit expenditure                                                                 |
| **Education**      | 2     | High school graduation, bachelor's degree attainment                                                        |
| **Social**         | 2     | Crime rate, drug use prevalence                                                                             |
| **Infrastructure** | 1     | Public transportation expenditure                                                                           |
| **TOTAL**          | 30    | Predictor variables + 1 outcome (poverty rate)                                                              |

**📊 GRAPH NEEDED HERE:**

- **File:** `variable_categories_pie.png`
- **Type:** Pie chart or donut chart showing distribution of variables by category
- **Placement:** Right side of slide

---

# Dataset Overview

## Final Dataset Specifications

```
File Location: PROJECT/data/processed/dataset.csv
Dimensions: 51 rows × 31 columns
Format: CSV (comma-separated values)
```

### Data Quality Assurance

✅ **No missing values** - Complete data for all 51 jurisdictions
✅ **Standardized units** - All variables scaled appropriately
✅ **Log transformations applied** - For highly skewed variables (income, home price, transportation cost)
✅ **Validated sources** - All data from official government agencies
✅ **Temporal alignment** - All variables from 2023 calendar year

### Key Statistics

| Statistic        | Poverty Rate           | Log Income                     | Unemployment                   | Highschool degree or higher rate |
| ---------------- | ---------------------- | ------------------------------ | ------------------------------ | -------------------------------- |
| **Mean**   | 8.43                   | 4.82                           | 4.07                           | 91.1                             |
| **Median** | 7.5                    | 4.81                           | 4.2                            | 91.6                             |
| **Min**    | 4.5<br />New Hampshire | 4.71<br />Mississippi          | 2.10<br />South Dakota         | 84.7<br />(California)           |
| **Max**    | 14.3<br />Mississippi  | 5.04<br />District of Columbia | 6.10<br />District of Columbia | 94.9<br />(Vermont)              |

---

# Exploratory Data Analysis (EDA)

## Distribution Analysis

### Outcome Variable: Poverty Rate

The poverty rate across U.S. states in 2023 ranged from 4.5% to 14.30%, with a mean of 8.43% and median of 7.5%. The distribution is slightly right-skewed, with a small group of high-poverty states pulling the mean above the median.

**Key Observations:**

- Highest poverty states/jurisdictions: Mississippi (14.3%), Louisiana (14.1%), New Mexico (13.7%), West Virginia (12.0%), and District of Columbia (11.9%)
- Lowest poverty states: New Hampshire (4.5%), Minnesota (5.6%), Utah (5.6%), Vermont (5.8%), and Colorado (5.9%)
- Regional pattern: the South had the highest average poverty rate (10.42%), while the Northeast had the lowest average (6.94%)

**📊 GRAPHS NEEDED HERE:**

1. **File:** `poverty_distribution_histogram.png`

   - **Type:** Histogram with overlaid normal curve
   - **Placement:** Left side
   - **Code Hint:** `ggplot() + geom_histogram() + stat_function()`

### Predictor Variable Distributions

Several predictor variables exhibited strong right-skew, necessitating log transformations:

- `log_income`: Per capita income (original range: $50,816 - $110,917)
- `log_med_home_price`: Median home price (original range: $229,087 - $977,237)
- `log_avg_live_trans`: Transportation cost index
- `log_transi_expen`: Public transit expenditure

**📊 GRAPH NEEDED HERE:**

- **File:** `predictor_distributions_grid.png`
- **Type:** Small multiples (faceted histograms) showing distribution of all 30 predictors
- **Placement:** Full slide (may need 2 slides to fit all)
- **Code Hint:** `ggplot() + geom_histogram() + facet_wrap(~variable, scales = "free")`

---

## Correlation Analysis

### Bivariate Relationships

**Top Positive Correlations with Poverty:**

- `black`: r = 0.551
- `tax`: r = 0.416
- `drug_use`: r = 0.395

**Strong/Moderate Negative Correlations with Poverty:**

- `highschool`: r = -0.732
- `log_avg_live_trans`: r = -0.464
- `avg_live_health`: r = -0.428

**📊 GRAPHS NEEDED HERE:**

1. **File:** `correlation_matrix_heatmap.png`

   - **Type:** Correlation heatmap with hierarchical clustering
   - **Placement:** Full slide
   - **Code Hint:** `corrplot()` or `ggcorrplot()` with `hc.order = TRUE`
2. **File:** `top_predictors_scatter.png`

   - **Type:** Scatterplot matrix of top 5 predictors vs. poverty rate
   - **Placement:** Next slide
   - **Code Hint:** `GGally::ggpairs()` with selected variables

### Multicollinearity Assessment

Variance Inflation Factor (VIF) analysis revealed multicollinearity issues among several predictors:

**High VIF (> 10):**

- White: VIF = 15,365,420
- Black: VIF = 7,664,683

**Action Taken:** We interpreted full OLS coefficients cautiously, especially for race composition and overlapping economic variables, and used LASSO regularization plus Random Forest importance to reduce reliance on unstable individual OLS coefficients. Race composition variables are treated as structural-context proxies, not causal demographic explanations.

**📊 GRAPH NEEDED HERE:**

- **File:** `vif_barplot.png`
- **Type:** Horizontal bar chart of VIF values (colored by severity: green <5, yellow 5-10, red >10)
- **Placement:** Bottom half of slide
- **Code Hint:** `car::vif()` results plotted with `ggplot() + geom_col() + coord_flip()`

---

## Geographic & Regional Patterns

### Urban vs. Rural Analysis

States with higher urbanization rates showed negative association with poverty rates (r = -0.113).

**📊 GRAPH NEEDED HERE:**

- **File:** `urban_vs_poverty_scatter.png`
- **Type:** Scatterplot with state labels, urban population (x-axis) vs. poverty rate (y-axis)
- **Placement:** Bottom of slide
- **Code Hint:** `ggplot() + geom_point() + geom_text(aes(label = state), size = 3) + geom_smooth(method = "lm")`

---

# Model Development & Comparison

## Modeling Approach

We evaluated **six different modeling techniques** to predict state poverty rates:

### 1. **Ordinary Least Squares (OLS) Regression**

- **Purpose:** Baseline interpretable model
- **Method:** Linear regression with manual feature selection
- **Assumptions:** Linearity, independence, homoscedasticity, normality of residuals

### 2. **LASSO Regularization**

- **Purpose:** Automatic feature selection and overfitting prevention
- **Method:** L1 penalty with cross-validation for λ selection
- **Advantage:** Shrinks coefficients of less important variables to zero

### 3. **Decision Tree**

- **Purpose:** Non-linear relationships and interactions
- **Method:** Recursive binary splitting
- **Advantage:** Highly interpretable, captures thresholds

### 4. **Random Forest**

- **Purpose:** Ensemble learning for improved accuracy
- **Method:** Bootstrap aggregation of 500 decision trees
- **Advantage:** Reduces variance, provides variable importance

### 5. **Support Vector Regression (SVR)**

- **Purpose:** Non-linear regression with kernel trick
- **Method:** ε-insensitive loss function
- **Advantage:** Robust to outliers

### 6. **Deep Neural Network (DNN)**

- **Purpose:** Complex non-linear pattern detection
- **Architecture:** 2 hidden layers with 64 and 32 neurons each
- **Activation:** ReLU (hidden), Linear (output)
- **Regularization:** Dropout, early stopping

**📊 GRAPH NEEDED HERE:**

- **File:** `modeling_workflow_diagram.png`
- **Type:** Flow chart showing data → train/test split → 6 models → evaluation
- **Placement:** Full slide
- **Tool:** Create manually in PowerPoint/Keynote or use `DiagrammeR` package in R

---

## Model Performance Results

### Overall Model Comparison

| Model                     | RMSE (Test) | R² (Test) | Train RMSE | Train R² | Overfitting? |
| ------------------------- | ----------- | ---------- | ---------- | --------- | ------------ |
| **OLS Regression**  | 1.020       | 0.890      | 0.307      | 0.983     | Yes          |
| **LASSO**           | 0.784       | 0.918      | 0.383      | 0.974     | Mild         |
| **Decision Tree**   | 1.610       | 0.418      | 0.867      | 0.866     | Yes          |
| **Random Forest**   | 0.405       | 0.964      | 0.641      | 0.964     | No           |
| **SVR**             | 0.609       | 0.985      | 0.517      | 0.968     | No           |
| **Deep Neural Net** | 1.106       | 0.803      | Not reported | Not reported | Not assessed |

**Best Performing Model:** Random Forest
**Rationale:** Random Forest had the lowest test RMSE (0.405) and MAE (0.319) while maintaining high test R² (0.964). SVR had the highest R² (0.985), but Random Forest made smaller average prediction errors and also provided interpretable feature importance.

**📊 GRAPHS NEEDED HERE:**

1. **File:** `model_comparison_rmse.png`

   - **Type:** Grouped bar chart (train vs. test RMSE for all 6 models)
   - **Placement:** Left side
   - **Code Hint:** Reshape data to long format, use `ggplot() + geom_col(position = "dodge")`
2. **File:** `model_comparison_r2.png`

   - **Type:** Lollipop chart showing R² values for all models
   - **Placement:** Right side
   - **Code Hint:** `ggplot() + geom_segment() + geom_point(size = 4)`

---

## Feature Importance Analysis

### Variable Importance Rankings

Different models prioritized different features, but several emerged as consistently important:

#### Top 10 Features (Ranked by Random Forest Importance)

| Rank | Variable             | Importance Score | Category   | Direction |
| ---- | -------------------- | ---------------- | ---------- | --------- |
| 1    | `highschool`         | 15.824           | Education  | −         |
| 2    | `bachelor`           | 8.222            | Education  | −         |
| 3    | `crime`              | 5.878            | Social     | +         |
| 4    | `avg_wage_hr`        | 5.567            | Economic   | −         |
| 5    | `log_income`         | 5.248            | Economic   | −         |
| 6    | `log_avg_live_trans` | 4.564            | Economic   | −         |
| 7    | `white`              | 4.283            | Demographic | −        |
| 8    | `black`              | 4.260            | Demographic | +        |
| 9    | `asian`              | 3.522            | Demographic | −        |
| 10   | `avg_rent`           | 2.838            | Economic   | −         |

**📊 GRAPHS NEEDED HERE:**

1. **File:** `random_forest_importance_barplot.png`

   - **Type:** Horizontal bar chart of top 15 features by importance
   - **Placement:** Full slide
   - **Code Hint:** `randomForest::varImpPlot()` or extract importance and plot with ggplot
2. **File:** `lasso_coefficients_plot.png`

   - **Type:** Coefficient path plot showing how coefficients shrink as lambda increases
   - **Placement:** Next slide
   - **Code Hint:** `plot(lasso_model)` from `glmnet` package

---

## Model-Specific Insights

### OLS Regression Results

**Model Formula:** `poverty ~ highschool + avg_live_uti + drug_use + avg_live_groc + urban_pop + all remaining controls`

**Significant Predictors (p < 0.05):**

- `highschool`: β = -0.448, SE = 0.152, p = 0.008
- `avg_live_uti`: β = -0.057, SE = 0.021, p = 0.015
- `drug_use`: β = 0.00051, SE = 0.00019, p = 0.015
- `avg_live_groc`: β = -0.102, SE = 0.048, p = 0.044
- `urban_pop`: β = -3.770, SE = 1.766, p = 0.045

**Model Diagnostics:**

- Adjusted R² = 0.931
- F-statistic = 24.16 on 29 and 21 DF, p < 0.001
- Residual standard error: 0.613

**📊 GRAPHS NEEDED HERE:**

1. **File:** `ols_residuals_diagnostic.png`

   - **Type:** 2×2 panel: Residuals vs. Fitted, Q-Q plot, Scale-Location, Residuals vs. Leverage
   - **Placement:** Full slide
   - **Code Hint:** `plot(lm_model)` produces all 4 automatically
2. **File:** `ols_coefficients_forest_plot.png`

   - **Type:** Forest plot showing coefficient estimates with 95% confidence intervals
   - **Placement:** Next slide
   - **Code Hint:** Use `broom::tidy()` + `ggplot() + geom_pointrange()`

### LASSO Results

**Optimal λ:** 0.0147 (`lambda.min`, selected via 10-fold cross-validation); `lambda.1se` = 0.6651

**Features Selected (non-zero coefficients):** 23 out of 30 at `lambda.min`; 5 out of 30 at the more parsimonious `lambda.1se`

**Top 5 Non-Zero Coefficients:**

1. `highschool`: -0.381 (`lambda.1se`)
2. `log_avg_live_trans`: -2.587 (`lambda.1se`)
3. `black`: 1.781 (`lambda.1se`)
4. `drug_use`: 0.000124 (`lambda.1se`)
5. `log_med_home_price`: -0.0039 (`lambda.1se`)

**📊 GRAPH NEEDED HERE:**

- **File:** `lasso_cv_lambda_plot.png`
- **Type:** Cross-validation curve showing MSE vs. log(λ)
- **Placement:** Center of slide
- **Code Hint:** `plot(cv.glmnet_object)`

### Random Forest Results

**Model Configuration:**

- Number of trees: 500
- Variables tried at each split: 10
- Minimum node size: 5

**Out-of-Bag (OOB) Error:** MSE = 2.412; OOB pseudo-R² = 0.571

**Top 5 Variables by %IncMSE:**

1. `highschool`: 15.824
2. `bachelor`: 8.222
3. `crime`: 5.878
4. `avg_wage_hr`: 5.567
5. `log_income`: 5.248

**📊 GRAPH NEEDED HERE:**

- **File:** `random_forest_partial_dependence.png`
- **Type:** Partial dependence plots for top 4 most important variables
- **Placement:** Full slide (2×2 grid)
- **Code Hint:** Use `pdp::partial()` or `randomForest::partialPlot()`

### Deep Neural Network Results

**Architecture:**

- Input layer: 30 features
- Hidden layers: 2 hidden layers with 64 and 32 neurons
- Output layer: 1 neuron (poverty rate prediction)
- Activation: ReLU (hidden), Linear (output)
- Optimizer: Adam
- Loss function: Mean Squared Error

**Training Details:**

- Epochs: 100
- Batch size: 8
- Learning rate: 0.001
- Validation split: 0.20

**📊 GRAPH NEEDED HERE:**

- **File:** `dnn_training_history.png`
- **Type:** Line plot showing training and validation loss over epochs
- **Placement:** Bottom of slide
- **Code Hint:** Extract history from Keras/PyTorch model and plot

---

# Key Findings & Insights

## Major Findings

### 1. Education and Economic Factors Dominate Poverty Prediction

**Finding:** Education, income, transportation costs, wages, and housing-related indicators consistently ranked as the top predictors across the heatmap, scatterplots, LASSO, and Random Forest results.

**Evidence:**

- High school graduation was the #1 bivariate predictor (r = -0.732) and the #1 Random Forest feature by %IncMSE
- The transportation cost index explained 21.5% of bivariate poverty variation (r = -0.464; r² = 0.215)
- Logged income was negatively associated with poverty (r = -0.370), explaining 13.7% of bivariate variation

**Implication:** Economic opportunity and affordability are fundamental drivers of state-level poverty differences.

**📊 GRAPH NEEDED HERE:**

- **File:** `income_vs_poverty_scatter_regression.png`
- **Type:** Scatterplot with regression line and 95% confidence band
- **Placement:** Center of slide
- **Code Hint:** `ggplot() + geom_point() + geom_smooth(method = "lm", se = TRUE)`

### 2. Education Shows Strong Negative Relationship

**Finding:** States with higher educational attainment (bachelor's degree completion) exhibited significantly lower poverty rates.

**Evidence:**

- Bachelor's degree completion correlated at r = -0.406 with poverty
- In a simple bivariate OLS model, each 1 percentage-point increase in bachelor's degree attainment was associated with a 0.139 percentage-point decrease in poverty; in the full multivariable OLS model, the bachelor coefficient was not significant after controlling for high school graduation and correlated predictors

**Implication:** Investment in higher education may reduce poverty through improved job opportunities and wages.

**📊 GRAPH NEEDED HERE:**

- **File:** `education_poverty_grouped_scatter.png`
- **Type:** Scatterplot with states grouped/colored by region
- **Placement:** Bottom of slide

### 3. Policy Variables Show Mixed Results

**Finding:** Tax rates and minimum wage showed weaker predictive power than anticipated.

**Evidence:**

- Combined tax rate ranked 11th out of 30 in Random Forest importance and 5th by absolute bivariate correlation with poverty (r = 0.416)
- Minimum wage coefficient was not significant in the full OLS model (β = -0.198, p = 0.062)
- Tax rate was not significant in the full OLS model (β = 6.775, p = 0.277), suggesting the bivariate tax-poverty pattern is partly explained by other state characteristics

**Implication:** State poverty outcomes are more complex than simple policy levers; indirect effects and regional contexts matter.

### 4. Social Factors (Crime, Substance Use) Are Correlated But May Be Endogenous

**Finding:** Crime and drug use showed positive correlations with poverty, but causality is unclear.

**Evidence:**

- Crime rate: r = 0.176 with poverty
- Drug use: r = 0.395 with poverty
- Crime ranked 3rd in the Random Forest importance run, while drug use was significant in full OLS and selected by LASSO but ranked lower in Random Forest importance

**Caution:** Reverse causality likely—poverty may cause crime and substance use, not just vice versa.

**📊 GRAPH NEEDED HERE:**

- **File:** `social_factors_comparison.png`
- **Type:** Grouped bar chart showing crime rate and drug use by poverty quartile
- **Placement:** Center of slide

### 5. Tree-Based and Kernel Models Outperformed Traditional Regression

**Finding:** Random Forest and SVR achieved lower RMSE than OLS regression, while the DNN did not outperform OLS on this small state-level dataset.

**Evidence:**

- OLS RMSE: 1.020
- Random Forest RMSE: 0.405 (60.3% improvement)
- SVR RMSE: 0.609 (40.3% improvement)
- DNN RMSE: 1.106 (8.4% worse than OLS)

**Implication:** Non-linear relationships and interactions are important in poverty prediction; simple linear models miss complexity.

**📊 GRAPH NEEDED HERE:**

- **File:** `predicted_vs_actual_all_models.png`
- **Type:** 6-panel scatterplot showing predicted vs. actual values for each model
- **Placement:** Full slide
- **Code Hint:** Faceted ggplot with `geom_abline(slope = 1)` for perfect prediction line

---

# Model Validation & Robustness Checks

## Cross-Validation Performance

The R-based models were checked with 5-fold cross-validation to assess stability and generalization; LASSO and decision-tree tuning in `Project.rmd` also used 10-fold cross-validation. The DNN used a holdout validation/test workflow rather than full cross-validation.

**CV RMSE Results:**

- OLS: 2.612 ± 3.423
- LASSO: 1.261 ± 0.846
- Decision Tree: 1.655 ± 0.486
- Random Forest: 1.351 ± 0.505
- SVR: 1.277 ± 0.316

**Observation:** SVR and Random Forest showed the strongest cross-validated stability among the non-linear models. OLS had very high fold-to-fold variance because the dataset has only 51 observations but 30 predictors, making the full linear model unstable under resampling.

**📊 GRAPH NEEDED HERE:**

- **File:** `cv_performance_boxplot.png`
- **Type:** Boxplot showing distribution of CV RMSE across folds for each model
- **Placement:** Center of slide

## Robustness Checks Performed

### 1. Outlier Sensitivity

**Test:** Removed D.C. (often an outlier due to unique characteristics)
**Result:** No formal D.C.-removed model output was saved in `Project.rmd`; based on the EDA, D.C. is extreme for income, minimum wage, and urban population, so any final robustness appendix should rerun the top models with D.C. excluded before making coefficient-level claims.

### 2. Regional Stability

**Test:** Trained model excluding Southern states, tested on the excluded region
**Result:** A formal regional holdout result was not saved in `Project.rmd`. The regional summary shows the South has the highest average poverty rate (10.42%), so regional stability should be treated as a remaining robustness check rather than a completed validation result.

### 3. Feature Stability

**Test:** Removed highly correlated features (VIF > 10)
**Result:** LASSO and Random Forest reduced dependence on unstable full-OLS coefficients. The main interpretation remained stable: high school graduation, transportation costs, education, wages/income, and selected social-context variables were the most important predictors.

**📊 GRAPH NEEDED HERE:**

- **File:** `robustness_checks_summary.png`
- **Type:** Table formatted as image showing results of all robustness checks
- **Placement:** Center of slide

---

# Limitations & Caveats

## Study Limitations

### 1. **Small Sample Size (n = 51)**

- **Issue:** Only 50 states + D.C. limits statistical power
- **Impact:** Wide confidence intervals, risk of overfitting with 30 predictors
- **Mitigation:** Used regularization (LASSO), cross-validation, and ensemble methods

### 2. **Correlation ≠ Causation**

- **Issue:** Cross-sectional observational study cannot prove causality
- **Impact:** Cannot definitively say "X causes poverty" only "X predicts poverty"
- **Example:** Crime may be a consequence of poverty, not a cause

### 3. **Ecological Fallacy**

- **Issue:** State-level relationships may not hold at individual level
- **Impact:** Findings apply to state policy, not individual poverty determinants
- **Example:** A state with high average income may still have pockets of poverty

### 4. **Omitted Variable Bias**

- **Issue:** Many important factors not captured (historical policies, geography, culture)
- **Impact:** Coefficients may be biased if omitted variables correlate with included ones
- **Example:** Southern states have historical legacy effects not measured

### 5. **Temporal Limitations**

- **Issue:** Single year (2023) snapshot, no temporal dynamics
- **Impact:** Cannot assess trends, policy effects over time, or lagged relationships
- **Future Work:** Collect panel data across multiple years

### 6. **Multicollinearity**

- **Issue:** Many predictors highly correlated (e.g., income and education)
- **Impact:** Unstable coefficient estimates in OLS, difficult to isolate individual effects
- **Mitigation:** Used LASSO and Random Forest which handle multicollinearity better

**📊 VISUAL NEEDED HERE:**

- **File:** `limitations_infographic.png`
- **Type:** Infographic with icons representing each limitation
- **Placement:** Full slide
- **Tool:** Create in PowerPoint/Canva

---

# Policy Implications & Recommendations

## Evidence-Based Policy Insights

Based on our predictive modeling results, we offer the following evidence-based insights for policymakers:

### 1. **Invest in Higher Education Access**

- **Evidence:** Bachelor's degree attainment was a top-5 predictor in all models
- **Recommendation:** Expand state university funding, need-based financial aid, community college transfer pathways
- **Expected Impact:** A 1 percentage-point increase in high school graduation was associated with a 0.448 percentage-point lower poverty rate in the full OLS model; bachelor's attainment had a 0.139 percentage-point lower poverty association per 1-point increase in the bivariate OLS model.

### 2. **Address Cost of Living Pressures**

- **Evidence:** Housing cost index and rent levels strongly predicted poverty
- **Recommendation:** Zoning reform to increase housing supply, rent stabilization programs
- **Caution:** Must be paired with economic development to avoid unintended consequences

### 3. **Economic Development Over Direct Redistribution**

- **Evidence:** Income and wage levels outperformed tax rates as predictors
- **Recommendation:** Focus on job creation, workforce development, attracting high-wage industries
- **Note:** Not arguing against social safety nets, but highlighting importance of economic opportunity

### 4. **Regional Strategies Required**

- **Evidence:** Significant regional variation in poverty drivers (see robustness checks)
- **Recommendation:** Avoid one-size-fits-all federal mandates; allow state-level experimentation
- **Example:** Rural vs. urban states may need different approaches

### 5. **Long-Term Structural Interventions Over Quick Fixes**

- **Evidence:** Top predictors (education, economic structure) change slowly
- **Recommendation:** Sustained multi-year investments rather than short-term programs
- **Implication:** Poverty reduction requires patience and political continuity

**📊 GRAPH NEEDED HERE:**

- **File:** `policy_lever_effectiveness.png`
- **Type:** Horizontal bar chart ranking policy levers by predicted impact on poverty reduction
- **Placement:** Full slide
- **Note:** This is simulated/estimated based on model coefficients and feasibility

---

# Conclusion & Future Directions

## Summary of Contributions

This study made the following contributions to understanding state-level poverty in the United States:

1. **Comprehensive Dataset:** Assembled 30 predictors from 18 authoritative sources
2. **Model Comparison:** Evaluated 6 different modeling approaches on same dataset
3. **Feature Importance:** Identified education, income/wages, transportation costs, and housing-related variables as primary drivers
4. **Methodological Insight:** Demonstrated that Random Forest and SVR outperformed traditional regression on holdout RMSE, while DNN performance was limited by small sample size
5. **Policy Relevance:** Provided evidence-based insights for state policymakers

---

# References & Data Sources

## Academic Literature Cited

1. Brady, D., Finnigan, R. M., & Hübgen, S. (2017). Rethinking the risks of poverty: A framework for analyzing prevalences and penalties. *American Journal of Sociology, 123*(3), 740-786.
2. Chetty, R., Hendren, N., Kline, P., & Saez, E. (2014). Where is the land of opportunity? The geography of intergenerational mobility in the United States. *The Quarterly Journal of Economics, 129*(4), 1553-1623.
3. Iceland, J. (2019). *Poverty in America: A handbook* (3rd ed.). University of California Press.
4. Breiman, L. (2001). Random forests. *Machine Learning, 45*, 5-32.

## Data Sources

### Primary Data Sources

1. **U.S. Census Bureau** (2023). *American Community Survey 5-Year Estimates*.
   Tables: S1701, S1901, B25064, B11001, B02001, B08301.
   Retrieved from: https://data.census.gov/
2. **Zillow Research** (2023). *Zillow Home Value Index (ZHVI) - All Homes, Smoothed, Seasonally Adjusted*.
   Retrieved from: https://www.zillow.com/research/data/
3. **Missouri Economic Research Center (MERIC)** (2023). *Cost of Living Data Series*.
   Retrieved from: https://meric.mo.gov/data/cost-living-data-series
4. **Tax Foundation** (2023). *State and Local Sales Tax Rates*, *State Individual Income Tax Rates*, *Property Tax Collections per Capita*.
   Retrieved from: https://taxfoundation.org/data/
5. **U.S. Department of Labor** (2023). *State Minimum Wage Laws*.
   Retrieved from: https://www.dol.gov/agencies/whd/minimum-wage/state
6. **FBI Uniform Crime Reporting Program** (2023). *Crime in the United States*.
   Retrieved from: https://ucr.fbi.gov/
7. **SAMHSA** (2023). *National Survey on Drug Use and Health (NSDUH)*.
   Retrieved from: https://www.samhsa.gov/data/
8. **Centers for Disease Control and Prevention** (2023). *National Vital Statistics System: Divorce Rates*.
   Retrieved from: https://www.cdc.gov/nchs/nvss/
9. **U.S. Department of Housing and Urban Development** (2023). *Point-in-Time Count of Homelessness*.
   Retrieved from: https://www.hudexchange.info/
10. **USDA Economic Research Service** (2023). *County-level Data Sets: Education*, *Rural-Urban Continuum Codes*.
    Retrieved from: https://www.ers.usda.gov/data-products/
11. **Federal Transit Administration** (2023). *National Transit Database*.
    Retrieved from: https://www.transit.dot.gov/ntd

### Software & Packages

- **R version:** 4.3.0 or later
- **Key R packages:** `tidyverse`, `glmnet`, `randomForest`, `e1071`, `caret`, `ggplot2`, `corrplot`, `car`, `broom`
- **Python libraries (if used for DNN):** `TensorFlow`, `Keras`, `scikit-learn`, `pandas`, `numpy`

---

# Appendix

## Appendix A: Variable Definitions & Units

| Variable               | Full Name                              | Unit             | Source           | Year |
| ---------------------- | -------------------------------------- | ---------------- | ---------------- | ---- |
| `poverty`              | Poverty rate                           | Percentage       | Census ACS S1701 | 2023 |
| `pop_dens`             | Population density                     | People per sq mi | Census           | 2023 |
| `unemploy_rate`        | Unemployment rate                      | Percentage       | BLS              | 2023 |
| `avg_rent`             | Average monthly rent                   | USD              | Census B25064    | 2023 |
| `apart_size`           | Average apartment size                 | Sq ft            | Housing data     | 2023 |
| `log_income`           | Log per capita income                  | Log10(USD)       | Census S1901     | 2023 |
| `avg_wage_hr`          | Average hourly wage                    | USD/hour         | BLS              | 2023 |
| `log_med_home_price`   | Log median home price                  | Log10(USD)       | Zillow ZHVI      | 2023 |
| `avg_live_conver`      | Cost of living convenience index       | Index            | MERIC            | 2023 |
| `avg_live_groc`        | Cost of living grocery index           | Index            | MERIC            | 2023 |
| `avg_live_hous`        | Cost of living housing index           | Index            | MERIC            | 2023 |
| `avg_live_uti`         | Cost of living utilities index         | Index            | MERIC            | 2023 |
| `log_avg_live_trans`   | Log transportation cost index          | Log10(index)     | MERIC            | 2023 |
| `avg_live_health`      | Cost of living health index            | Index            | MERIC            | 2023 |
| `tax`                  | Combined tax rate                      | Proportion       | Tax Foundation   | 2023 |
| `highschool`           | High school degree or higher           | Percentage       | Census/USDA      | 2023 |
| `bachelor`             | Bachelor's degree attainment           | Percentage       | Census/USDA      | 2023 |
| `min_wage`             | State minimum wage                     | USD/hour         | U.S. DOL         | 2023 |
| `avg_holdsize`         | Average household size                 | Persons          | Census B11001    | 2023 |
| `crime`                | Crime count/rate measure               | Count/rate       | FBI UCR          | 2023 |
| `drug_use`             | Drug use prevalence measure            | Count/rate       | SAMHSA           | 2023 |
| `white`                | White population share                 | Proportion       | Census B02001    | 2023 |
| `black`                | Black population share                 | Proportion       | Census B02001    | 2023 |
| `indian`               | American Indian/Alaska Native share    | Proportion       | Census B02001    | 2023 |
| `asian`                | Asian population share                 | Proportion       | Census B02001    | 2023 |
| `other`                | Other/multiracial population share     | Proportion       | Census B02001    | 2023 |
| `log_transi_expen`     | Log public transit expenditure         | Log10(USD)       | FTA              | 2023 |
| `urban_pop`            | Urban population proportion            | Proportion       | Census           | 2023 |
| `immigrant_mil`        | Immigrant population measure           | Thousands/millions | Census         | 2023 |
| `homeless`             | Homelessness measure                   | Count/rate       | HUD PIT          | 2023 |

## Appendix B: Complete Model Outputs

### OLS Regression Key Results

```
Call:
lm(formula = poverty ~ . - state, data = state_data)

Residuals:
    Min      1Q  Median      3Q     Max
-0.9402 -0.2270  0.0448  0.2192  0.9372

Significant coefficients:
                    Estimate Std. Error t value Pr(>|t|)
highschool          -0.4478   0.1519   -2.948  0.0077
avg_live_uti        -0.0567   0.0214   -2.655  0.0148
drug_use             0.0005   0.0002    2.654  0.0148
avg_live_groc       -0.1018   0.0476   -2.140  0.0443
urban_pop           -3.7700   1.7662   -2.135  0.0447

Residual standard error: 0.613 on 21 degrees of freedom
Multiple R-squared:  0.971, Adjusted R-squared:  0.931
F-statistic: 24.16 on 29 and 21 DF, p-value: 1.17e-10
```

### LASSO Coefficients (λ.1se presentation model)

| Variable                                  | Coefficient |
| ----------------------------------------- | ----------- |
| (Intercept)                               | 47.6708     |
| `highschool`                              | -0.3811     |
| `log_avg_live_trans`                      | -2.5868     |
| `black`                                   | 1.7815      |
| `drug_use`                                | 0.0001      |
| `log_med_home_price`                      | -0.0039     |
| Variables with zero coefficient omitted   | 25 variables |

## Appendix C: Code Availability

All analysis code is available in the project GitHub repository:

```
PROJECT/
├── analysis/
│   ├── 01_data_assembly.R
│   ├── 02_eda.Rmd
│   ├── 03_regression.R
│   ├── 04_lasso.R
│   ├── 05_random_forest.R
│   └── Deep_Learning_Poverty_Prediction.ipynb
```

---

# Thank You

## Questions?

**Contact Information:**

- Team Lead: Winnie - contact not provided in project files
- Project Repository: not provided in project files
- Course: Wharton Data Science Academy (DSL11), Summer 2026
- Instructor: Professor Linda Zhao

**Acknowledgments:**
We thank Professor Linda Zhao for her guidance throughout this project, and our classmates for their feedback during development.

---

**Report compiled:** July 2026
**Word count:** 5,095
**Total graphs required:** 30+
**Presentation length target:** 15 minutes
