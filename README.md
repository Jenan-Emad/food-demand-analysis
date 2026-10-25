# Food Demand Analysis

## Problem

Fulfillment centers plan meal supplies largely from experience. This analysis uses weekly order history to describe demand changes, identify high-volume meals and centers, and examine how observed prices and promotions relate to orders. The aim is to inform better forecasting and operational review—not to claim a cause from observational data.

## Three important results

1. **Demand varies substantially by week.** Average weekly orders were **824,534** across the observed period. The highest total was **1,303,457** in week 48 and the lowest was **380,065** in week 62.  
   Source: `notebooks/analysis.ipynb`, `weekly-demand` cell.
2. **Orders are concentrated in a few categories.** Beverages, Rice Bowl, and Sandwich together accounted for **66.07%** of all orders; Beverages alone accounted for **33.86%**.  
   Source: `notebooks/analysis.ipynb`, `meal-mix` cell.
3. **Promotion exposure is associated with higher orders in matched comparisons, but causality is not established.** After Benjamini–Hochberg adjustment at **0.05**, differences were significant in **2,344 of 2,830** email-promotion center-meal pairs and **2,315 of 2,928** homepage-featured pairs. Repeated weekly observations mean these test results are exploratory.  
   Source: `notebooks/analysis.ipynb`, `80b44d5a` cell.

## Additional questions explored

Beyond the six core questions referenced in the project brief, the analysis notebook also explores:

- Which meal is most ordered in each fulfillment center? (`top-meal-by-center`)
- Which cuisines account for the most orders? (`meal-mix`)
- Are order counts associated with email promotion or homepage featuring when comparing the same meal within the same center? (`80b44d5a`)
- How do observed orders vary across `checkout_price` bands? (`price-area-association`)
- Is `op_area` associated with total orders by center? (`price-area-association`)
- Is relative discount associated with `num_orders` according to Spearman rank correlation? (`spearman-discount-correlation`)

These are descriptive or observational questions. The promotion and price analyses do not establish causal effects.

## Project contents

- `data/raw/`: source CSVs and the prepared merged dataset.
- `notebooks/clean_data.ipynb`: merges the training, meal, and fulfillment-center data and prepares `cleaned_merged_data.csv`.
- `notebooks/analysis.ipynb`: calculates demand, center, promotion, price, and correlation results.
- `notebooks/linear_regression.ipynb`: trains and validates a linear regression model, then predicts orders for every row in test weeks 146–155.
- `data/processed/linear_regression_predictions.csv`: generated linear regression predictions, with one predicted order count for each test row.
- `notebooks/random_forest.ipynb`: trains a Random Forest model to predict orders for the test dataset.
- `demand_story.md`: narrative summary and recommendations.
- `reports/client_report.docx`: Arabic client report with embedded charts and RTL formatting.
- `.github/skills/client-report-ar/build_report.py`: script that assembles the Arabic Word report from the saved notebook results and chart images.
- `reports/figures/`: PNG charts used in the reports.

## Available skills

Project skills are stored in `.github/skills/`. Use them when a task matches their focus:

- [`clear-charts`](.github/skills/clear-charts/SKILL.md): Create clear Matplotlib charts suited to comparisons, trends, and distributions.
- [`data-quality-report`](.github/skills/data-quality-report/SKILL.md): Inspect a CSV or table for data-quality issues and summarize findings.
- [`data-storytelling`](.github/skills/data-storytelling/SKILL.md): Turn analysis results into a focused narrative with supporting visuals and recommendations.
- [`discount-correlation`](.github/skills/discount-correlation/SKILL.md): Calculate and interpret Spearman correlation between relative meal discounts and order counts.
- [`find-skills`](.github/skills/find-skills/SKILL.md): Discover skills that may help with a requested task.
- [`insight-report`](.github/skills/insight-report/SKILL.md): Write evidence-based Arabic insights and recommendations from notebook results.
- [`promotion-order-mann-whitney`](.github/skills/promotion-order-mann-whitney/SKILL.md): Compare promoted and unpromoted order distributions with Mann–Whitney U tests.
- [`client-report-ar`](.github/skills/client-report-ar/SKILL.md): Generate a final Arabic Word client report from notebook results and report figures.

## How to run

### Requirements

Use Python with Jupyter and these notebook libraries: `pandas`, `matplotlib`, `scipy`, `scikit-learn`, and `python-docx` for Word report generation. The notebooks are intended to be run from the project root so their relative data paths resolve.

If those packages are not installed in your Python environment, install them with:

```powershell
python -m pip install pandas matplotlib scipy scikit-learn python-docx jupyter ipykernel
```

### Run interactively in Jupyter

From the project root, launch Jupyter:

```powershell
python -m jupyter lab
```

Open `notebooks/clean_data.ipynb` and run its cells from top to bottom to prepare the merged dataset. Then open `notebooks/analysis.ipynb` and run its cells from top to bottom. To predict orders for the next 10 test weeks, run `notebooks/linear_regression.ipynb` from top to bottom. The notebook fits on `train.csv`, evaluates the model on the latest 10 labeled weeks, then refits using all training weeks to create one prediction per test row. To use the Random Forest model instead, run `notebooks/random_forest.ipynb` from top to bottom.

The cleaning notebook writes the derived file `data/raw/cleaned_merged_data.csv`. It does not edit `train.csv`, `meal_info.csv`, or `fulfilment_center_info.csv`. The analysis notebook reads the prepared merged file and stores its cell outputs in the notebook when saved. The linear regression notebook saves its predictions to `data/processed/linear_regression_predictions.csv`; the Random Forest notebook saves its predictions to `data/processed/random_forest_predictions.csv`. Both prediction files are derived outputs, separate from the raw datasets.

### Build the final Arabic client report

From the project root, generate the client-ready Word document from the saved notebook results and figures:

```powershell
py -3 .github/skills/client-report-ar/build_report.py
```

This creates `reports/client_report_U.docx` and validates that the output contains the expected charts, RMSE text, and RTL formatting.

### Run and save notebook outputs from PowerShell

Alternatively, from the project root:

```powershell
python -m nbconvert --to notebook --execute --inplace notebooks/clean_data.ipynb
python -m nbconvert --to notebook --execute --inplace notebooks/analysis.ipynb
python -m nbconvert --to notebook --execute --inplace notebooks/linear_regression.ipynb
python -m nbconvert --to notebook --execute --inplace notebooks/random_forest.ipynb
```

These commands execute the notebooks and update their saved outputs. Run the cleaning notebook first when you need to regenerate the merged dataset.

## Data and interpretation notes

- Keep the original source CSVs unchanged and do not publish them.
- `week` is a week number; calendar dates are unavailable, so seasonal or holiday explanations cannot be confirmed.
- The data records orders, not inventory availability, waste, or stockouts.
- Promotion and price comparisons are observational associations, not proof of causal effects.
- Mann–Whitney comparisons use repeated weekly records within center-meal pairs; treat their p-values as exploratory.


## How I do use AI in this project
- this project is almost generated by copilot agent
- My role was to write good prompt and the agent apply
- The decisions are mine but the agent generate the code which save my time and effort
- I use agent to generate the notebooks code, check data problems, creating skills and generate linear regression prediction