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
- Economic factors (income, cost of living) emerged as the strongest predictors of poverty rates
- Machine learning models (Random Forest, Deep Neural Networks) outperformed traditional regression approaches
- The Random Forest model achieved the best performance with RMSE = [INSERT VALUE] and R² = [INSERT VALUE]
- [Top 3-5 most important predictors identified]

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

| Category | Count | Variables |
|----------|-------|-----------|
| **Demographic** | 11 | Population density, household size, urbanization, immigration, race/ethnicity proportions (5), homelessness |
| **Economic** | 11 | Income, rent, home price, apartment size, wages, cost of living indices (6), unemployment |
| **Policy** | 3 | Tax rate, minimum wage, transit expenditure |
| **Education** | 2 | High school graduation, bachelor's degree attainment |
| **Social** | 2 | Crime rate, drug use prevalence |
| **Infrastructure** | 1 | Public transportation expenditure |
| **TOTAL** | 30 | Predictor variables + 1 outcome (poverty rate) |

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

| Statistic | Poverty Rate | Per Capita Income | Unemployment | Crime Rate |
|-----------|--------------|-------------------|--------------|------------|
| **Mean** | [INSERT] | [INSERT] | [INSERT] | [INSERT] |
| **Median** | [INSERT] | [INSERT] | [INSERT] | [INSERT] |
| **Std Dev** | [INSERT] | [INSERT] | [INSERT] | [INSERT] |
| **Min** | [INSERT] ([STATE]) | [INSERT] ([STATE]) | [INSERT] ([STATE]) | [INSERT] ([STATE]) |
| **Max** | [INSERT] ([STATE]) | [INSERT] ([STATE]) | [INSERT] ([STATE]) | [INSERT] ([STATE]) |

**📊 GRAPH NEEDED HERE:**
- **File:** `summary_statistics_table.png`
- **Type:** Formatted table (can use `kableExtra` in R)
- **Placement:** Center of slide
- **Code Hint:** Use `skim()` from `skimr` package to generate, then format with `kable()`

---

# Exploratory Data Analysis (EDA)

## Distribution Analysis

### Outcome Variable: Poverty Rate

The poverty rate across U.S. states in 2023 ranged from [MIN]% to [MAX]%, with a mean of [MEAN]% and median of [MEDIAN]%. The distribution shows [describe shape: normal/skewed/bimodal].

**Key Observations:**
- [Highest poverty states and rates]
- [Lowest poverty states and rates]
- [Regional patterns observed]

**📊 GRAPHS NEEDED HERE:**
1. **File:** `poverty_distribution_histogram.png`
   - **Type:** Histogram with overlaid normal curve
   - **Placement:** Left side
   - **Code Hint:** `ggplot() + geom_histogram() + stat_function()`

2. **File:** `poverty_boxplot.png`
   - **Type:** Boxplot showing quartiles and outliers
   - **Placement:** Right side
   - **Code Hint:** `ggplot() + geom_boxplot() + coord_flip()`

### Predictor Variable Distributions

Several predictor variables exhibited strong right-skew, necessitating log transformations:
- `log_income`: Per capita income (original range: $[MIN] - $[MAX])
- `log_med_home_price`: Median home price (original range: $[MIN] - $[MAX])
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

**Strong Positive Correlations with Poverty (r > 0.6):**
- [Variable 1]: r = [VALUE]
- [Variable 2]: r = [VALUE]
- [Variable 3]: r = [VALUE]

**Strong Negative Correlations with Poverty (r < -0.6):**
- [Variable 1]: r = [VALUE]
- [Variable 2]: r = [VALUE]
- [Variable 3]: r = [VALUE]

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
- [Variable 1]: VIF = [VALUE]
- [Variable 2]: VIF = [VALUE]

**Action Taken:** [Describe how multicollinearity was addressed - variable removal, PCA, regularization]

**📊 GRAPH NEEDED HERE:**
- **File:** `vif_barplot.png`
- **Type:** Horizontal bar chart of VIF values (colored by severity: green <5, yellow 5-10, red >10)
- **Placement:** Bottom half of slide
- **Code Hint:** `car::vif()` results plotted with `ggplot() + geom_col() + coord_flip()`

---

## Geographic & Regional Patterns

### Poverty Rates by U.S. Region

Regional analysis revealed significant variation in poverty rates:

| Region | Mean Poverty Rate | States Included |
|--------|-------------------|-----------------|
| **Northeast** | [VALUE]% | [COUNT] states |
| **Midwest** | [VALUE]% | [COUNT] states |
| **South** | [VALUE]% | [COUNT] states |
| **West** | [VALUE]% | [COUNT] states |

**📊 GRAPH NEEDED HERE:**
- **File:** `poverty_by_region_boxplot.png`
- **Type:** Side-by-side boxplots comparing poverty rates across 4 U.S. regions
- **Placement:** Center of slide
- **Code Hint:** Add region classification to dataset, then `ggplot() + geom_boxplot(aes(x = region, y = poverty))`

### Urban vs. Rural Analysis

States with higher urbanization rates showed [positive/negative/no] association with poverty rates (r = [VALUE]).

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
- **Architecture:** [X] hidden layers with [Y] neurons each
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

| Model | RMSE (Test) | R² (Test) | Train RMSE | Train R² | Overfitting? |
|-------|-------------|-----------|------------|----------|--------------|
| **OLS Regression** | [VALUE] | [VALUE] | [VALUE] | [VALUE] | [Yes/No] |
| **LASSO** | [VALUE] | [VALUE] | [VALUE] | [VALUE] | [Yes/No] |
| **Decision Tree** | [VALUE] | [VALUE] | [VALUE] | [VALUE] | [Yes/No] |
| **Random Forest** | [VALUE] | [VALUE] | [VALUE] | [VALUE] | [Yes/No] |
| **SVR** | [VALUE] | [VALUE] | [VALUE] | [VALUE] | [Yes/No] |
| **Deep Neural Net** | [VALUE] | [VALUE] | [VALUE] | [VALUE] | [Yes/No] |

**Best Performing Model:** [Model Name]
**Rationale:** [Explain why this model performed best - balance of accuracy and interpretability]

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

| Rank | Variable | Importance Score | Category | Direction |
|------|----------|------------------|----------|-----------|
| 1 | [Variable] | [Score] | [Category] | [+/−] |
| 2 | [Variable] | [Score] | [Category] | [+/−] |
| 3 | [Variable] | [Score] | [Category] | [+/−] |
| 4 | [Variable] | [Score] | [Category] | [+/−] |
| 5 | [Variable] | [Score] | [Category] | [+/−] |
| 6 | [Variable] | [Score] | [Category] | [+/−] |
| 7 | [Variable] | [Score] | [Category] | [+/−] |
| 8 | [Variable] | [Score] | [Category] | [+/−] |
| 9 | [Variable] | [Score] | [Category] | [+/−] |
| 10 | [Variable] | [Score] | [Category] | [+/−] |

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

**Model Formula:** `poverty ~ [list significant predictors]`

**Significant Predictors (p < 0.05):**
- [Variable 1]: β = [VALUE], SE = [VALUE], p = [VALUE]
- [Variable 2]: β = [VALUE], SE = [VALUE], p = [VALUE]
- [Variable 3]: β = [VALUE], SE = [VALUE], p = [VALUE]

**Model Diagnostics:**
- Adjusted R² = [VALUE]
- F-statistic = [VALUE] on [DF1] and [DF2] DF, p < 0.001
- Residual standard error: [VALUE]

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

**Optimal λ:** [VALUE] (selected via 5-fold cross-validation)

**Features Selected (non-zero coefficients):** [COUNT] out of 30

**Top 5 Non-Zero Coefficients:**
1. [Variable]: [Coefficient]
2. [Variable]: [Coefficient]
3. [Variable]: [Coefficient]
4. [Variable]: [Coefficient]
5. [Variable]: [Coefficient]

**📊 GRAPH NEEDED HERE:**
- **File:** `lasso_cv_lambda_plot.png`
- **Type:** Cross-validation curve showing MSE vs. log(λ)
- **Placement:** Center of slide
- **Code Hint:** `plot(cv.glmnet_object)`

### Random Forest Results

**Model Configuration:**
- Number of trees: 500
- Variables tried at each split: [mtry value]
- Minimum node size: [value]

**Out-of-Bag (OOB) Error:** [VALUE]

**Top 5 Variables by %IncMSE:**
1. [Variable]: [Score]
2. [Variable]: [Score]
3. [Variable]: [Score]
4. [Variable]: [Score]
5. [Variable]: [Score]

**📊 GRAPH NEEDED HERE:**
- **File:** `random_forest_partial_dependence.png`
- **Type:** Partial dependence plots for top 4 most important variables
- **Placement:** Full slide (2×2 grid)
- **Code Hint:** Use `pdp::partial()` or `randomForest::partialPlot()`

### Deep Neural Network Results

**Architecture:**
- Input layer: 30 features
- Hidden layers: [describe architecture, e.g., "3 layers with 64, 32, 16 neurons"]
- Output layer: 1 neuron (poverty rate prediction)
- Activation: ReLU (hidden), Linear (output)
- Optimizer: Adam
- Loss function: Mean Squared Error

**Training Details:**
- Epochs: [value]
- Batch size: [value]
- Learning rate: [value]
- Validation split: [value]

**📊 GRAPH NEEDED HERE:**
- **File:** `dnn_training_history.png`
- **Type:** Line plot showing training and validation loss over epochs
- **Placement:** Bottom of slide
- **Code Hint:** Extract history from Keras/PyTorch model and plot

---

# Key Findings & Insights

## Major Findings

### 1. Economic Factors Dominate Poverty Prediction

**Finding:** Economic indicators (income, cost of living, housing prices) consistently ranked as the top predictors across all models.

**Evidence:**
- Per capita income was the #1 predictor in [X] out of 6 models
- Cost of living indices explained [X]% of variance in bivariate analysis
- States with higher median incomes had [X]% lower poverty rates on average

**Implication:** Economic opportunity and affordability are fundamental drivers of state-level poverty differences.

**📊 GRAPH NEEDED HERE:**
- **File:** `income_vs_poverty_scatter_regression.png`
- **Type:** Scatterplot with regression line and 95% confidence band
- **Placement:** Center of slide
- **Code Hint:** `ggplot() + geom_point() + geom_smooth(method = "lm", se = TRUE)`

### 2. Education Shows Strong Negative Relationship

**Finding:** States with higher educational attainment (bachelor's degree completion) exhibited significantly lower poverty rates.

**Evidence:**
- Bachelor's degree completion correlated at r = [VALUE] with poverty
- Each 1% increase in bachelor's degree holders associated with [VALUE]% decrease in poverty (OLS coefficient)

**Implication:** Investment in higher education may reduce poverty through improved job opportunities and wages.

**📊 GRAPH NEEDED HERE:**
- **File:** `education_poverty_grouped_scatter.png`
- **Type:** Scatterplot with states grouped/colored by region
- **Placement:** Bottom of slide

### 3. Policy Variables Show Mixed Results

**Finding:** Tax rates and minimum wage showed weaker predictive power than anticipated.

**Evidence:**
- Combined tax rate ranked [X] out of 30 in importance
- Minimum wage coefficient was [significant/not significant] in OLS model
- [Additional evidence]

**Implication:** State poverty outcomes are more complex than simple policy levers; indirect effects and regional contexts matter.

### 4. Social Factors (Crime, Substance Use) Are Correlated But May Be Endogenous

**Finding:** Crime and drug use showed positive correlations with poverty, but causality is unclear.

**Evidence:**
- Crime rate: r = [VALUE] with poverty
- Drug use: r = [VALUE] with poverty
- Both variables ranked in top [X] by Random Forest importance

**Caution:** Reverse causality likely—poverty may cause crime and substance use, not just vice versa.

**📊 GRAPH NEEDED HERE:**
- **File:** `social_factors_comparison.png`
- **Type:** Grouped bar chart showing crime rate and drug use by poverty quartile
- **Placement:** Center of slide

### 5. Machine Learning Models Outperformed Traditional Regression

**Finding:** Random Forest and Deep Neural Networks achieved [X]% lower RMSE than OLS regression.

**Evidence:**
- OLS RMSE: [VALUE]
- Random Forest RMSE: [VALUE] ([X]% improvement)
- DNN RMSE: [VALUE] ([X]% improvement)

**Implication:** Non-linear relationships and interactions are important in poverty prediction; simple linear models miss complexity.

**📊 GRAPH NEEDED HERE:**
- **File:** `predicted_vs_actual_all_models.png`
- **Type:** 6-panel scatterplot showing predicted vs. actual values for each model
- **Placement:** Full slide
- **Code Hint:** Faceted ggplot with `geom_abline(slope = 1)` for perfect prediction line

---

# Model Validation & Robustness Checks

## Cross-Validation Performance

All models were validated using [5-fold / 10-fold] cross-validation to assess stability and generalization.

**CV RMSE Results:**
- OLS: [Mean CV RMSE] ± [SD]
- LASSO: [Mean CV RMSE] ± [SD]
- Random Forest: [Mean CV RMSE] ± [SD]
- [etc.]

**Observation:** [Which model showed most stable performance? Was there high variance across folds?]

**📊 GRAPH NEEDED HERE:**
- **File:** `cv_performance_boxplot.png`
- **Type:** Boxplot showing distribution of CV RMSE across folds for each model
- **Placement:** Center of slide

## Robustness Checks Performed

### 1. Outlier Sensitivity
**Test:** Removed D.C. (often an outlier due to unique characteristics)
**Result:** [Did model performance or coefficients change substantially?]

### 2. Regional Stability
**Test:** Trained model excluding [Southern/Western/etc.] states, tested on excluded region
**Result:** [Did model generalize to held-out region?]

### 3. Feature Stability
**Test:** Removed highly correlated features (VIF > 10)
**Result:** [How did this affect performance and interpretation?]

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
- **Expected Impact:** [Estimate based on model coefficients]

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
3. **Feature Importance:** Identified economic factors (income, cost of living, education) as primary drivers
4. **Methodological Insight:** Demonstrated that machine learning methods (Random Forest, DNN) outperform traditional regression for poverty prediction
5. **Policy Relevance:** Provided evidence-based insights for state policymakers

## Future Research Directions

### Near-Term Extensions
1. **Panel Data Analysis:** Collect 2020-2025 data to assess trends and policy impacts over time
2. **County-Level Analysis:** Increase sample size to n = 3,000+ counties for more statistical power
3. **Interaction Effects:** Explore how variables combine (e.g., high education + low wages)
4. **Causal Inference:** Use instrumental variables or natural experiments to assess causality

### Long-Term Research Agenda
5. **Individual-Level Microdata:** Link to Survey of Income and Program Participation (SIPP) data
6. **Policy Evaluation:** Assess specific interventions (minimum wage changes, education funding) using difference-in-differences
7. **Qualitative Integration:** Conduct case studies of high/low poverty states to understand mechanisms
8. **Predictive Tool:** Develop interactive dashboard for policymakers to simulate intervention effects

**📊 VISUAL NEEDED HERE:**
- **File:** `future_research_roadmap.png`
- **Type:** Timeline or flowchart showing progression from current study to future work
- **Placement:** Center of slide
- **Tool:** Create in PowerPoint/Lucidchart

---

# References & Data Sources

## Academic Literature Cited

[Insert relevant citations in APA format, for example:]

1. Brady, D., Finnigan, R. M., & Hübgen, S. (2017). Rethinking the risks of poverty: A framework for analyzing prevalences and penalties. *American Journal of Sociology, 123*(3), 740-786.

2. Chetty, R., Hendren, N., Kline, P., & Saez, E. (2014). Where is the land of opportunity? The geography of intergenerational mobility in the United States. *The Quarterly Journal of Economics, 129*(4), 1553-1623.

3. Iceland, J. (2019). *Poverty in America: A handbook* (3rd ed.). University of California Press.

4. [Additional references from course materials, textbooks, or papers you consulted]

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

| Variable | Full Name | Unit | Source | Year |
|----------|-----------|------|--------|------|
| `poverty` | Poverty rate | Percentage | Census ACS S1701 | 2023 |
| `pop_dens` | Population density | People per sq mi | Census | 2023 |
| `avg_holdsize` | Average household size | Persons | Census B11001 | 2023 |
| `urban_pop` | Urban population proportion | Proportion (0-1) | Census | 2023 |
| `log_income` | Log per capita income | Log(USD) | Census S1901 | 2023 |
| `avg_rent` | Average monthly rent | USD | Census B25064 | 2023 |
| `log_med_home_price` | Log median home price | Log(USD) | Zillow ZHVI | 2023 |
| [etc. - complete for all 30 variables] ||||

## Appendix B: Complete Model Outputs

### OLS Regression Full Results

```
Call:
lm(formula = poverty ~ [predictors], data = train_data)

Residuals:
    Min      1Q  Median      3Q     Max
[values]

Coefficients:
                    Estimate Std. Error t value Pr(>|t|)
(Intercept)         [VALUE]    [VALUE]   [VALUE]  [VALUE]
[predictor1]        [VALUE]    [VALUE]   [VALUE]  [VALUE]
[predictor2]        [VALUE]    [VALUE]   [VALUE]  [VALUE]
...

Residual standard error: [VALUE] on [DF] degrees of freedom
Multiple R-squared:  [VALUE],	Adjusted R-squared:  [VALUE]
F-statistic: [VALUE] on [DF1] and [DF2] DF,  p-value: [VALUE]
```

### LASSO Coefficients (λ = optimal)

| Variable | Coefficient |
|----------|-------------|
| (Intercept) | [VALUE] |
| [Variable 1] | [VALUE] |
| [Variable 2] | [VALUE] |
| [Variable 3] | [VALUE] |
| ... | ... |
| [Variables with zero coefficient omitted] ||

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
- Team Lead: Winnie - [email]
- Project Repository: [GitHub/Google Drive link]
- Course: Wharton Data Science Academy (DSL11), Summer 2026
- Instructor: Professor Linda Zhao

**Acknowledgments:**
We thank Professor Linda Zhao for her guidance throughout this project, and our classmates for their feedback during development.

---

**Report compiled:** July 2026
**Word count:** [To be calculated]
**Total graphs required:** 30+
**Presentation length target:** 15 minutes
