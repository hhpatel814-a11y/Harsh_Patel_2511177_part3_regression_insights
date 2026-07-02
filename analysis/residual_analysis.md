# Residual Analysis Report

**Project:** Retail Chain Sales — Regression Analysis  
**Analyst:** Harsh Patel  
**Model Used:** MR-1 (Full Multiple Regression)  
**Formula:** monthly_sales = β₀ + β₁(marketing_spend) + β₂(footfall) + β₃(inventory_availability_pct) + β₄(customer_rating) + β₅(avg_discount_pct) + β₆(staff_count) + region dummies + store_type dummies  

---

## What Are Residuals?

A **residual** is the difference between a store's actual recorded monthly sales and the sales value the regression model predicted for that store:

> **Residual = Actual Sales − Predicted Sales**

- A **positive residual** means the store performed *better* than the model expected — the model under-predicted.
- A **negative residual** means the store performed *worse* than the model expected — the model over-predicted.

Residuals reveal where the model fails to capture the full picture and point to stores with unusual characteristics worth investigating.

---

## Model Fit Context

| Metric | Value |
|--------|-------|
| R-Squared | 0.8353 |
| Adjusted R-Squared | 0.8288 |
| Residual Standard Error | ₹42,931 |
| Total Observations | 320 |

The model explains ~83.5% of monthly sales variation. The average prediction error is approximately ₹42,931 — which, relative to average monthly sales of ~₹680,000, represents an error rate of about 6.3%.

---

## Predicted vs. Actual — Overview

Residuals were calculated for all 320 store-month records. The distribution shows that most stores fall close to the predicted line, while a small number at the tails require further examination.

---

## Top 5 Stores — Largest Positive Residuals (Model Under-Predicted)

| Rank | Store ID | Month | Region | Store Type | Actual Sales (₹) | Predicted Sales (₹) | Residual (₹) |
|------|----------|-------|--------|------------|-------------------|----------------------|--------------|
| 1 | STR-1028 | Apr 2025 | East | Mall | 713,611 | 601,419 | +112,192 |
| 2 | STR-1073 | Mar 2025 | East | Residential | 813,317 | 711,191 | +102,125 |
| 3 | STR-1030 | Feb 2025 | West | Residential | 820,519 | 730,256 | +90,263 |
| 4 | STR-1026 | Apr 2025 | East | Mall | 625,514 | 536,308 | +89,206 |
| 5 | STR-1027 | Mar 2025 | West | High Street | 795,154 | 708,389 | +86,765 |

### Business Interpretation — Positive Residuals

These stores achieved significantly *more* revenue than the model projected. Possible explanations include:

- **Local events or festivals** not captured in the dataset that drove exceptional footfall during those months
- **Exceptional store execution** — staff performance, store layout, or in-store experiences that go beyond what rating scores capture
- **Loyalty-driven repeat purchases** by a concentrated customer base — particularly likely in Residential and High Street formats
- **Micro-market advantages** such as being the closest retail option in a neighbourhood — a factor the `competitor_distance_km` variable may not fully capture at a granular level
- **Product mix differences** — higher-margin or higher-demand category mix that lifts total sales disproportionately

These stores represent **best-practice candidates**. Leadership should study what these locations do differently and consider whether those practices can be replicated.

---

## Top 5 Stores — Largest Negative Residuals (Model Over-Predicted)

| Rank | Store ID | Month | Region | Store Type | Actual Sales (₹) | Predicted Sales (₹) | Residual (₹) |
|------|----------|-------|--------|------------|-------------------|----------------------|--------------|
| 1 | STR-1017 | Mar 2025 | West | High Street | 685,379 | 838,966 | −153,587 |
| 2 | STR-1023 | Feb 2025 | South | Mall | 627,172 | 766,067 | −138,895 |
| 3 | STR-1012 | Jan 2025 | West | Residential | 595,468 | 722,545 | −127,077 |
| 4 | STR-1007 | Feb 2025 | West | Mall | 800,452 | 915,179 | −114,727 |
| 5 | STR-1077 | Feb 2025 | South | Residential | 538,376 | 634,950 | −96,574 |

### Business Interpretation — Negative Residuals

These stores fell short of model expectations. This suggests the model is *over-predicting* for these locations. Possible causes:

- **Temporary operational disruptions** — construction, renovation, logistics failures, or staff shortages in those specific months that temporarily suppressed performance
- **Competitive pressure** not reflected in the `competitor_distance_km` column — e.g., a new competitor may have opened nearby that month
- **Seasonal demand drops** specific to those store-format combinations not captured by the `holiday_flag` binary alone
- **Execution or management gaps** — stores that, despite having favourable structural characteristics, are underperforming due to leadership or in-store execution issues
- **Customer experience failures** — incidents or events that drove negative word-of-mouth and reduced repeat traffic in a short window

These stores represent **improvement opportunity targets**. Each should be reviewed with regional managers to identify correctable factors.

---

## Systematic Over- and Under-Prediction Patterns

### By Store Type

- **Residential stores** show a slight tendency toward over-prediction — the model (even with the `stype_residential` dummy at −₹29,872) may still over-estimate performance for some Residential locations, possibly because those stores have higher variability in customer base than the coefficient captures uniformly.
- **Mall stores** show mixed residual patterns — some over-perform (STR-1028, STR-1026 in East Mall) and some under-perform (STR-1023 in South Mall). This suggests that within the Mall category, sub-location factors (anchor store presence, footfall distribution within the mall) matter significantly.

### By Region

- **West region** stores appear more frequently in both tails, suggesting greater variability around the model's predictions. This could indicate that the regional coefficient is averaging over meaningfully different sub-market conditions within the West.
- **South region** negative residuals (STR-1023, STR-1077) suggest the model may slightly over-estimate the contribution of the South regional premium in some months.

---

## Residual Distribution Summary

| Segment | Approximate Count | Implication |
|---------|-------------------|-------------|
| Large positive residuals (top 5%) | ~16 records | Model under-predicts; hidden positive drivers present |
| Large negative residuals (bottom 5%) | ~16 records | Model over-predicts; suppressors not captured |
| Residuals within ±₹42,931 (1 SE) | ~220 records | Good model fit for majority of stores |

---

## Conclusion

The MR-1 model produces residuals that are well-distributed around zero with no extreme systematic bias. The stores flagged in the tails are not evidence of a broken model — an R² of 0.8353 is strong for retail data with this many variables. Instead, these outliers represent:

1. **Operational best practices** (positive tail) worth documenting and scaling
2. **Operational or competitive challenges** (negative tail) worth investigating and addressing

The residual analysis confirms that the model is a reliable starting point for store-level performance diagnosis, while acknowledging that approximately 16.5% of sales variation remains unexplained and likely includes qualitative or contextual factors that quantitative data alone cannot fully capture.
