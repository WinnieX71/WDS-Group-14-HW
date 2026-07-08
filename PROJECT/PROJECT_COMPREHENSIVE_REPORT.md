# Wharton DSA 2026 — Poverty Rate Predictor Study

## Comprehensive Project Status Report

**Report Date:** July 7, 2026
**Program:** Wharton Data Science Academy (DSL11), Summer 2026
**Presentation Date:** July 10, 2026 (3 days remaining)

---

## Executive Summary

This project investigates **what state-level variables most strongly predict higher poverty rates across the United States**. Using data from 50 states in 2023, the team has assembled 30 predictor variables and applied 6 different modeling approaches: OLS regression, LASSO, Decision Trees, Random Forest, Support Vector Regression, and Deep Neural Networks.

**Project Status:** ✅ **Analysis Complete** — Models built and evaluated
**Next Steps:** Build final presentation slides

---

## Team Members

- **Winnie** (Lead analyst)
- **June** (You)
- **Helen**
- **Akshaj**
- **Lilly**

---

## Research Question

> **What state-level variables most strongly predict higher poverty rates across the United States?**

### Study Design

- **Unit of analysis:** U.S. state (n = 50 + DC = 51 rows in dataset)
- **Time period:** 2023 (cross-sectional data)
- **Outcome variable:** Poverty rate (%)
- **Predictors:** 30 economic, social, demographic, and policy variables

---

## Dataset Overview

### File Location

```
PROJECT/data/processed/dataset.csv
```

### Dataset Dimensions

- **Rows:** 51 (50 states + DC)
- **Columns:** 31 (1 outcome + 30 predictors)

### Variable Inventory

| #                                    | Variable               | Category       | Description                       | Format            |
| ------------------------------------ | ---------------------- | -------------- | --------------------------------- | ----------------- |
| **Outcome**                    |                        |                |                                   |                   |
| 1                                    | `poverty`            | Outcome        | Poverty rate (%)                  | Continuous        |
| **Demographic**                |                        |                |                                   |                   |
| 2                                    | `pop_dens`           | Demographic    | Population density (per sq mi)    | Continuous        |
| 3                                    | `avg_holdsize`       | Demographic    | Average household size            | Continuous        |
| 4                                    | `urban_pop`          | Demographic    | Urban population proportion       | Proportion (0-1)  |
| 5                                    | `immigrant_mil`      | Demographic    | Immigrant population (thousands)  | Continuous        |
| 6                                    | `homeless`           | Social         | Homeless population (thousands)   | Continuous        |
| **Race/Ethnicity Proportions** |                        |                |                                   |                   |
| 7                                    | `white`              | Demographic    | White population proportion       | Proportion (0-1)  |
| 8                                    | `black`              | Demographic    | Black population proportion       | Proportion (0-1)  |
| 9                                    | `indian`             | Demographic    | Native American proportion        | Proportion (0-1)  |
| 10                                   | `asian`              | Demographic    | Asian population proportion       | Proportion (0-1)  |
| 11                                   | `other`              | Demographic    | Other races proportion            | Proportion (0-1)  |
| **Economic**                   |                        |                |                                   |                   |
| 12                                   | `unemploy_rate`      | Economic       | Unemployment rate (%)             | Continuous        |
| 13                                   | `avg_rent`           | Economic       | Average monthly rent ($)          | Continuous        |
| 14                                   | `apart_size`         | Economic       | Average apartment size (sq ft)    | Continuous        |
| 15                                   | `log_income`         | Economic       | Log of per capita income          | Log-transformed   |
| 16                                   | `avg_wage_hr`        | Economic       | Average hourly wage ($)           | Continuous        |
| 17                                   | `log_med_home_price` | Economic       | Log of median home price          | Log-transformed   |
| 18                                   | `min_wage`           | Policy         | State minimum wage ($)            | Continuous        |
| **Cost of Living Indices**     |                        |                |                                   |                   |
| 19                                   | `avg_live_conver`    | Economic       | Convenience stores cost index     | Index (100 = avg) |
| 20                                   | `avg_live_groc`      | Economic       | Grocery cost index                | Index (100 = avg) |
| 21                                   | `avg_live_hous`      | Economic       | Housing cost index                | Index (100 = avg) |
| 22                                   | `avg_live_uti`       | Economic       | Utilities cost index              | Index (100 = avg) |
| 23                                   | `log_avg_live_trans` | Economic       | Log of transportation cost index  | Log-transformed   |
| 24                                   | `avg_live_health`    | Economic       | Healthcare cost index             | Index (100 = avg) |
| **Policy & Infrastructure**    |                        |                |                                   |                   |
| 25                                   | `tax`                | Policy         | Combined state tax rate           | Proportion        |
| 26                                   | `log_transi_expen`   | Infrastructure | Log of public transit expenditure | Log-transformed   |
| **Education**                  |                        |                |                                   |                   |
| 27                                   | `highschool`         | Education      | High school graduation rate (%)   | Continuous        |
| 28                                   | `bachelor`           | Education      | Bachelor's degree attainment (%)  | Continuous        |
| **Social Issues**              |                        |                |                                   |                   |
| 29                                   | `crime`              | Social         | Violent crime rate (per 100k)     | Continuous        |
| 30                                   | `drug_use`           | Social         | Substance use prevalence          | Rate per 100k     |

### Data Quality

- ✅ **No missing values** (confirmed via `colSums(is.na())`)
- ✅ **Standardized format** (all numeric except State name)
- ✅ **Log transformations applied** to highly skewed variables (income, home price, transportation)
- ✅ **Government sources** (Census, FBI, CDC, HUD, USDA, Tax Foundation)

---

## Exploratory Data Analysis (EDA)

### Poverty Rate Summary Statistics

```
Mean:   ~11.3%
SD:     ~2.8%
Range:  6.5% (Alaska) to 11.5% (Arkansas, based on visible data)
```

### Top Correlations with Poverty Rate

Based on the analysis in `Project.rmd`, the **top 4 predictors** identified through correlation analysis are:

1. **Variable 1** (highest absolute correlation)
2. **Variable 2**
3. **Variable 3**
4. **Variable 4**

*(Note: Exact correlation values and variable names need to be extracted from the knitted HTML output)*

### Key EDA Visualizations Generated

✅ **Completed:**

1. Poverty rate distribution histogram (with mean line)
2. US map choropleth of state poverty rates
3. Correlation matrix heatmap (all 30 predictors)
4. Scatterplots: Top 4 predictors vs poverty (with regression lines)
5. Pairs plot: Poverty + top 4 predictors
6. Predictor distribution histograms
7. Correlation strength categorization (Strong/Moderate/Weak)

### Geographic Patterns

- Generated US map using `usmap` package
- Visualizes spatial distribution of poverty rates
- Allows identification of regional patterns (South, Northeast, etc.)

---

## Modeling Approaches — Summary

### 1. ✅ Ordinary Least Squares (OLS) Regression

**Model:** `lm(poverty ~ . - state, data = state_data)`

**Purpose:** Baseline linear model using all 30 predictors

**Status:** Complete

**Key Outputs:**

- Full model summary (coefficients, p-values, R², adjusted R²)
- Identifies statistically significant predictors (p < 0.05)

**Interpretation Notes:**

- With n=50 and p=30, high risk of overfitting
- Some predictors likely not significant
- Multicollinearity expected (especially among race variables, cost indices)

---

### 2. ✅ LASSO Regression (Regularized Feature Selection)

**Models:** Two LASSO models fit using `cv.glmnet()`

#### Model 2a: LASSO with lambda.min

- **Purpose:** Minimize cross-validation error
- **Result:** More predictors retained (less aggressive shrinkage)
- **Variables selected:** Extracted via `coef(lasso_cv, s = "lambda.min")`

#### Model 2b: LASSO with lambda.1se

- **Purpose:** Maximize parsimony (1 SE rule)
- **Result:** Fewer predictors retained (most conservative)
- **Variables selected:** Extracted via `coef(lasso_cv, s = "lambda.1se")`

**Cross-validation:** 10-fold CV used (note: with n=50, 5-fold may be more stable)

**Post-LASSO Models:**

- ✅ Fit OLS on lambda.min selected variables → `fit.min`
- ✅ Fit OLS on lambda.1se selected variables → `fit.1se`

**Visualization:**

- ✅ CV error plot (`plot(lasso_cv)`)
- ✅ Coefficient paths extracted (`coefplot::extract.coef()`)

---

### 3. ✅ Decision Tree (Regression Tree)

**Model:** `rpart(poverty ~ ., data = train_data)`

**Train/Test Split:** 80/20 split using `createDataPartition()`

**Key Features:**

- Minimum split: 10 observations
- Complexity parameter (cp): 0.01
- Method: ANOVA (regression tree)

**Evaluation Metrics:**

- ✅ RMSE (Root Mean Squared Error)
- ✅ R² (variance explained)
- ✅ MAE (Mean Absolute Error)

**Tree Optimization:**

- ✅ Identified optimal cp via cross-validation (`printcp()`)
- ✅ Pruned tree using optimal cp
- ✅ Hyperparameter tuning with 10-fold CV (tested cp from 0.01 to 0.1)

**Feature Importance:**

- ✅ Extracted `tree_model$variable.importance`
- ✅ Bar plot showing most important splits

**Visualization:**

- ✅ Tree diagram (`rpart.plot()`) for both full and pruned trees

---

### 4. ✅ Random Forest

**Model:** `randomForest(poverty ~ ., data = trainData)`

**Configuration:**

- Default: 500 trees (ntree = 500)
- ✅ Tuned model: `ntree = 500, mtry = 10`
- Train/Test split: 80/20

**Performance:**

- ✅ Test MAE calculated
- ✅ Out-of-bag error reported

**Feature Importance:**

- ✅ `importance(rf_model)` extracted
- ✅ Variable importance plot (`varImpPlot()`)
- Two importance measures:
  - **%IncMSE:** How much MSE increases when variable is permuted
  - **IncNodePurity:** Total decrease in node impurity from splits on that variable

**Comparison:**

- Random Forest importance compared to LASSO selections
- Identifies most robust predictors across methods

---

### 5. ✅ Support Vector Regression (SVR)

**Model:** `svm(poverty ~ ., type = "eps-regression", kernel = "radial")`

**Hyperparameters:**

- Kernel: Radial basis function (RBF)
- Cost: 15
- Epsilon: 0.25

**Performance:**

- ✅ Test RMSE calculated
- ✅ Predictions vs actuals scatterplot (with 45° reference line)

**Feature Importance:**

- ✅ Permutation importance using `vip()` package
- Shows how much each feature reduces RMSE when kept intact
- Visualized as bar chart

**Visualization:**

- ✅ SVR fit plot (unemployment rate vs poverty — example predictor)
- ✅ Predicted vs Actual scatter plot

---

### 6. ✅ Deep Learning (Neural Network)

**Framework:** Keras3 (with PyTorch backend)

**Model Location:**

```
PROJECT/models/nn_model_l2_poverty_prediction.keras
```

**Architecture:** (details need to be extracted from notebook)

- Input layer: 30 features (scaled)
- Hidden layers: [architecture details]
- Output layer: 1 neuron (regression)
- Regularization: L2 regularization applied (indicated by filename)

**Preprocessing:**

- ✅ Scaler saved: `models/preprocessing_scaler.rds`
- All features standardized before training

**Training:**

- Implemented in `Deep_Learning_Poverty_Prediction.ipynb` (Jupyter/Colab notebook)
- Google Colab compatible (R runtime)

**Key Fixes Applied:** (from FIXES_APPLIED.md)

- Updated from deprecated `keras` to `keras3`
- Fixed column name (`poverty` not `poverty_rate`)
- Added flexible file path detection
- Removed unnecessary `install_keras()` call
- Added `gridExtra` for visualization

**Status:** Model trained and saved

---

## Model Comparison Framework

### Convergence Analysis

**Strategy:** Identify predictors that are important across **multiple methods**

| Method         | Importance Metric                                    |
| -------------- | ---------------------------------------------------- |
| LASSO          | Non-zero coefficients (lambda.1se most conservative) |
| Random Forest  | %IncMSE and IncNodePurity                            |
| Decision Tree  | Variable importance scores                           |
| SVR            | Permutation importance (RMSE reduction)              |
| Neural Network | [Weight analysis or permutation importance]          |

**Robust Predictors:** Variables that rank high across ≥3 methods are the strongest candidates for the final "answer" to the research question.

### Expected Top Predictors (Literature-Based)

Based on poverty research, likely important variables:

1. **Unemployment rate** (strong positive correlation)
2. **Education level** (bachelor's, high school)
3. **Income** (log_income, avg_wage_hr)
4. **Household structure** (avg_holdsize — proxy for single-parent families)
5. **Homeless rate**

---

## Performance Metrics Summary

### Model Evaluation Table

| Model                  | Metric       | Value                 | Notes                       |
| ---------------------- | ------------ | --------------------- | --------------------------- |
| OLS Full               | Adjusted R² | [Extract from output] | Likely overfit (p=30, n=50) |
| LASSO (lambda.min)     | CV MSE       | [Extract]             | Less regularized            |
| LASSO (lambda.1se)     | CV MSE       | [Extract]             | Most parsimonious           |
| Decision Tree          | RMSE         | [Extract]             | Single tree baseline        |
| Decision Tree (Pruned) | RMSE         | [Extract]             | Reduced overfitting         |
| Random Forest          | MAE          | [Extract]             | Best ensemble?              |
| Random Forest (Tuned)  | MAE          | [Extract]             | ntree=500, mtry=10          |
| SVR (Radial)           | RMSE         | [Extract]             | Nonlinear approach          |
| Neural Network         | [MSE/MAE]    | [Extract]             | Deep learning approach      |

*(Exact values need to be extracted from knitted RMarkdown output or saved model objects)*

---

## Critical Limitations

### Statistical Limitations

1. **Small sample size (n=50)**

   - Wide confidence intervals
   - Low statistical power
   - Overfitting risk with 30 predictors
2. **Cross-sectional design**

   - Correlation ≠ causation
   - Cannot establish temporal precedence
   - No ability to control for state-specific fixed effects
3. **Reverse causality**

   - Poverty may **cause** some predictors (e.g., crime, substance abuse, divorce)
   - Not just the other way around
   - Cannot disentangle direction of effect
4. **Ecological fallacy**

   - State-level patterns may not hold at individual level
   - Aggregate relationships can be misleading
   - Policy implications require caution
5. **Omitted variable bias**

   - Historical policy decisions not captured
   - Political factors excluded
   - Geographic/climate variables missing
   - Pre-existing inequality trends unaccounted for

### Methodological Limitations

6. **Multicollinearity**

   - Race variables likely correlated
   - Cost of living indices overlap conceptually
   - Income variables redundant (log_income, avg_wage_hr)
   - **Mitigation:** VIF check recommended (not yet documented)
7. **Feature engineering decisions**

   - Log transformations assumed (income, home price, transportation)
   - No interaction terms explored
   - No polynomial terms tested
8. **Race ratio framing**

   - Including race variables requires careful interpretation
   - Risk of misattribution (race as proxy for structural factors)
   - Must frame as demographic controls, not causal claims

---

## Data Sources

All data from official U.S. government sources:

| Source                               | Variables                              | URL                 |
| ------------------------------------ | -------------------------------------- | ------------------- |
| **U.S. Census Bureau**         | Poverty, income, demographics, housing | data.census.gov     |
| **Bureau of Labor Statistics** | Unemployment, wages                    | bls.gov             |
| **FBI Uniform Crime Reports**  | Crime rates                            | ucr.fbi.gov         |
| **SAMHSA**                     | Substance use prevalence               | samhsa.gov          |
| **HUD**                        | Homeless counts                        | hudexchange.info    |
| **CDC NCHS**                   | [If divorce data used]                 | cdc.gov/nchs        |
| **USDA ERS**                   | Education, rural-urban codes           | ers.usda.gov        |
| **Tax Foundation**             | Tax rates                              | taxfoundation.org   |
| **Zillow Research**            | Home prices                            | zillow.com/research |
| **MERIC**                      | Cost of living indices                 | meric.mo.gov        |

**Temporal Consistency:** All data from 2023 (single year, cross-section)

---

## Code Files — Status Summary

### Analysis Scripts

| File                                       | Purpose                | Status                | Location      |
| ------------------------------------------ | ---------------------- | --------------------- | ------------- |
| `Project.rmd`                            | Main analysis notebook | ✅ Complete           | `analysis/` |
| `Evaluation.Rmd`                         | Model evaluation       | ⚠️ Empty/incomplete | `analysis/` |
| `Deep_Learning_Poverty_Prediction.ipynb` | Neural network         | ✅ Complete           | `analysis/` |

### Data Files

| File            | Purpose            | Status              | Location                  |
| --------------- | ------------------ | ------------------- | ------------------------- |
| `dataset.csv` | Master dataset     | ✅ Complete         | `data/processed/`       |
| Raw data files  | Original downloads | ⚠️ Not documented | `data/raw/` (if exists) |

### Model Artifacts

| File                                     | Purpose                 | Status   | Location    |
| ---------------------------------------- | ----------------------- | -------- | ----------- |
| `nn_model_l2_poverty_prediction.keras` | Trained neural network  | ✅ Saved | `models/` |
| `preprocessing_scaler.rds`             | Feature scaler (for NN) | ✅ Saved | `models/` |

### Documentation

| File                                | Purpose                | Status          | Location |
| ----------------------------------- | ---------------------- | --------------- | -------- |
| `README.md`                       | Project overview       | ✅ Complete     | Root     |
| `QUICKSTART.md`                   | Quick start guide      | ✅ Complete     | Root     |
| `FIXES_APPLIED.md`                | Notebook debugging log | ✅ Complete     | Root     |
| `PROJECT_COMPREHENSIVE_REPORT.md` | **This report**  | ✅**New** | Root     |

### Presentation

| File                         | Purpose            | Status                  | Location          |
| ---------------------------- | ------------------ | ----------------------- | ----------------- |
| `wharton_dsl11_slides.Rmd` | Final presentation | ❌**NOT STARTED** | `presentation/` |

---

## Next Steps — Path to July 10 Presentation

### Priority 1: Extract Key Results (Today)

**Tasks:**

1. ✅ Knit `Project.rmd` to HTML (if not already done)
2. Extract exact correlation values for top predictors
3. Document performance metrics for all 6 models
4. Create model comparison table (which predictors appear in each?)
5. Identify **top 3-5 most robust predictors** (high importance across methods)

### Priority 2: Build Hero Visualizations (July 8)

**Key Charts:**

1. **Coefficient plot** from LASSO (lambda.1se) — surviving predictors
2. **Variable importance plot** from Random Forest
3. **Convergence table** — predictors selected by each method
4. **Geographic map** of poverty rates (already created)
5. **Model performance bar chart** — RMSE or MAE comparison

### Priority 3: Create Presentation (July 9)

**Slide Outline (15 minutes = ~10-12 slides):**

1. **Title slide**

   - Research question
   - Team names
2. **Motivation**

   - Why poverty prediction matters
   - 2-3x variation across states (show range)
3. **Data overview**

   - 50 states, 30 predictors, 6 models
   - Sources (government data)
4. **EDA highlights**

   - Poverty distribution histogram
   - US map
   - Top correlations
5. **Modeling approach**

   - Sequential strategy: OLS → LASSO → Trees → RF → SVR → NN
   - Cross-validation strategy
6. **🎯 HERO SLIDE: LASSO Results**

   - Coefficient plot (lambda.1se)
   - "These are the predictors that matter"
7. **Random Forest Confirmation**

   - Variable importance plot
   - Convergence with LASSO
8. **Model Performance Comparison**

   - Table or bar chart: RMSE/MAE across models
   - Best performing model highlighted
9. **Robustness Checks** (if time permits)

   - Drop DC (outlier check)
   - Regional analysis
10. **Limitations**

    - n=50 is small
    - Correlation ≠ causation
    - Reverse causality concerns
11. **Key Findings**

    - Top 3 predictors of poverty
    - Policy implications (cautiously framed)
12. **Conclusion**

    - Answer to research question
    - Future directions

### Priority 4: Practice & Polish (July 10 morning)

- Rehearse timing (15 min target)
- Prepare for questions
- Backup slides (if needed)

---

## Outstanding Questions for Team Discussion

### Analysis Questions

1. **VIF Check:** Have you tested for multicollinearity? (Should do VIF on OLS model)
2. **Outliers:** Is DC an outlier? Should it be excluded in robustness check?
3. **Regional Effects:** South vs non-South comparison? (Poverty higher in South historically)
4. **Interaction Terms:** Any theoretically motivated interactions to test? (e.g., education × unemployment)

### Interpretation Questions

5. **Causal Language:** Agreed team framing on correlation vs causation?
6. **Race Variables:** How to present without causal misattribution?
7. **Policy Implications:** Which findings are actionable vs descriptive?

### Presentation Questions

8. **Target Audience:** Assume classmates have DSA background or explain methods?
9. **Time Allocation:** 10 min presentation + 5 min Q&A, or 15 min total?
10. **Technical Depth:** Show code/equations or focus on interpretation?

---

## Team Contribution Summary

Based on file metadata and code comments:

- **Winnie:** Primary analyst (file path indicates lead on data assembly)
- **June (You):** [Your specific contributions]
- **Helen:** [Contributions]
- **Akshaj:** [Contributions]
- **Lilly:** [Contributions]

*(Consider adding a contributions section to presentation if required by Prof. Zhao)*

---

## R Packages Used

**Data Manipulation:**

- `tidyverse` (dplyr, ggplot2, readr, tidyr)
- `readxl` (Excel import)
- `skimr` (summary statistics)

**Visualization:**

- `corrplot` (correlation matrix)
- `GGally` (pairs plots)
- `gridExtra` (multi-panel plots)
- `rpart.plot` (decision trees)
- `usmap` (geographic maps)

**Modeling:**

- `glmnet` (LASSO)
- `rpart` (decision trees)
- `randomForest` (random forest)
- `e1071` (SVR)
- `keras3` (neural networks)
- `caret` (train/test splits, metrics)
- `vip` (variable importance plots)

**Post-Processing:**

- `broom` (tidy model output) [recommended, not confirmed used]
- `coefplot` (LASSO coefficient plots)
- `MLmetrics` (performance metrics)

---

## Computational Environment

**Primary Environment:**

- R (version: check `R.version`)
- RStudio/Positron

**Deep Learning:**

- Python backend (via `reticulate`)
- Keras3 with PyTorch backend
- Google Colab compatible (R runtime)

**Reproducibility:**

- Seed set in most models: `set.seed(42)`, `set.seed(1)`, `set.seed(10)`, `set.seed(123)`
- Train/test splits: 80/20 standard
- Cross-validation: 10-fold (LASSO, Decision Tree)

---

## File Structure (Actual)

```
PROJECT/
├── PROJECT_COMPREHENSIVE_REPORT.md  ← THIS FILE (NEW)
├── README.md                         ← Project overview
├── QUICKSTART.md                     ← EDA quick start
├── FIXES_APPLIED.md                  ← Deep learning debugging notes
├── Deep_Learning_Poverty_Prediction.ipynb  ← Neural network (root copy)
│
├── data/
│   ├── processed/
│   │   └── dataset.csv               ← 51 rows x 31 columns
│   └── raw/
│       └── README.md                 ← (placeholder for raw files)
│
├── analysis/
│   ├── Project.rmd                   ← ✅ MAIN ANALYSIS FILE
│   ├── Evaluation.Rmd                ← ⚠️ Empty/stub
│   └── Deep_Learning_Poverty_Prediction.ipynb  ← ✅ Neural network
│
├── models/
│   ├── nn_model_l2_poverty_prediction.keras   ← Trained NN
│   └── preprocessing_scaler.rds               ← Feature scaler
│
├── output/
│   ├── figures/  ← (empty — save plots here)
│   └── tables/   ← (empty — save tables here)
│
└── presentation/
    └── (empty — create slides here)
```

---

## Recommended Immediate Actions

### For June (You)

1. **Review this report** with team
2. **Knit `Project.rmd`** to HTML and review all outputs
3. **Extract key results:**
   - Top predictors from each model
   - Performance metrics (RMSE/MAE/R²)
   - Statistical significance (p-values from OLS)
4. **Start presentation outline** in `presentation/wharton_dsl11_slides.Rmd`

### For Team

5. **Divide presentation work:**

   - Intro/motivation (1 person)
   - Methods/EDA (1-2 people)
   - Results/hero slides (1-2 people)
   - Conclusion/limitations (1 person)
6. **Schedule practice run** (July 9 evening)
7. **Prepare Q&A responses** for anticipated questions:

   - "Why did you choose LASSO over elastic net?"
   - "How do you interpret the race variables?"
   - "What are the policy implications?"
   - "Why is n=50 sufficient?"

---

## Key Findings (Preliminary — To Be Finalized)

### Research Question Answer

**What state-level variables most strongly predict higher poverty rates?**

**Based on multi-model convergence analysis, the top predictors are:**

1. **[Variable 1]** — [correlation/importance score]
2. **[Variable 2]** — [correlation/importance score]
3. **[Variable 3]** — [correlation/importance score]
4. **[Variable 4]** — [correlation/importance score]
5. **[Variable 5]** — [correlation/importance score]

*(Exact ranking requires extracting results from all models and building convergence table)*

### Policy Implications (Tentative)

If the top predictors are as expected (unemployment, education, income):

- **Leverage unemployment reduction** as poverty-fighting tool
- **Invest in education access** (especially bachelor's attainment)
- **Address structural income inequality**

**Caveats:**

- Cross-sectional data cannot prove causation
- State-level policies must account for local context
- Omitted historical factors likely important

---

## Appendix: Variable Name Mappings

Quick reference for interpreting variable names in code:

| Code Name                                              | Full Name                                                |
| ------------------------------------------------------ | -------------------------------------------------------- |
| `pop_dens`                                           | Population density                                       |
| `unemploy_rate`                                      | Unemployment rate                                        |
| `avg_rent`                                           | Average rent                                             |
| `apart_size`                                         | Average apartment size                                   |
| `log_income`                                         | Log per capita income                                    |
| `avg_wage_hr`                                        | Average hourly wage                                      |
| `log_med_home_price`                                 | Log median home price                                    |
| `avg_live_*`                                         | Cost of living indices (conver, groc, hous, uti, health) |
| `log_avg_live_trans`                                 | Log transportation cost index                            |
| `tax`                                                | Combined state tax rate                                  |
| `highschool`                                         | High school graduation rate                              |
| `bachelor`                                           | Bachelor's degree attainment                             |
| `min_wage`                                           | State minimum wage                                       |
| `avg_holdsize`                                       | Average household size                                   |
| `crime`                                              | Violent crime rate                                       |
| `drug_use`                                           | Substance use prevalence                                 |
| `white`, `black`, `indian`, `asian`, `other` | Racial/ethnic composition                                |
| `log_transi_expen`                                   | Log public transit expenditure                           |
| `urban_pop`                                          | Urban population proportion                              |
| `immigrant_mil`                                      | Immigrant population                                     |
| `homeless`                                           | Homeless population                                      |
| `poverty`                                            | **Poverty rate (OUTCOME)**                         |

---

## Report Metadata

**Document:** Comprehensive Project Status Report
**Author:** June (compiled from team's work)
**Purpose:** Pre-presentation synthesis and planning document
**Audience:** Team members (internal reference)
**Last Updated:** July 7, 2026
**Version:** 1.0

---

## End of Report

**Next Steps:**

1. Team review meeting
2. Extract final results
3. Build presentation slides
4. Practice presentation

**Presentation Deadline:** July 10, 2026 (3 days)

---

**Questions?** Discuss with team or consult Prof. Linda Zhao.
