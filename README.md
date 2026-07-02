# Part 3: Regression-Based Business Insights & Model Interpretation

**Analyst:** Harsh Patel  
**Dataset:** Retail Chain Monthly Store Performance Data (320 Records)  
**Tools Used:** Microsoft Excel (Regression Analysis), Python (Data Preparation & Visualisation)

---

## Business Problem Summary

A retail chain leadership team needs to understand which factors are most responsible for variation in monthly store sales across 80 locations over 4 months (January–April 2025). They are considering investment decisions around marketing, staffing, inventory management, discounting, and store format strategy. The objective of this analysis is to use regression modelling to identify which variables are most strongly associated with monthly sales and translate those statistical findings into actionable business recommendations.

---

## Dataset Description

**File:** `data/business_regression_data.xlsx`  
**Records:** 320 (80 unique stores × 4 months)

| Column | Type | Description |
|--------|------|-------------|
| store_id | Categorical | Unique store identifier |
| month | Date | Month of observation (Jan–Apr 2025) |
| region | Categorical | Store region: East, North, South, West |
| store_type | Categorical | Store format: Mall, Residential, High Street, Airport |
| marketing_spend | Numerical | Monthly marketing investment (₹) |
| footfall | Numerical | Total monthly customer visits |
| avg_discount_pct | Numerical | Average discount offered (%) |
| staff_count | Numerical | Number of employees at the store |
| inventory_availability_pct | Numerical | Percentage of time items were in stock |
| competitor_distance_km | Numerical | Distance to nearest competitor (km) — 6 missing values imputed with median |
| holiday_flag | Binary | 1 = holiday month, 0 = non-holiday |
| customer_rating | Numerical | Average customer satisfaction rating (1–5 scale) — 8 missing values imputed with median |
| monthly_sales | **Numerical (Dependent)** | Total monthly store revenue (₹) |
| monthly_profit | Numerical | Monthly profit (excluded from regression — avoid data leakage) |

### Data Cleaning Decisions

- `competitor_distance_km`: 6 missing values replaced with column median
- `customer_rating`: 8 missing values replaced with column median
- `monthly_profit`: Retained in dataset but excluded from regression (it is a downstream outcome of sales, not a predictor)
- `store_id` and `month`: Not used as predictors — identifiers only
- `holiday_flag`: Binary and included in analysis context but not in final multiple regression (low explanatory contribution in full model)

---

## Dependent and Independent Variables

**Dependent Variable (Y):** `monthly_sales`

**Independent Variables (X) — used in models:**

| Variable | Type | Used In |
|----------|------|---------|
| marketing_spend | Continuous | SR-1, MR-1 |
| footfall | Continuous | SR-2, MR-1 |
| inventory_availability_pct | Continuous | SR-3, MR-1 |
| customer_rating | Continuous | SR-4, MR-1 |
| avg_discount_pct | Continuous | SR-5, MR-1 |
| staff_count | Continuous | MR-1 |
| rgn_north, rgn_south, rgn_west | Dummy (Region) | MR-1 |
| stype_residential, stype_high_street, stype_airport | Dummy (Store Type) | MR-1 |

---

## Regression Approach

### Simple Regression Models (SR-1 through SR-5)
Five separate single-predictor regression models were run with `monthly_sales` as the target. These isolate the relationship between each variable and sales revenue, providing a baseline for comparison.

### Multiple Regression Model (MR-1)
A full multiple linear regression model combining all significant numerical predictors plus dummy variables. This is the final recommended model with R² = 0.8353.

---

## Dummy Variable Approach

Two categorical variables were encoded as dummy variables:

### Region (Reference Category: **East**)
| Dummy Column | Category | Value = 1 |
|--------------|----------|-----------|
| rgn_north | North region | North stores |
| rgn_south | South region | South stores |
| rgn_west | West region | West stores |

### Store Type (Reference Category: **Mall**)
| Dummy Column | Category | Value = 1 |
|--------------|----------|-----------|
| stype_residential | Residential | Residential stores |
| stype_high_street | High Street | High Street stores |
| stype_airport | Airport | Airport stores |

**Reference categories (East and Mall)** are excluded from the model to avoid perfect multicollinearity (the dummy variable trap). All coefficients for dummy variables represent performance relative to these reference categories.

Full explanation: see `outputs/model_equations.md`

---

## Model Comparison Summary

| Model | Predictor(s) | R-Squared | Significant? |
|-------|-------------|-----------|--------------|
| SR-1 | marketing_spend | 0.1672 | Yes (p<0.001) |
| SR-2 | footfall | 0.7363 | Yes (p<0.001) |
| SR-3 | inventory_availability_pct | 0.0131 | Borderline |
| SR-4 | customer_rating | 0.0007 | No |
| SR-5 | avg_discount_pct | 0.0083 | No |
| **MR-1 ★** | **All 12 predictors** | **0.8353** | **Multiple vars** |

---

## Final Model Selected

**MR-1 — Full Multiple Linear Regression**

**Why selected:**
- Highest R² (0.8353) — explains over 83% of monthly sales variation
- Adjusted R² = 0.8288 confirms no overfitting
- Includes both operational and structural predictors
- Multiple statistically significant variables (marketing_spend, footfall, inventory_availability_pct, customer_rating, staff_count, rgn_south, rgn_west, stype_residential)
- Provides the richest basis for business decision-making

---

## Business Recommendation (Summary)

The three most impactful controllable drivers of monthly store sales are:

1. **Footfall** (+₹28.53 per additional visitor, p<0.001) — Drive traffic through events, promotions, and digital-to-store campaigns
2. **Inventory Availability** (+₹3,048 per 1% improvement, p<0.001) — Eliminate stockouts; target 92%+ availability
3. **Marketing Spend** (+₹1.18 per ₹1 invested, p<0.001) — Maintain and optimise marketing budgets across stores

Customer rating (+₹14,214 per rating point) and staff count (+₹3,041 per person) are also statistically confirmed contributors. Discounting is **not confirmed** as a reliable sales driver and should not be used as a blanket strategy.

**Regional insight:** South and West stores structurally outperform East by ~₹18,000–₹19,000/month — these regions' operating models should be studied and replicated where possible.

Full details: see `outputs/final_recommendation.md`

---

## Assumptions and Limitations

1. **Linearity:** The regression assumes linear relationships between all predictors and sales. Real-world effects (e.g., diminishing returns on marketing spend) may not be fully linear.
2. **Data duration:** Only 4 months of data (Jan–Apr 2025). Seasonal effects and year-over-year trends are not captured.
3. **Imputed values:** 14 records had missing values and were imputed with medians — this introduces a small estimation bias.
4. **Multicollinearity:** Marketing spend and footfall are likely correlated (spend drives traffic). Coefficients should be interpreted carefully as marginal effects.
5. **Causation:** Regression establishes association, not causation. Controlled experiments are needed before committing major capital based on these findings alone.
6. **Omitted variables:** Store manager quality, lease quality, local competition details, and omnichannel sales are not in the dataset and may explain some of the residual variance.

---

## Screenshots Included

| File | Contents |
|------|----------|
| `screenshots/simple_regression_output.png` | 5-panel chart showing each simple regression model (SR-1 through SR-5) with scatter plot and regression line |
| `screenshots/multiple_regression_output.png` | Full MR-1 output: actual vs predicted scatter, normalised coefficient chart, and complete coefficient table |
| `screenshots/residuals_preview.png` | Residuals vs fitted plot, residual distribution histogram, and top 10 outlier records table |
| `screenshots/model_comparison_preview.png` | R² comparison bar chart, significant variable count, model summary panel, and full comparison table |

---

## Repository Structure

```
part3_regression_insights/
├── data/
│   └── business_regression_data.xlsx       ← Original dataset (unchanged)
├── analysis/
│   ├── regression_workbook.xlsx            ← All regression outputs in Excel
│   ├── model_comparison.md                 ← Detailed model-by-model comparison
│   └── residual_analysis.md               ← Residual interpretation & outlier analysis
├── outputs/
│   ├── regression_summary.xlsx            ← Clean comparison table (Excel)
│   ├── final_recommendation.md            ← Business recommendation document
│   └── model_equations.md                 ← All equations with coefficient explanations
├── screenshots/
│   ├── simple_regression_output.png
│   ├── multiple_regression_output.png
│   ├── residuals_preview.png
│   └── model_comparison_preview.png
└── README.md
```
