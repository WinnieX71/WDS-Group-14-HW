# Wharton DSA 2026 — Poverty Rate Predictor Study

**Program:** Wharton Data Science Academy (DSL11), Summer 2026
**Instructor:** Professor Linda Zhao
**Deliverable:** 15-minute group presentation, July 10, 2026
**Team Size:** 5 members
**Primary Language:** R (Positron + RMarkdown)

---

## Research Question

> **What state-level variables most strongly predict higher poverty rates across the United States?**

- **Unit of analysis:** U.S. state (n = 50)
- **Time period:** 2023 (cross-sectional data)
- **Goal:** Identify which economic, social, and policy factors are most associated with state poverty rates

---

## Project Structure

```Shell
iPROJECT/
├── data/
│   ├── raw/                    # Downloaded files (unchanged)
│   └── processed/              # Cleaned and joined data
├── analysis/
│   ├── 01_data_assembly.R      # Load, clean, join all data
│   ├── 02_eda.Rmd              # Exploratory Data Analysis
│   ├── 03_regression.R         # OLS models
│   ├── 04_lasso.R              # LASSO feature selection
│   └── 05_random_forest.R      # Random Forest importance
├── output/
│   ├── figures/                # All plots and charts
│   └── tables/                 # Regression tables, summaries
└── presentation/
    └── wharton_dsl11_slides.Rmd  # Final presentation
```

---

## Workflow

### Step 1: Download Raw Data

Place all downloaded files in `data/raw/`. See the main project context document for URLs.

**Required files (17 predictors + 1 outcome):**

- Census ACS Tables: S1701, S1901, B25064, B11001, B02001, B08301
- Zillow: ZHVI All Homes by State
- MERIC: Cost of Living Index
- Tax Foundation: Sales, Income, Property Tax rates
- USDA ERS: Education levels, Rural-Urban Continuum Codes
- FBI UCR: Estimated crimes 2023
- SAMHSA: State substance use prevalence
- CDC: Divorce rates by state
- HUD: Point-in-Time homeless count

### Step 2: Run Data Assembly

```r
source("analysis/01_data_assembly.R")
```

This will:

- Load all 18 raw data files
- Standardize state names
- Join into one master dataset
- Save to `data/processed/state_data_2023.csv`

### Step 3: Run EDA

Open and knit `analysis/02_eda.Rmd` in RStudio/Positron.

This will generate:

- Distribution plots for all variables
- Correlation matrix
- Geographic map of poverty rates
- Scatterplots of top predictors
- VIF multicollinearity check
- Outlier detection
- Log transformation analysis

### Step 4: Modeling (to be completed)

Run in sequence:

1. `03_regression.R` - OLS baseline models
2. `04_lasso.R` - LASSO feature selection
3. `05_random_forest.R` - Random Forest importance

### Step 5: Build Presentation

Create final slides in `presentation/wharton_dsl11_slides.Rmd`

---

## Required R Packages

```r
install.packages(c(
  "tidyverse",    # dplyr, ggplot2, readr, tidyr
  "readxl",       # read Excel files
  "janitor",      # clean column names
  "skimr",        # quick summaries
  "corrplot",     # correlation matrix
  "GGally",       # pairs plots
  "plotly",       # interactive charts
  "maps",         # geographic maps
  "viridis",      # color scales
  "broom",        # tidy model output
  "car",          # VIF check
  "glmnet",       # LASSO
  "randomForest", # Random Forest
  "knitr",        # kable tables
  "kableExtra"    # enhanced tables
))
```

---

## Key Variables

| Category                 | Variables                                                                 |
| ------------------------ | ------------------------------------------------------------------------- |
| **Outcome**        | Poverty rate                                                              |
| **Economic**       | Avg Rent, Housing Price, Individual Income, Family Income, Living Expense |
| **Tax Policy**     | Sales Tax, Income Tax, Property Tax                                       |
| **Social**         | Education Level, Household Structure, Crime Rate, Substance Abuse         |
| **Demographic**    | Race Ratio, Urbanization Rate                                             |
| **Infrastructure** | Public Transportation                                                     |
| **Instability**    | Divorce Rate, Homeless Rate                                               |

---

## Analysis Checklist

- [ ] Download all 18 data files to `data/raw/`
- [ ] Run `01_data_assembly.R` successfully
- [ ] Knit `02_eda.Rmd` and review output
- [ ] Check for missing values and outliers
- [ ] Identify high VIF variables (multicollinearity)
- [ ] Build OLS Model 1 (baseline)
- [ ] Build OLS Model 2 (with race ratio)
- [ ] Run LASSO with 5-fold CV
- [ ] Run Random Forest (ntree=500)
- [ ] Compare importance across models
- [ ] Run robustness checks (drop DC, drop South, etc.)
- [ ] Create hero chart (LASSO coefficient plot)
- [ ] Build final presentation

---

## Critical Limitations

1. **n=50 is small** - wide confidence intervals, low power
2. **Correlation ≠ causation** - cross-sectional observational study
3. **Reverse causality** - poverty may cause some predictors (crime, divorce)
4. **Ecological fallacy** - state-level patterns may not hold at individual level
5. **Omitted variables** - historical policies, geography not captured

---

## Contact

For questions about this project, contact your group members or Prof. Zhao.

---

**Last Updated:** July 1, 2026
