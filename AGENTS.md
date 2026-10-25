# Food Demand Analysis

## Project context

This project analyzes anonymized data for a ready-meal delivery company. The company serves multiple cities through fulfillment centers. Its menu includes items such as drinks, rice, and salads, and it purchases fresh ingredients weekly.

The company has two related operational problems:

- Some centers waste ingredients or prepared meals, especially perishable items.
- Other centers run out of stock, which can prevent customers from placing orders.

Center managers currently request supplies based largely on experience. The marketing team also runs promotions and changes prices, but the company does not know whether those actions increase orders or by how much. Treat the company and its identity as confidential; do not attempt to identify it from the data.

## Data description

The project uses three anonymized CSV files stored in `data/raw/`. Do not edit these source files or upload them to GitHub.

| File | Contents |
| --- | --- |
| `train.csv` | Weekly meal order counts for each fulfillment center and meal over 145 weeks. |
| `fulfilment_center_info.csv` | Fulfillment center information. |
| `meal_info.csv` | Meal information. |

Important columns:

| Column(s) | Meaning |
| --- | --- |
| `week` | Week number; actual calendar dates are not available. |
| `center_id`, `meal_id` | Fulfillment center and meal identifiers used to link the files. |
| `num_orders` | Number of orders. |
| `checkout_price`, `base_price` | Checkout price and base price before discount. |
| `emailer_for_promotion`, `homepage_featured` | Whether the meal was promoted by email and whether it was featured on the homepage; represented as 0 or 1. |
| `center_type`, `op_area` | Fulfillment center type and operating area. |
| `category`, `cuisine` | Meal category and cuisine. |

## Rules

- [Rule 1: Protect the source data](./.github/instructions/rule-1-data.instructions.md)
- [Rule 2: Respect confidentiality](./.github/instructions/rule-2-confidentiality.instructions.md)

## Project goals

Help the business make better, evidence-based demand and inventory decisions by:

1. Translating the business problems into clear, answerable data questions.
2. Combining relevant data tables and documenting cleaning, assumptions, and decisions.
3. Calculating useful performance measures, finding patterns, and testing stated hypotheses.
4. Evaluating, where the available data supports it, how promotions and price changes relate to orders.
5. Providing practical recommendations for reducing waste and stockouts.
6. Presenting the main findings in a concise report that a non-technical audience can understand.

## Instructions for coding agents

- Inspect the repository, data files, and existing notebooks or code before making changes. Follow established project conventions.
- Keep changes focused on the analysis requested. Do not invent columns, business rules, or results; check that the data supports each claim.
- Preserve source data. Do not overwrite, delete, or modify raw datasets unless the user explicitly asks.
- When joining or cleaning data, check keys, row counts, missing values, duplicates, and relevant date or category fields. Document important decisions and limitations.
- Clearly distinguish observed associations from causal conclusions. Do not claim a promotion or price change caused a demand change unless the analysis design supports that conclusion.
- Prefer reproducible analysis and explain metrics, assumptions, and units. Make charts and written conclusions understandable to non-technical readers.
- Write code yourself when using Copilot, then inspect and validate its output. Do not rely on generated code or results without verification.
- Add or update tests for reusable analysis logic when appropriate. Run focused checks for changes and report what was and was not validated.
- Do not add unsupported setup commands or assume a particular dependency manager; use the repository's existing environment and documentation.
