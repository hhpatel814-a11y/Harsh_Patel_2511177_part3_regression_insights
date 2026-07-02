# Regression Model Comparison

**Project:** Retail Chain Sales Analysis  
**Analyst:** Harsh Patel  
**Objective:** Compare simple and multiple regression models to identify the most reliable framework for understanding monthly store sales.

---

## Models Evaluated

| # | Model ID | Type | Independent Variable(s) | R-Squared |
|---|----------|------|--------------------------|-----------|
| 1 | SR-1 | Simple Linear | marketing_spend | 0.1672 |
| 2 | SR-2 | Simple Linear | footfall | 0.7363 |
| 3 | SR-3 | Simple Linear | inventory_availability_pct | 0.0131 |
| 4 | SR-4 | Simple Linear | customer_rating | 0.0007 |
| 5 | SR-5 | Simple Linear | avg_discount_pct | 0.0083 |
| 6 | MR-1 | Multiple Linear | All 12 predictors (3 numerical + dummies) | **0.8353** |

---

## Detailed Model Profiles

### SR-1 — Marketing Spend vs Monthly Sales

- **Variables:** monthly_sales (Y) ~ marketing_spend (X)  
- **R-Squared:** 0.1672  
- **Slope:** 2.1296 (each additional ₹1 in marketing is linked to ~₹2.13 increase in sales)  
- **P-Value:** < 0.001 — statistically significant  
- **Significant Variables:** marketing_spend  
- **Business Usefulness:** Useful for justifying marketing investment. Shows a real, positive link between spend and revenue, though marketing alone explains only ~17% of sales variation.  
- **Limitation:** Ignores footfall, inventory, location, store format — major omissions that likely drive most of sales variance.

---

### SR-2 — Footfall vs Monthly Sales

- **Variables:** monthly_sales (Y) ~ footfall (X)  
- **R-Squared:** 0.7363  
- **Slope:** 35.678 (each additional visitor adds ~₹35.68 to monthly sales)  
- **P-Value:** < 0.001 — highly significant  
- **Significant Variables:** footfall  
- **Business Usefulness:** The strongest single-predictor model. Nearly 74% of sales variation is explained by store traffic alone. This validates footfall-driving strategies like promotional events, accessibility improvements, and digital-to-store campaigns.  
- **Limitation:** Does not account for conversion rate, basket size, or discount effects.

---

### SR-3 — Inventory Availability vs Monthly Sales

- **Variables:** monthly_sales (Y) ~ inventory_availability_pct (X)  
- **R-Squared:** 0.0131  
- **Slope:** 2217.83 (1% improvement in availability adds ~₹2,218 in sales)  
- **P-Value:** 0.041 — marginally significant  
- **Significant Variables:** inventory_availability_pct (borderline)  
- **Business Usefulness:** Even though R² is low in isolation, the directional finding is operationally meaningful — stockouts cost revenue. In the multiple regression model, this variable is much more significant.  
- **Limitation:** Very low standalone explanatory power. Confounded by store type and footfall.

---

### SR-4 — Customer Rating vs Monthly Sales

- **Variables:** monthly_sales (Y) ~ customer_rating (X)  
- **R-Squared:** 0.0007  
- **Slope:** −5284.87 (counterintuitive negative direction in simple model)  
- **P-Value:** 0.638 — not significant  
- **Significant Variables:** None  
- **Business Usefulness:** Customer rating alone is a poor predictor of sales volume. This does not mean rating is unimportant — it likely interacts with other variables (footfall, repeat traffic). Its impact becomes more meaningful in the full model.  
- **Limitation:** In simple regression, the negative slope may reflect confounding — perhaps lower-footfall stores also receive different rating profiles.

---

### SR-5 — Discount Percentage vs Monthly Sales

- **Variables:** monthly_sales (Y) ~ avg_discount_pct (X)  
- **R-Squared:** 0.0083  
- **Slope:** −138,730.45 (higher discounts appear negatively linked to total sales in this model)  
- **P-Value:** 0.104 — not statistically significant  
- **Significant Variables:** None  
- **Business Usefulness:** Weak as a standalone predictor. The negative coefficient suggests that heavy discounting does not reliably drive revenue uplift. Leadership should be cautious about using discounts as a primary sales strategy.  
- **Limitation:** Non-linear relationship likely. Discounts may drive volume but reduce average transaction value — which affects total sales calculation differently.

---

### MR-1 — Full Multiple Regression Model ★ (Recommended)

- **Variables:** monthly_sales (Y) ~ marketing_spend + footfall + inventory_availability_pct + customer_rating + avg_discount_pct + staff_count + dummy variables for region (North, South, West; ref=East) and store type (Residential, High Street, Airport; ref=Mall)
- **R-Squared:** 0.8353  
- **Adjusted R-Squared:** 0.8288  
- **Key Significant Predictors:** marketing_spend, footfall, inventory_availability_pct, customer_rating, staff_count, rgn_south, rgn_west, stype_residential  
- **Business Usefulness:** This model explains approximately 83.5% of monthly sales variation — a strong, actionable result. It incorporates both operational factors and structural store characteristics, making it the most comprehensive decision-support tool available.  
- **Limitation:** Assumes linear relationships across all predictors. Multicollinearity may exist between marketing_spend and footfall. Does not capture seasonality or competitive activity beyond competitor distance.

---

## Summary Comparison Table

| Criterion | SR-1 | SR-2 | SR-3 | SR-4 | SR-5 | MR-1 (★ Best) |
|-----------|------|------|------|------|------|--------------|
| R-Squared | 0.167 | 0.736 | 0.013 | 0.001 | 0.008 | **0.835** |
| Statistically Significant | ✔ | ✔ | Borderline | ✗ | ✗ | ✔ Multiple vars |
| Captures Multiple Factors | ✗ | ✗ | ✗ | ✗ | ✗ | ✔ |
| Decision-Ready | Partial | Partial | Partial | No | No | **Yes** |
| Recommended | No | No | No | No | No | **Yes** |

---

## Conclusion

The **MR-1 Full Multiple Regression Model** is the clear winner. It delivers the highest explanatory power (R² = 0.8353) and incorporates the full range of business-relevant variables. Among simple models, **SR-2 (Footfall)** stands out as the most informative single predictor and should be a priority metric for operations teams.
