---
name: promotion-order-mann-whitney
description: "Compare meal order counts between promoted and unpromoted observations using two-sided Mann–Whitney U tests. Use when measuring the association of email promotions or homepage featuring with num_orders in the food-demand analysis."
argument-hint: "Optionally provide the data file path or significance level."
user-invocable: true
---

# Promotion and order-count comparison

## Purpose

Assess whether the distribution of meal order counts differs between observations with and without:

1. An email promotion (`emailer_for_promotion`).
2. Homepage featuring (`homepage_featured`).

Use a separate two-sided Mann–Whitney U test for each indicator. The test compares distributions; it does not establish that a promotion caused a difference in orders.

## Procedure

1. Inspect the repository and existing notebooks, scripts, and dependency files. Follow the established analysis conventions.
2. Load the relevant order data, normally `data/raw/train.csv`. Verify that `num_orders`, `emailer_for_promotion`, and `homepage_featured` exist and inspect their types, missing values, and distinct indicator values.
3. For each indicator, form two independent groups of `num_orders`: indicator equals `1` and indicator equals `0`. Exclude rows missing the group indicator or order count, and report how many rows were excluded. Do not silently reinterpret unexpected indicator values; investigate and explain them.
4. Report the sample size and median order count for each group, and summarize the distributions (for example, with boxplots or empirical distribution plots) so the reader can understand the comparison.
5. Run a two-sided Mann–Whitney U test for each comparison using the repository's existing statistical tooling (for example, `scipy.stats.mannwhitneyu` with `alternative="two-sided"`). State the U statistic, p-value, and significance threshold used (use 0.05 unless the user specifies another threshold). Use a tie-aware method or the library's documented default when ties occur, and report the method.
6. Describe whether the evidence indicates a difference in distributions at the chosen threshold. Do not describe a significant result as a difference in medians unless the group distributions have sufficiently similar shapes.
7. Treat the findings as associations, not causal effects. The dataset is observational and has repeated meal/fulfillment-center observations across weeks; discuss the independence limitation and avoid presenting the naive p-values as definitive if that dependence is not addressed.
8. Summarize the two comparisons in a compact table with the indicator, group sample sizes, group medians, U statistic, p-value, threshold, and a plain-language conclusion. Include any data exclusions and limitations.

## Interpretation safeguards

- Do not confuse `emailer_for_promotion` (email promotion) with `homepage_featured` (homepage feature); test them separately.
- Do not infer that either promotion caused higher or lower order counts. Price, meal, fulfillment center, week, and other factors may differ between groups.
- A Mann–Whitney result tests for a difference in ranks/distributions, not specifically a difference in means.
- Consider the repeated observations for each `center_id` and `meal_id` over `week`. If a valid independence assumption cannot be justified, clearly qualify the standard test and recommend a method that accounts for repeated/grouped observations for confirmatory inference.
- Do not modify raw data files. Keep analysis reproducible and preserve the source dataset.
