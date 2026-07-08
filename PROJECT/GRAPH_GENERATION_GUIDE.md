# Graph Generation Guide for Final Presentation
## Complete Checklist of All Required Visualizations

**Project:** Poverty Rate Prediction Study
**Presentation Date:** July 10, 2026
**Total Graphs Needed:** 30+

---

## Section 1: Executive Summary (1 graph)

### Graph 1: Model Performance Comparison
- **Filename:** `presentation_overview_chart.png`
- **Type:** Bar chart
- **Purpose:** Compare RMSE or R² across all 6 models (OLS, LASSO, Decision Tree, Random Forest, SVR, DNN)
- **Placement:** Right side of executive summary slide
- **R Code:**
```r
library(ggplot2)
library(dplyr)

# Create data frame with model results
model_results <- data.frame(
  Model = c("OLS", "LASSO", "Decision Tree", "Random Forest", "SVR", "Deep Neural Net"),
  RMSE = c(...),  # Insert your actual values
  R_squared = c(...)  # Insert your actual values
)

# Bar chart for RMSE
ggplot(model_results, aes(x = reorder(Model, RMSE), y = RMSE, fill = Model)) +
  geom_col() +
  coord_flip() +
  labs(title = "Model Performance Comparison",
       subtitle = "Lower RMSE = Better Performance",
       x = "Model", y = "Root Mean Squared Error (RMSE)") +
  theme_minimal() +
  theme(legend.position = "none")

ggsave("output/figures/presentation_overview_chart.png", width = 8, height = 5, dpi = 300)
```

---

## Section 2: Research Question (1 graph)

### Graph 2: U.S. Poverty Map
- **Filename:** `us_poverty_map.png`
- **Type:** Choropleth map
- **Purpose:** Show geographic distribution of poverty rates by state
- **Placement:** Bottom of slide
- **R Code:**
```r
library(maps)
library(viridis)
library(ggplot2)

# Load state data
states_map <- map_data("state")

# Merge with your poverty data
# (Assuming you have a data frame called 'state_data' with columns: state, poverty)
state_data$region <- tolower(state_data$state)

map_data <- states_map %>%
  left_join(state_data, by = "region")

# Create choropleth
ggplot(map_data, aes(x = long, y = lat, group = group, fill = poverty)) +
  geom_polygon(color = "white", size = 0.2) +
  scale_fill_viridis(option = "plasma", name = "Poverty Rate (%)") +
  coord_map("albers", lat0 = 39, lat1 = 45) +
  labs(title = "State Poverty Rates Across the United States (2023)") +
  theme_void() +
  theme(legend.position = "right")

ggsave("output/figures/us_poverty_map.png", width = 10, height = 6, dpi = 300)
```

---

## Section 3: Data Sources (1 graph)

### Graph 3: Variable Categories Pie Chart
- **Filename:** `variable_categories_pie.png`
- **Type:** Pie/donut chart
- **Purpose:** Show distribution of 30 variables by category
- **Placement:** Right side of slide
- **R Code:**
```r
# Create category counts
categories <- data.frame(
  Category = c("Demographic", "Economic", "Policy", "Education", "Social", "Infrastructure"),
  Count = c(11, 11, 3, 2, 2, 1)
)

ggplot(categories, aes(x = "", y = Count, fill = Category)) +
  geom_bar(stat = "identity", width = 1) +
  coord_polar("y", start = 0) +
  labs(title = "Distribution of Predictor Variables by Category",
       subtitle = "Total: 30 predictors") +
  theme_void() +
  scale_fill_brewer(palette = "Set3")

ggsave("output/figures/variable_categories_pie.png", width = 6, height = 6, dpi = 300)
```

---

## Section 4: Dataset Overview (1 graph/table)

### Graph 4: Summary Statistics Table
- **Filename:** `summary_statistics_table.png`
- **Type:** Formatted table
- **Purpose:** Show descriptive statistics for key variables
- **Placement:** Center of slide
- **R Code:**
```r
library(knitr)
library(kableExtra)

# Create summary statistics
summary_stats <- data %>%
  select(poverty, log_income, unemploy_rate, crime) %>%
  summarise(across(everything(),
                   list(Mean = mean,
                        Median = median,
                        SD = sd,
                        Min = min,
                        Max = max))) %>%
  pivot_longer(everything(),
               names_to = c("Variable", "Statistic"),
               names_sep = "_") %>%
  pivot_wider(names_from = Statistic, values_from = value)

# Create formatted table
kable(summary_stats, format = "html", digits = 2,
      caption = "Summary Statistics for Key Variables") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed")) %>%
  save_kable("output/figures/summary_statistics_table.png")
```

---

## Section 5: EDA - Distribution Analysis (3 graphs)

### Graph 5: Poverty Distribution Histogram
- **Filename:** `poverty_distribution_histogram.png`
- **Type:** Histogram with normal curve overlay
- **Purpose:** Show distribution of outcome variable
- **Placement:** Left side of slide
- **R Code:**
```r
ggplot(data, aes(x = poverty)) +
  geom_histogram(aes(y = ..density..), bins = 15, fill = "steelblue", alpha = 0.7) +
  stat_function(fun = dnorm,
                args = list(mean = mean(data$poverty), sd = sd(data$poverty)),
                color = "red", size = 1) +
  labs(title = "Distribution of State Poverty Rates",
       x = "Poverty Rate (%)", y = "Density") +
  theme_minimal()

ggsave("output/figures/poverty_distribution_histogram.png", width = 6, height = 4, dpi = 300)
```

### Graph 6: Poverty Boxplot
- **Filename:** `poverty_boxplot.png`
- **Type:** Boxplot
- **Purpose:** Show quartiles and outliers
- **Placement:** Right side of slide
- **R Code:**
```r
ggplot(data, aes(y = poverty, x = "")) +
  geom_boxplot(fill = "lightblue", outlier.color = "red", outlier.size = 3) +
  coord_flip() +
  labs(title = "Poverty Rate Distribution",
       subtitle = "Boxplot showing quartiles and outliers",
       y = "Poverty Rate (%)", x = "") +
  theme_minimal() +
  theme(axis.text.y = element_blank())

ggsave("output/figures/poverty_boxplot.png", width = 6, height = 3, dpi = 300)
```

### Graph 7: Predictor Distributions Grid
- **Filename:** `predictor_distributions_grid.png`
- **Type:** Faceted histograms (small multiples)
- **Purpose:** Show distribution of all 30 predictors
- **Placement:** Full slide (may need 2 slides)
- **R Code:**
```r
# Reshape data to long format
data_long <- data %>%
  select(-state) %>%  # Remove state names
  pivot_longer(everything(), names_to = "variable", values_to = "value")

# Create faceted histograms
ggplot(data_long, aes(x = value)) +
  geom_histogram(bins = 10, fill = "steelblue", alpha = 0.7) +
  facet_wrap(~ variable, scales = "free", ncol = 5) +
  labs(title = "Distribution of All Predictor Variables",
       x = "Value", y = "Count") +
  theme_minimal() +
  theme(strip.text = element_text(size = 8))

ggsave("output/figures/predictor_distributions_grid.png", width = 15, height = 10, dpi = 300)
```

---

## Section 6: EDA - Correlation Analysis (3 graphs)

### Graph 8: Correlation Matrix Heatmap
- **Filename:** `correlation_matrix_heatmap.png`
- **Type:** Correlation heatmap with hierarchical clustering
- **Purpose:** Show relationships between all variables
- **Placement:** Full slide
- **R Code:**
```r
library(corrplot)

# Calculate correlation matrix
cor_matrix <- cor(data %>% select(-state), use = "complete.obs")

# Create hierarchical clustered heatmap
png("output/figures/correlation_matrix_heatmap.png", width = 12, height = 12, units = "in", res = 300)
corrplot(cor_matrix, method = "color", type = "upper",
         order = "hclust",
         addCoef.col = "black",
         number.cex = 0.5,
         tl.col = "black", tl.srt = 45,
         col = colorRampPalette(c("blue", "white", "red"))(200),
         title = "Correlation Matrix of All Variables")
dev.off()
```

### Graph 9: Top Predictors Scatterplot Matrix
- **Filename:** `top_predictors_scatter.png`
- **Type:** Pairs plot (scatterplot matrix)
- **Purpose:** Detailed view of relationships between top 5 predictors and poverty
- **Placement:** Full slide
- **R Code:**
```r
library(GGally)

# Select top 5 predictors + poverty
top_vars <- data %>% select(poverty, log_income, bachelor, crime, avg_rent, unemploy_rate)

# Create pairs plot
ggpairs(top_vars,
        title = "Relationships Between Top Predictors and Poverty Rate",
        lower = list(continuous = wrap("points", alpha = 0.5)),
        diag = list(continuous = wrap("densityDiag", alpha = 0.5))) +
  theme_minimal()

ggsave("output/figures/top_predictors_scatter.png", width = 10, height = 10, dpi = 300)
```

### Graph 10: VIF Barplot
- **Filename:** `vif_barplot.png`
- **Type:** Horizontal bar chart
- **Purpose:** Show multicollinearity issues
- **Placement:** Bottom half of slide
- **R Code:**
```r
library(car)

# Fit model and calculate VIF
model <- lm(poverty ~ ., data = data %>% select(-state))
vif_values <- vif(model)

# Create data frame
vif_df <- data.frame(
  Variable = names(vif_values),
  VIF = vif_values
) %>%
  mutate(Severity = case_when(
    VIF < 5 ~ "Low (<5)",
    VIF >= 5 & VIF < 10 ~ "Moderate (5-10)",
    VIF >= 10 ~ "High (>10)"
  ))

# Plot
ggplot(vif_df, aes(x = reorder(Variable, VIF), y = VIF, fill = Severity)) +
  geom_col() +
  coord_flip() +
  geom_hline(yintercept = 5, linetype = "dashed", color = "orange") +
  geom_hline(yintercept = 10, linetype = "dashed", color = "red") +
  scale_fill_manual(values = c("Low (<5)" = "green3",
                               "Moderate (5-10)" = "orange",
                               "High (>10)" = "red")) +
  labs(title = "Variance Inflation Factors (VIF)",
       subtitle = "Higher VIF indicates multicollinearity",
       x = "Variable", y = "VIF Value") +
  theme_minimal()

ggsave("output/figures/vif_barplot.png", width = 8, height = 10, dpi = 300)
```

---

## Section 7: EDA - Geographic Patterns (2 graphs)

### Graph 11: Poverty by Region Boxplot
- **Filename:** `poverty_by_region_boxplot.png`
- **Type:** Side-by-side boxplots
- **Purpose:** Compare poverty across U.S. regions
- **Placement:** Center of slide
- **R Code:**
```r
# First, add region classification to your data
data <- data %>%
  mutate(region = case_when(
    state %in% c("Connecticut", "Maine", "Massachusetts", "New Hampshire",
                 "Rhode Island", "Vermont", "New Jersey", "New York", "Pennsylvania") ~ "Northeast",
    state %in% c("Illinois", "Indiana", "Michigan", "Ohio", "Wisconsin", "Iowa",
                 "Kansas", "Minnesota", "Missouri", "Nebraska", "North Dakota", "South Dakota") ~ "Midwest",
    state %in% c("Delaware", "Florida", "Georgia", "Maryland", "North Carolina",
                 "South Carolina", "Virginia", "District of Columbia", "West Virginia",
                 "Alabama", "Kentucky", "Mississippi", "Tennessee", "Arkansas",
                 "Louisiana", "Oklahoma", "Texas") ~ "South",
    TRUE ~ "West"
  ))

# Create boxplot
ggplot(data, aes(x = region, y = poverty, fill = region)) +
  geom_boxplot() +
  geom_jitter(width = 0.2, alpha = 0.3) +
  labs(title = "Poverty Rates by U.S. Region",
       x = "Region", y = "Poverty Rate (%)") +
  theme_minimal() +
  theme(legend.position = "none") +
  scale_fill_brewer(palette = "Set2")

ggsave("output/figures/poverty_by_region_boxplot.png", width = 8, height = 5, dpi = 300)
```

### Graph 12: Urban vs. Poverty Scatterplot
- **Filename:** `urban_vs_poverty_scatter.png`
- **Type:** Scatterplot with state labels
- **Purpose:** Examine urbanization-poverty relationship
- **Placement:** Bottom of slide
- **R Code:**
```r
library(ggrepel)

ggplot(data, aes(x = urban_pop, y = poverty)) +
  geom_point(size = 3, alpha = 0.6, color = "steelblue") +
  geom_smooth(method = "lm", se = TRUE, color = "red", linetype = "dashed") +
  geom_text_repel(aes(label = state), size = 2.5) +
  labs(title = "Urbanization vs. Poverty Rate",
       x = "Urban Population Proportion",
       y = "Poverty Rate (%)") +
  theme_minimal()

ggsave("output/figures/urban_vs_poverty_scatter.png", width = 10, height = 6, dpi = 300)
```

---

## Section 8: Model Development (1 graph)

### Graph 13: Modeling Workflow Diagram
- **Filename:** `modeling_workflow_diagram.png`
- **Type:** Flowchart
- **Purpose:** Visualize analysis pipeline
- **Placement:** Full slide
- **Creation Method:**
  - **Option 1:** Use PowerPoint/Keynote to create manually
  - **Option 2:** Use R's DiagrammeR package:

```r
library(DiagrammeR)

grViz("
digraph modeling_workflow {
  rankdir=LR;
  node [shape=box, style=filled, fillcolor=lightblue];

  Data [label='Raw Data\n(51 states × 31 vars)'];
  Clean [label='Data Cleaning\n& Preprocessing'];
  Split [label='Train/Test Split\n(80/20)'];

  node [fillcolor=lightgreen];
  OLS [label='OLS\nRegression'];
  LASSO [label='LASSO\nRegularization'];
  Tree [label='Decision\nTree'];
  RF [label='Random\nForest'];
  SVR [label='Support Vector\nRegression'];
  DNN [label='Deep Neural\nNetwork'];

  node [fillcolor=lightyellow];
  Eval [label='Model Evaluation\n(RMSE, R²)'];
  Best [label='Best Model\nSelection'];

  Data -> Clean;
  Clean -> Split;
  Split -> OLS;
  Split -> LASSO;
  Split -> Tree;
  Split -> RF;
  Split -> SVR;
  Split -> DNN;

  OLS -> Eval;
  LASSO -> Eval;
  Tree -> Eval;
  RF -> Eval;
  SVR -> Eval;
  DNN -> Eval;

  Eval -> Best;
}
")

# Save as PNG (you may need to screenshot or use webshot package)
```

---

## Section 9: Model Performance (2 graphs)

### Graph 14: Model Comparison - RMSE
- **Filename:** `model_comparison_rmse.png`
- **Type:** Grouped bar chart (train vs. test)
- **Purpose:** Compare overfitting across models
- **Placement:** Left side of slide
- **R Code:**
```r
# Create data frame with results
performance <- data.frame(
  Model = rep(c("OLS", "LASSO", "Tree", "RF", "SVR", "DNN"), 2),
  Dataset = rep(c("Train", "Test"), each = 6),
  RMSE = c(...) # Insert your actual values: train RMSE for 6 models, then test RMSE for 6 models
)

ggplot(performance, aes(x = Model, y = RMSE, fill = Dataset)) +
  geom_col(position = "dodge") +
  labs(title = "Model Performance: Train vs. Test RMSE",
       subtitle = "Lower RMSE = Better Performance",
       x = "Model", y = "Root Mean Squared Error") +
  theme_minimal() +
  scale_fill_manual(values = c("Train" = "lightblue", "Test" = "coral"))

ggsave("output/figures/model_comparison_rmse.png", width = 8, height = 5, dpi = 300)
```

### Graph 15: Model Comparison - R²
- **Filename:** `model_comparison_r2.png`
- **Type:** Lollipop chart
- **Purpose:** Show explained variance for each model
- **Placement:** Right side of slide
- **R Code:**
```r
r2_results <- data.frame(
  Model = c("OLS", "LASSO", "Tree", "RF", "SVR", "DNN"),
  R_squared = c(...)  # Insert your actual R² values
)

ggplot(r2_results, aes(x = reorder(Model, R_squared), y = R_squared)) +
  geom_segment(aes(x = Model, xend = Model, y = 0, yend = R_squared), size = 1.5, color = "gray") +
  geom_point(size = 4, color = "steelblue") +
  coord_flip() +
  labs(title = "Model Performance: R² Values",
       subtitle = "Higher R² = Better Fit",
       x = "Model", y = "R² (Proportion of Variance Explained)") +
  theme_minimal() +
  ylim(0, 1)

ggsave("output/figures/model_comparison_r2.png", width = 6, height = 5, dpi = 300)
```

---

## Section 10: Feature Importance (2 graphs)

### Graph 16: Random Forest Importance
- **Filename:** `random_forest_importance_barplot.png`
- **Type:** Horizontal bar chart
- **Purpose:** Show which variables matter most
- **Placement:** Full slide
- **R Code:**
```r
library(randomForest)

# Assuming you have a fitted Random Forest model called 'rf_model'
importance_df <- data.frame(
  Variable = rownames(importance(rf_model)),
  Importance = importance(rf_model)[, 1]  # %IncMSE
) %>%
  arrange(desc(Importance)) %>%
  slice(1:15)  # Top 15

ggplot(importance_df, aes(x = reorder(Variable, Importance), y = Importance)) +
  geom_col(fill = "forestgreen") +
  coord_flip() +
  labs(title = "Random Forest: Top 15 Most Important Variables",
       subtitle = "Measured by %IncMSE",
       x = "Variable", y = "Importance Score") +
  theme_minimal()

ggsave("output/figures/random_forest_importance_barplot.png", width = 8, height = 6, dpi = 300)
```

### Graph 17: LASSO Coefficient Path
- **Filename:** `lasso_coefficients_plot.png`
- **Type:** Coefficient path plot
- **Purpose:** Show how coefficients shrink as lambda increases
- **Placement:** Full slide
- **R Code:**
```r
library(glmnet)

# Assuming you have a fitted LASSO model
png("output/figures/lasso_coefficients_plot.png", width = 10, height = 6, units = "in", res = 300)
plot(lasso_model, xvar = "lambda", label = TRUE,
     main = "LASSO Coefficient Paths",
     xlab = "Log(Lambda)", ylab = "Coefficient Value")
abline(v = log(optimal_lambda), lty = 2, col = "red")  # Mark optimal lambda
dev.off()
```

---

## Section 11: OLS Model Details (2 graphs)

### Graph 18: OLS Residual Diagnostics
- **Filename:** `ols_residuals_diagnostic.png`
- **Type:** 2×2 diagnostic panel
- **Purpose:** Check regression assumptions
- **Placement:** Full slide
- **R Code:**
```r
# Fit OLS model
ols_model <- lm(poverty ~ ., data = train_data %>% select(-state))

# Create diagnostic plots
png("output/figures/ols_residuals_diagnostic.png", width = 10, height = 10, units = "in", res = 300)
par(mfrow = c(2, 2))
plot(ols_model)
dev.off()
```

### Graph 19: OLS Coefficient Forest Plot
- **Filename:** `ols_coefficients_forest_plot.png`
- **Type:** Forest plot with confidence intervals
- **Purpose:** Show coefficient estimates with uncertainty
- **Placement:** Full slide
- **R Code:**
```r
library(broom)

# Extract coefficients
coef_df <- tidy(ols_model, conf.int = TRUE) %>%
  filter(term != "(Intercept)") %>%
  arrange(estimate)

ggplot(coef_df, aes(x = estimate, y = reorder(term, estimate))) +
  geom_vline(xintercept = 0, linetype = "dashed", color = "red") +
  geom_errorbarh(aes(xmin = conf.low, xmax = conf.high), height = 0.3) +
  geom_point(size = 3, color = "steelblue") +
  labs(title = "OLS Regression Coefficient Estimates",
       subtitle = "With 95% Confidence Intervals",
       x = "Coefficient Estimate", y = "Variable") +
  theme_minimal()

ggsave("output/figures/ols_coefficients_forest_plot.png", width = 8, height = 10, dpi = 300)
```

---

## Section 12: LASSO Details (1 graph)

### Graph 20: LASSO Cross-Validation Lambda Plot
- **Filename:** `lasso_cv_lambda_plot.png`
- **Type:** CV error curve
- **Purpose:** Show optimal lambda selection
- **Placement:** Center of slide
- **R Code:**
```r
library(glmnet)

# Perform cross-validation
cv_lasso <- cv.glmnet(x = as.matrix(train_data %>% select(-state, -poverty)),
                      y = train_data$poverty,
                      alpha = 1, nfolds = 5)

# Plot CV curve
png("output/figures/lasso_cv_lambda_plot.png", width = 8, height = 6, units = "in", res = 300)
plot(cv_lasso, main = "LASSO: Cross-Validation for Lambda Selection")
dev.off()
```

---

## Section 13: Random Forest Details (1 graph)

### Graph 21: Partial Dependence Plots
- **Filename:** `random_forest_partial_dependence.png`
- **Type:** 2×2 grid of partial dependence plots
- **Purpose:** Show marginal effects of top predictors
- **Placement:** Full slide
- **R Code:**
```r
library(pdp)
library(gridExtra)

# Create partial dependence plots for top 4 variables
# (Replace with your actual top variables)
top_vars <- c("log_income", "bachelor", "crime", "avg_rent")

pdp_plots <- lapply(top_vars, function(var) {
  partial(rf_model, pred.var = var, train = train_data) %>%
    autoplot() +
    theme_minimal() +
    labs(title = paste("Partial Dependence:", var))
})

# Combine into grid
png("output/figures/random_forest_partial_dependence.png", width = 10, height = 10, units = "in", res = 300)
grid.arrange(grobs = pdp_plots, ncol = 2)
dev.off()
```

---

## Section 14: Deep Neural Network (1 graph)

### Graph 22: DNN Training History
- **Filename:** `dnn_training_history.png`
- **Type:** Line plot (loss over epochs)
- **Purpose:** Show convergence and overfitting
- **Placement:** Bottom of slide
- **R Code (if using Keras in R):**
```r
library(keras)

# Assuming you have training history from model fit
history_df <- data.frame(
  Epoch = 1:length(history$metrics$loss),
  Train_Loss = history$metrics$loss,
  Val_Loss = history$metrics$val_loss
) %>%
  pivot_longer(cols = c(Train_Loss, Val_Loss), names_to = "Dataset", values_to = "Loss")

ggplot(history_df, aes(x = Epoch, y = Loss, color = Dataset)) +
  geom_line(size = 1) +
  labs(title = "Deep Neural Network Training History",
       x = "Epoch", y = "Mean Squared Error") +
  theme_minimal() +
  scale_color_manual(values = c("Train_Loss" = "blue", "Val_Loss" = "red"))

ggsave("output/figures/dnn_training_history.png", width = 8, height = 5, dpi = 300)
```

---

## Section 15: Key Findings (5 graphs)

### Graph 23: Income vs. Poverty Scatter
- **Filename:** `income_vs_poverty_scatter_regression.png`
- **Type:** Scatterplot with regression line and confidence band
- **Purpose:** Highlight strongest economic predictor
- **Placement:** Center of slide
- **R Code:**
```r
ggplot(data, aes(x = log_income, y = poverty)) +
  geom_point(size = 3, alpha = 0.6, color = "steelblue") +
  geom_smooth(method = "lm", se = TRUE, color = "red", fill = "pink", alpha = 0.3) +
  labs(title = "Per Capita Income vs. Poverty Rate",
       x = "Log(Per Capita Income)",
       y = "Poverty Rate (%)") +
  theme_minimal() +
  annotate("text", x = Inf, y = Inf,
           label = paste0("r = ", round(cor(data$log_income, data$poverty), 2)),
           hjust = 1.1, vjust = 1.5, size = 5)

ggsave("output/figures/income_vs_poverty_scatter_regression.png", width = 8, height = 6, dpi = 300)
```

### Graph 24: Education-Poverty Grouped Scatter
- **Filename:** `education_poverty_grouped_scatter.png`
- **Type:** Scatterplot with regional coloring
- **Purpose:** Show education relationship
- **Placement:** Bottom of slide
- **R Code:**
```r
ggplot(data, aes(x = bachelor, y = poverty, color = region)) +
  geom_point(size = 3, alpha = 0.7) +
  geom_smooth(method = "lm", se = FALSE, color = "black", linetype = "dashed") +
  labs(title = "Bachelor's Degree Attainment vs. Poverty Rate",
       subtitle = "Colored by U.S. Region",
       x = "Bachelor's Degree Completion (%)",
       y = "Poverty Rate (%)") +
  theme_minimal() +
  scale_color_brewer(palette = "Set1")

ggsave("output/figures/education_poverty_grouped_scatter.png", width = 8, height = 6, dpi = 300)
```

### Graph 25: Social Factors Comparison
- **Filename:** `social_factors_comparison.png`
- **Type:** Grouped bar chart
- **Purpose:** Compare crime and drug use by poverty quartile
- **Placement:** Center of slide
- **R Code:**
```r
# Create poverty quartiles
data <- data %>%
  mutate(poverty_quartile = cut(poverty,
                                 breaks = quantile(poverty, probs = 0:4/4),
                                 labels = c("Q1 (Low)", "Q2", "Q3", "Q4 (High)"),
                                 include.lowest = TRUE))

# Calculate means by quartile
social_summary <- data %>%
  group_by(poverty_quartile) %>%
  summarise(Avg_Crime = mean(crime),
            Avg_Drug_Use = mean(drug_use)) %>%
  pivot_longer(cols = c(Avg_Crime, Avg_Drug_Use),
               names_to = "Variable", values_to = "Value")

ggplot(social_summary, aes(x = poverty_quartile, y = Value, fill = Variable)) +
  geom_col(position = "dodge") +
  labs(title = "Social Issues by Poverty Quartile",
       x = "Poverty Quartile", y = "Average Value") +
  theme_minimal() +
  scale_fill_brewer(palette = "Set2")

ggsave("output/figures/social_factors_comparison.png", width = 8, height = 5, dpi = 300)
```

### Graph 26: Predicted vs. Actual (All Models)
- **Filename:** `predicted_vs_actual_all_models.png`
- **Type:** 6-panel faceted scatterplot
- **Purpose:** Visual comparison of model fit
- **Placement:** Full slide
- **R Code:**
```r
# Combine predictions from all models
predictions <- data.frame(
  Actual = rep(test_data$poverty, 6),
  Predicted = c(ols_pred, lasso_pred, tree_pred, rf_pred, svr_pred, dnn_pred),
  Model = rep(c("OLS", "LASSO", "Tree", "RF", "SVR", "DNN"), each = length(test_data$poverty))
)

ggplot(predictions, aes(x = Actual, y = Predicted)) +
  geom_point(alpha = 0.6, color = "steelblue") +
  geom_abline(slope = 1, intercept = 0, color = "red", linetype = "dashed") +
  facet_wrap(~ Model, ncol = 3) +
  labs(title = "Predicted vs. Actual Poverty Rates",
       subtitle = "Red dashed line = perfect prediction",
       x = "Actual Poverty Rate (%)",
       y = "Predicted Poverty Rate (%)") +
  theme_minimal()

ggsave("output/figures/predicted_vs_actual_all_models.png", width = 12, height = 8, dpi = 300)
```

### Graph 27: Prediction Error by State (Best Model)
- **Filename:** `prediction_errors_map.png`
- **Type:** Choropleth map of residuals
- **Purpose:** Geographic pattern of errors
- **Placement:** Next slide
- **R Code:**
```r
# Calculate residuals from best model (e.g., Random Forest)
test_results <- data.frame(
  state = test_data$state,
  actual = test_data$poverty,
  predicted = rf_pred,
  error = test_data$poverty - rf_pred
)

# Merge with map data
test_results$region <- tolower(test_results$state)
map_data_errors <- states_map %>%
  left_join(test_results, by = "region")

# Plot
ggplot(map_data_errors, aes(x = long, y = lat, group = group, fill = error)) +
  geom_polygon(color = "white", size = 0.2) +
  scale_fill_gradient2(low = "blue", mid = "white", high = "red", midpoint = 0,
                       name = "Prediction Error\n(Actual - Predicted)") +
  coord_map("albers", lat0 = 39, lat1 = 45) +
  labs(title = "Random Forest Prediction Errors by State") +
  theme_void()

ggsave("output/figures/prediction_errors_map.png", width = 10, height = 6, dpi = 300)
```

---

## Section 16: Model Validation (1 graph)

### Graph 28: Cross-Validation Performance
- **Filename:** `cv_performance_boxplot.png`
- **Type:** Boxplot showing CV RMSE distribution
- **Purpose:** Show model stability across folds
- **Placement:** Center of slide
- **R Code:**
```r
library(caret)

# Assuming you've stored CV results for each model
cv_results <- data.frame(
  Model = rep(c("OLS", "LASSO", "RF"), each = 5),  # 5-fold CV
  Fold = rep(1:5, 3),
  RMSE = c(...)  # Insert your CV RMSE values for each fold
)

ggplot(cv_results, aes(x = Model, y = RMSE, fill = Model)) +
  geom_boxplot() +
  geom_jitter(width = 0.1, alpha = 0.3) +
  labs(title = "Cross-Validation Performance (5-Fold CV)",
       subtitle = "Distribution of RMSE across folds",
       x = "Model", y = "RMSE") +
  theme_minimal() +
  theme(legend.position = "none")

ggsave("output/figures/cv_performance_boxplot.png", width = 8, height = 5, dpi = 300)
```

---

## Section 17: Robustness Checks (1 table/graph)

### Graph 29: Robustness Checks Summary Table
- **Filename:** `robustness_checks_summary.png`
- **Type:** Formatted table
- **Purpose:** Document sensitivity analyses
- **Placement:** Center of slide
- **R Code:**
```r
robustness <- data.frame(
  Test = c("Baseline (All States)",
           "Exclude DC",
           "Exclude South",
           "Remove High VIF Variables",
           "Log-Transform Outcome"),
  N = c(51, 50, 35, 51, 51),
  RMSE = c(...),  # Insert results
  R_squared = c(...),
  Top_Predictor = c("log_income", "log_income", "bachelor", "crime", "log_income")
)

kable(robustness, format = "html", digits = 3,
      caption = "Robustness Checks: Sensitivity of Results") %>%
  kable_styling(bootstrap_options = c("striped", "hover")) %>%
  save_kable("output/figures/robustness_checks_summary.png")
```

---

## Section 18: Policy Implications (1 graph)

### Graph 30: Policy Lever Effectiveness
- **Filename:** `policy_lever_effectiveness.png`
- **Type:** Horizontal bar chart
- **Purpose:** Rank policy interventions by predicted impact
- **Placement:** Full slide
- **R Code:**
```r
# This is simulated based on your model coefficients
# Calculate predicted poverty change from 1 SD change in each policy lever
policy_impact <- data.frame(
  Policy_Lever = c("Increase bachelor's degree by 5%",
                   "Reduce housing costs by 10%",
                   "Increase minimum wage by $2",
                   "Reduce crime rate by 20%",
                   "Increase public transit funding by 50%"),
  Predicted_Poverty_Change = c(-2.1, -1.8, -0.5, -1.2, -0.3)  # Example values
) %>%
  arrange(Predicted_Poverty_Change)

ggplot(policy_impact, aes(x = reorder(Policy_Lever, Predicted_Poverty_Change),
                          y = Predicted_Poverty_Change)) +
  geom_col(fill = "steelblue") +
  coord_flip() +
  geom_hline(yintercept = 0, linetype = "solid") +
  labs(title = "Predicted Impact of Policy Interventions on Poverty Rate",
       subtitle = "Based on Random Forest model coefficients (simulated scenarios)",
       x = "Policy Intervention",
       y = "Predicted Change in Poverty Rate (percentage points)") +
  theme_minimal()

ggsave("output/figures/policy_lever_effectiveness.png", width = 10, height = 6, dpi = 300)
```

---

## Section 19: Future Directions (1 visual - create manually)

### Visual 31: Future Research Roadmap
- **Filename:** `future_research_roadmap.png`
- **Type:** Timeline or flowchart
- **Purpose:** Show progression of research
- **Placement:** Center of slide
- **Creation Method:** Create in PowerPoint, Lucidchart, or similar tool

**Suggested content:**
- **Phase 1 (2026):** Current cross-sectional study
- **Phase 2 (2027):** Panel data analysis (2020-2025)
- **Phase 3 (2028):** County-level analysis
- **Phase 4 (2029):** Causal inference with natural experiments

---

## Section 20: Additional Recommended Graphs

### Bonus Graph 1: State Rankings Table
- **Filename:** `state_poverty_rankings.png`
- **Type:** Formatted table
- **Purpose:** Show top/bottom 10 states
- **R Code:**
```r
state_rankings <- data %>%
  select(state, poverty, log_income, bachelor, crime) %>%
  arrange(desc(poverty)) %>%
  mutate(Rank = row_number()) %>%
  filter(Rank <= 10 | Rank > 41)  # Top 10 and bottom 10

kable(state_rankings, format = "html", digits = 2,
      caption = "Highest and Lowest Poverty States (2023)") %>%
  kable_styling(bootstrap_options = c("striped", "hover")) %>%
  row_spec(1:10, background = "#ffcccc") %>%  # Highlight high poverty
  row_spec(11:20, background = "#ccffcc") %>%  # Highlight low poverty
  save_kable("output/figures/state_poverty_rankings.png")
```

### Bonus Graph 2: Variable Importance Comparison Across Models
- **Filename:** `importance_comparison_heatmap.png`
- **Type:** Heatmap
- **Purpose:** Show which variables are consistently important
- **R Code:**
```r
# Create matrix of variable importance ranks across models
# (You'll need to extract and standardize importance metrics from each model)

importance_matrix <- data.frame(
  Variable = c(...),  # List of variables
  OLS_pvalue = c(...),  # p-values or standardized coefficients
  LASSO_coef = c(...),
  RF_importance = c(...)
)

# Normalize to 0-1 scale for comparison
importance_matrix_scaled <- importance_matrix %>%
  mutate(across(-Variable, ~scales::rescale(., to = c(0, 1))))

# Reshape and plot
importance_long <- importance_matrix_scaled %>%
  pivot_longer(-Variable, names_to = "Model", values_to = "Importance")

ggplot(importance_long, aes(x = Model, y = Variable, fill = Importance)) +
  geom_tile() +
  scale_fill_gradient(low = "white", high = "darkgreen") +
  labs(title = "Variable Importance Across Models",
       x = "Model", y = "Variable") +
  theme_minimal() +
  theme(axis.text.y = element_text(size = 8))

ggsave("output/figures/importance_comparison_heatmap.png", width = 6, height = 10, dpi = 300)
```

---

## Master Checklist

Use this checklist to track your progress:

### EDA Graphs (12 graphs)
- [ ] Graph 1: Model performance overview
- [ ] Graph 2: U.S. poverty map
- [ ] Graph 3: Variable categories pie chart
- [ ] Graph 4: Summary statistics table
- [ ] Graph 5: Poverty histogram
- [ ] Graph 6: Poverty boxplot
- [ ] Graph 7: Predictor distributions grid
- [ ] Graph 8: Correlation heatmap
- [ ] Graph 9: Top predictors scatterplot matrix
- [ ] Graph 10: VIF barplot
- [ ] Graph 11: Poverty by region boxplot
- [ ] Graph 12: Urban vs. poverty scatter

### Modeling Graphs (18 graphs)
- [ ] Graph 13: Modeling workflow diagram
- [ ] Graph 14: Model comparison RMSE
- [ ] Graph 15: Model comparison R²
- [ ] Graph 16: Random Forest importance
- [ ] Graph 17: LASSO coefficient path
- [ ] Graph 18: OLS residual diagnostics
- [ ] Graph 19: OLS coefficient forest plot
- [ ] Graph 20: LASSO CV lambda plot
- [ ] Graph 21: Partial dependence plots
- [ ] Graph 22: DNN training history
- [ ] Graph 23: Income vs. poverty scatter
- [ ] Graph 24: Education-poverty scatter
- [ ] Graph 25: Social factors comparison
- [ ] Graph 26: Predicted vs. actual (all models)
- [ ] Graph 27: Prediction errors map
- [ ] Graph 28: CV performance boxplot
- [ ] Graph 29: Robustness checks table
- [ ] Graph 30: Policy lever effectiveness

### Bonus Graphs (3 graphs)
- [ ] Graph 31: Future research roadmap (manual)
- [ ] Bonus 1: State poverty rankings table
- [ ] Bonus 2: Importance comparison heatmap

---

## File Organization

Save all graphs in the following directory structure:

```
PROJECT/
└── output/
    └── figures/
        ├── presentation_overview_chart.png
        ├── us_poverty_map.png
        ├── variable_categories_pie.png
        ├── summary_statistics_table.png
        ├── poverty_distribution_histogram.png
        ├── poverty_boxplot.png
        ├── predictor_distributions_grid.png
        ├── correlation_matrix_heatmap.png
        ├── top_predictors_scatter.png
        ├── vif_barplot.png
        ├── poverty_by_region_boxplot.png
        ├── urban_vs_poverty_scatter.png
        ├── modeling_workflow_diagram.png
        ├── model_comparison_rmse.png
        ├── model_comparison_r2.png
        ├── random_forest_importance_barplot.png
        ├── lasso_coefficients_plot.png
        ├── ols_residuals_diagnostic.png
        ├── ols_coefficients_forest_plot.png
        ├── lasso_cv_lambda_plot.png
        ├── random_forest_partial_dependence.png
        ├── dnn_training_history.png
        ├── income_vs_poverty_scatter_regression.png
        ├── education_poverty_grouped_scatter.png
        ├── social_factors_comparison.png
        ├── predicted_vs_actual_all_models.png
        ├── prediction_errors_map.png
        ├── cv_performance_boxplot.png
        ├── robustness_checks_summary.png
        ├── policy_lever_effectiveness.png
        ├── future_research_roadmap.png
        ├── state_poverty_rankings.png
        └── importance_comparison_heatmap.png
```

---

## Tips for Efficient Graph Generation

1. **Create a master R script** that generates all graphs in sequence
2. **Use consistent theming:**
   ```r
   my_theme <- theme_minimal() +
     theme(plot.title = element_text(size = 14, face = "bold"),
           plot.subtitle = element_text(size = 10, face = "italic"),
           axis.title = element_text(size = 10))
   ```
3. **Set standard dimensions:**
   - Full slide: 10×6 inches
   - Half slide: 6×4 inches
   - Tables: 8×5 inches
   - DPI: 300 for all
4. **Use color palettes consistently** (e.g., `viridis`, `RColorBrewer`)
5. **Preview graphs** before final export to check clarity

---

Good luck with your presentation! 🎓📊
