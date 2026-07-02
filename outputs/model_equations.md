# Regression Model Equations

**Project:** Retail Store Sales — Regression Analysis  
**Analyst:** Harsh Patel  
**Document Purpose:** Define all regression equations, explain coefficients in business language, and document the dummy variable encoding strategy.

---

## Part 1: Simple Regression Equations

Each equation takes the form: **Monthly Sales = β₀ + β₁ × (Predictor)**

---

### Equation SR-1 — Marketing Spend

```
Monthly Sales = 533,107.16 + 2.1296 × marketing_spend
```

**Coefficient Explanation:**
- **Intercept (₹533,107):** The estimated monthly sales if marketing spend were zero. In practice, this represents baseline sales driven by other factors (location, footfall, reputation).
- **Slope (2.1296):** For every additional ₹1 invested in marketing, monthly sales increase by approximately ₹2.13. This suggests a positive return on marketing investment, though the R² of 0.167 indicates many other factors are also at work.

---

### Equation SR-2 — Footfall

```
Monthly Sales = 165,258.46 + 35.678 × footfall
```

**Coefficient Explanation:**
- **Intercept (₹165,258):** Baseline sales at zero footfall — theoretically represents minimum fixed-cost recovery.
- **Slope (35.678):** Each additional customer visit is associated with ~₹35.68 more in monthly sales. This is a strong, highly significant relationship (R² = 0.736). Footfall is the most important single driver in this dataset.

---

### Equation SR-3 — Inventory Availability

```
Monthly Sales = 461,012.36 + 2,217.83 × inventory_availability_pct
```

**Coefficient Explanation:**
- **Intercept (₹461,012):** Sales expected at 0% inventory availability (no stock at all) — a floor scenario.
- **Slope (2,217.83):** Each 1-percentage-point improvement in inventory availability is linked to ~₹2,218 more in monthly sales. Though the R² is low (0.013), the relationship is statistically significant and operationally meaningful.

---

### Equation SR-4 — Customer Rating

```
Monthly Sales = 706,254.02 + (−5,284.87) × customer_rating
```

**Coefficient Explanation:**
- **Note:** In isolation, the rating coefficient is negative and statistically insignificant (p = 0.638). This is likely a spurious result caused by confounding — other unmeasured variables create the appearance of an inverse relationship. In the full multiple regression model, this reverses to a significantly positive relationship (₹14,214 per rating point).
- **Key learning:** Simple regression with customer_rating alone is misleading. Always interpret in context of other predictors.

---

### Equation SR-5 — Average Discount Percentage

```
Monthly Sales = 772,742.55 + (−138,730.45) × avg_discount_pct
```

**Coefficient Explanation:**
- **Slope (−138,730):** Higher average discount percentages are *negatively* associated with monthly sales in this simple model — which is counterintuitive if discounting is intended to drive volume. This likely reflects that stores in weaker trading positions apply more discounts defensively, creating a statistical association between discounting and poor performance.
- **P-Value:** 0.104 — not statistically significant. Discount effects need full multivariate context to be actionable.

---

## Part 2: Multiple Regression Equation (Final Model — MR-1)

```
Monthly Sales = 60,085.26
               + 1.1846 × marketing_spend
               + 28.527 × footfall
               + 3,047.97 × inventory_availability_pct
               + 14,214.32 × customer_rating
               − 49,245.12 × avg_discount_pct
               + 3,040.77 × staff_count
               + 5,269.53 × rgn_north
               + 19,280.26 × rgn_south
               + 18,016.80 × rgn_west
               − 29,872.11 × stype_residential
               − 10,815.09 × stype_high_street
               + 12,410.20 × stype_airport
```

**Model Fit:** R² = 0.8353 | Adjusted R² = 0.8288 | Residual Std Error = ₹42,931

---

## Part 3: Coefficient-by-Coefficient Business Explanation

| Variable | Coefficient | P-Value | Interpretation |
|----------|-------------|---------|----------------|
| Intercept | ₹60,085 | 0.211 | Baseline sales when all numeric predictors are at zero and store is Mall-type in East region. Not directly interpretable alone. |
| marketing_spend | +₹1.18 per ₹1 | < 0.001 *** | Every rupee of marketing spend generates approximately ₹1.18 in additional monthly sales, holding other factors constant. Statistically strong — invest in marketing. |
| footfall | +₹28.53 per visitor | < 0.001 *** | Each additional monthly visitor adds ~₹28.53 to sales. The most impactful continuous driver in the model. |
| inventory_availability_pct | +₹3,048 per 1% | < 0.001 *** | Each percentage point improvement in stock availability adds ~₹3,048 to monthly sales. Out-of-stock has a real revenue cost. |
| customer_rating | +₹14,214 per point | 0.003 ** | A 1-point improvement in average customer rating (on a 5-point scale) is linked to ~₹14,214 more in sales. Satisfaction converts to revenue. |
| avg_discount_pct | −₹49,245 per 1% | 0.174 ns | Discount-driven revenue uplift is not confirmed by this model. The negative coefficient (not significant) suggests aggressive discounting may not be generating net sales growth. Use discounts selectively. |
| staff_count | +₹3,041 per staff member | 0.015 * | Each additional staff member is associated with ~₹3,041 more in monthly sales — likely reflecting improved service capacity and customer experience. |
| rgn_north (vs East) | +₹5,270 | 0.451 ns | North stores perform marginally above East but the difference is not statistically significant. |
| rgn_south (vs East) | +₹19,280 | 0.007 ** | South region stores outperform East-region stores by ~₹19,280/month on average, holding other factors constant. Investigate what South region does differently. |
| rgn_west (vs East) | +₹18,017 | 0.004 ** | West region stores similarly outperform East by ~₹18,017/month. Regional market dynamics favour West and South. |
| stype_residential (vs Mall) | −₹29,872 | < 0.001 *** | Residential stores generate ~₹29,872 less per month than Mall stores with equivalent inputs. Format has a significant structural impact. |
| stype_high_street (vs Mall) | −₹10,815 | 0.094 ns | High Street stores underperform Mall stores but the difference is borderline significant. |
| stype_airport (vs Mall) | +₹12,410 | 0.197 ns | Airport stores tend to outperform Mall stores but this is not statistically confirmed. |

---

## Part 4: Dummy Variable Encoding

### Why Dummy Variables?

Regression requires numerical inputs. Categorical variables like `region` and `store_type` cannot be used directly. Dummy variable encoding converts each category into a binary column (0 or 1), allowing the regression to estimate the effect of each category relative to a chosen **reference category**.

### Region Dummy Variables

| Dummy Column | Category Represented | Reference (Excluded) |
|--------------|----------------------|----------------------|
| rgn_north | North | **East (reference)** |
| rgn_south | South | **East (reference)** |
| rgn_west | West | **East (reference)** |

**Reference Category:** `East`  
**Rationale:** East was chosen as the reference because it represents a naturally occurring baseline category. All region coefficients are interpreted as performance differences *relative to East-region stores.*

**Why not include all 4?** Including a dummy for every category (East, North, South, West) would create perfect multicollinearity — the "dummy variable trap." One category must always be left out as the reference.

### Store Type Dummy Variables

| Dummy Column | Category Represented | Reference (Excluded) |
|--------------|----------------------|----------------------|
| stype_residential | Residential | **Mall (reference)** |
| stype_high_street | High Street | **Mall (reference)** |
| stype_airport | Airport | **Mall (reference)** |

**Reference Category:** `Mall`  
**Rationale:** Mall is typically the largest-footfall store format and serves as a meaningful performance benchmark. Residential, High Street, and Airport coefficients indicate how each format compares to the Mall baseline.

---

## Part 5: Final Model Selection

**Selected Final Model:** MR-1 (Full Multiple Regression)

**Reasons for selection:**
1. Highest R-Squared (0.8353) — explains over 83% of monthly sales variation
2. Multiple statistically significant predictors — not dependent on a single driver
3. Includes both operational variables and structural/location variables (dummies)
4. Produces actionable, differentiated insights across store type and region
5. Adjusted R² (0.8288) confirms that the additional predictors genuinely improve model fit rather than inflating R² artificially

The model is appropriate for strategic planning and store-level performance benchmarking. Predictions should be treated as directional estimates with ±₹42,931 standard error, not as precise forecasts.
