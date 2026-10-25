# Food demand: turn volatile orders into a better planning signal

## The takeaway

Weekly order volume can swing sharply, while demand is concentrated in a few meal categories. That makes center- and meal-level forecasting more useful than relying on one overall average. Promotion and discount patterns are promising signals to investigate, but this observational analysis cannot show that either action caused orders to change.

## What the data shows

### 1. Weekly demand is uneven

Across 145 observed weeks, average demand was **824,534 orders per week**. The highest total was **1,303,457** in week 48; the lowest was **380,065** in week 62. The largest week-to-week rise was **495,188 orders (+61.3%)**, and the largest fall was **480,040 orders (-55.8%)**.

![Weekly order totals, with the observed high and low](./figures/01_weekly_demand.png)

Week numbers are not calendar dates. The chart identifies observed peaks and troughs, but it cannot attribute them to seasons, holidays, or particular events.

### 2. A small number of categories account for much of demand

**Beverages** led all categories with **40.5 million orders**. Rice Bowl and Sandwich followed. Together, those three categories accounted for **66.07%** of all orders.

![Five largest categories by total orders](./figures/02_top_categories.png)

This concentration can help prioritize forecast review and replenishment planning. It does not, by itself, reveal ingredient usage, spoilage, or stock availability.

### 3. Center type is associated with different order volumes

TYPE_B centers averaged **2.00 million orders per center** across the observed period, compared with **1.60 million** for TYPE_A and **1.08 million** for TYPE_C.

![Average cumulative orders per center by type](./figures/03_center_type_average.png)

The chart uses per-center averages so the comparison is not driven solely by how many centers belong to each type. It remains descriptive: center type is not an explanation for the difference, and center-level operating conditions may vary.

### 4. Promotion exposure lines up with higher orders in matched comparisons

Among center-meal pairs where both promoted and unpromoted observations were available, the median of pair-level medians was **325 vs. 134 orders** for email promotion and **311 vs. 123 orders** for homepage featuring.

![Median orders with and without promotion within matched center-meal pairs](./figures/04_promotion_medians.png)

Separate two-sided Mann–Whitney tests found adjusted distributional differences in **2,344 of 2,830** email-promotion pairs and **2,315 of 2,928** homepage-featured pairs after Benjamini–Hochberg adjustment at **0.05**. These comparisons control descriptively for center and meal identity, but weekly observations still repeat within each pair. The standard p-values are therefore exploratory, not confirmatory; promotion exposure may also coincide with other changes.

### 5. Discount and order volume have only a weak rank association

Across **456,548 observations**, Spearman’s rank correlation between relative discount and orders was **ρ = 0.122**, a weak positive association. Average orders varied across discount bands, but the pattern is not a causal estimate and may reflect different meals, centers, weeks, or promotion conditions.

![Mean orders by relative-discount quintile, with Spearman correlation](./figures/05_discount_order_association.png)

## Recommended next steps

1. **Use weekly and center-meal demand histories to guide a forecasting pilot.** Review unusually high- and low-demand weeks and validate forecast accuracy before changing replenishment quantities.
2. **Prioritize forecast attention for high-volume categories and local center leaders.** Pair order data with ingredient requirements, actual stockouts, waste, and availability before making inventory decisions.
3. **Test promotions and price changes with a stronger design.** Use a randomized or otherwise controlled pilot, or a model that accounts for repeated center-meal observations, before treating these associations as expected lift.

## Scope and interpretation

- The analysis uses the prepared merged dataset at `data/raw/cleaned_merged_data.csv`; raw source files were not changed.
- Order totals describe observed demand. The available data does not directly measure inventory, waste, stockouts, costs, or profit.
- Promotion and discount comparisons are observational. They describe associations and do not prove causal effects.
- Week numbers have no corresponding calendar dates, so no seasonal or event-based explanation is inferred.
- The Mann–Whitney tests compare order distributions, not means. Their repeated weekly observations limit the usual independence assumption.
