---
name: discount-correlation
description: "Calculate and interpret Spearman rank correlation between meal discount percentage and order counts. Use for discount-correlation analysis, price-discount relationships, or Spearman tests between checkout_price, base_price, and num_orders."
argument-hint: "Optionally provide a dataframe name or alternative variables to correlate."
user-invocable: true
---

# Discount and Order Correlation

## Purpose

Measure the monotonic association between the relative discount on a meal and its order count using Spearman's rank correlation.

Calculate discount percentage as:

`(base_price - checkout_price) / base_price * 100`

Report the number of valid observations, Spearman's rho, and a two-sided p-value. Interpret the result as an association, not evidence that a discount caused orders to change.

## When to Use

- Analyze the relationship between discount size and meal orders.
- Calculate Spearman correlation for `discount_percent` and `num_orders`.
- Explore whether larger discounts tend to occur alongside higher or lower orders.

## Procedure

1. Inspect the existing analysis dataframe and verify it contains `base_price`, `checkout_price`, and `num_orders`.
2. Exclude rows missing any of these required fields and report the valid observation count.
3. Check that `base_price` is nonzero before calculating the discount rate; raise an error if a zero base price is present.
4. Calculate each observation's relative discount as a percentage of `base_price`. Keep negative discounts when `checkout_price` is greater than `base_price`; do not silently clip or recode them.
5. Run a two-sided Spearman rank correlation with ties handled by SciPy's implementation. Report rho, p-value, and observation count.
6. Format extremely small p-values without rounding them to `0.0000`. If floating-point underflow occurs, report that the value is below representable precision (for example, `< 1e-300`).
7. Explain that rho measures direction and strength of a monotonic association, not causation. In weekly meal-demand data, repeated observations for the same center and meal and possible confounding factors limit the interpretation of naive p-values.

## Reference Implementation

Assumes `df` is the existing pandas DataFrame.

```python
# Step 1: Import Spearman's rank-correlation test and validate the required fields.
from scipy.stats import spearmanr

correlation_columns = {"base_price", "checkout_price", "num_orders"}
missing_columns = correlation_columns.difference(df.columns)
if missing_columns:
    raise ValueError(
        f"Missing columns for Spearman correlation: {sorted(missing_columns)}"
    )

# Step 2: Keep complete observations in a separate analysis copy.
correlation_data = df[
    ["base_price", "checkout_price", "num_orders"]
].dropna().copy()
if correlation_data.empty:
    raise ValueError("No complete observations are available for correlation.")

# Step 3: Validate the denominator before calculating discount percentage.
if correlation_data["base_price"].eq(0).any():
    raise ValueError(
        "Discount percentage cannot be calculated when base_price is zero."
    )

# Step 4: Express the difference between base and checkout prices as a percentage.
correlation_data["discount_percent"] = (
    (correlation_data["base_price"] - correlation_data["checkout_price"])
    / correlation_data["base_price"]
) * 100

# Step 5: Test the two-sided monotonic association; SciPy handles ties in the ranks.
spearman_result = spearmanr(
    correlation_data["discount_percent"],
    correlation_data["num_orders"],
    alternative="two-sided",
    nan_policy="omit",
)

# Step 6: Preserve very small p-values instead of rounding them to zero.
p_value_display = (
    f"{spearman_result.pvalue:.3e}"
    if spearman_result.pvalue > 0
    else "< 1e-300 (floating-point underflow)"
)

# Step 7: Present the result with its sample size and cautious interpretation.
spearman_summary = pd.Series({
    "valid_observations": len(correlation_data),
    "spearman_rho": spearman_result.statistic,
    "p_value": p_value_display,
})
print("Spearman rank correlation: discount percentage vs. num_orders")
display(spearman_summary.to_frame("value").round(4))
print(
    "Rho describes the direction and strength of a monotonic association, "
    "not a causal effect."
)
print(
    "Repeated center-meal-week observations and other factors limit naive "
    "p-value interpretation."
)
```

## Interpretation Safeguards

- Correlation is not causation; do not claim discounts caused order changes.
- Spearman rho measures monotonic rank association, not a linear relationship.
- Retain negative discounts unless the analysis specification explicitly defines a different treatment.
- Report missing-value exclusions and valid sample size.
- Do not report a p-value as exactly zero due to display rounding or floating-point underflow.
- Repeated weekly observations and potential confounders (meal popularity, fulfillment center, week, promotions, and other pricing differences) mean the naive correlation p-value may overstate inferential certainty.
