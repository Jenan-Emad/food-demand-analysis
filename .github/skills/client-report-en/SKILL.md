---
name: client-report-en
description: "Create or update the final English-language client report as a Word document from analysis.ipynb results, reports/figures, and linear-regression notebook metrics including RMSE."
---

# English Client Report

Create a polished, evidence-based English Word report for a non-technical business audience. Save it as `reports/client_report_en.docx`.

## Inputs and output

- Analysis notebook: `notebooks/analysis.ipynb`.
- Figures: every supported image in `reports/figures/`.
- Saved linear-regression notebook: `notebooks/linear_regression.ipynb`, or `notebooks/lenier_regression.ipynb` if that is the existing filename. Extract RMSE and any available supporting validation metrics from saved cell outputs.
- Training data: `data/raw/train.csv`, read-only, to calculate the distribution of `num_orders` and compare it with model error.
- Output: `reports/client_report_en.docx`.

## Procedure

1. Read `notebooks/analysis.ipynb` in full, including code cells, markdown cells, and saved outputs. Follow the analysis sequence and use only results supported by the saved outputs. If a key result is absent or cells have not been run, state that clearly; do not invent or infer a number.
2. Read the linear-regression notebook and extract `Root mean squared error (orders)` from its saved validation output. Also report MAE and validation weeks/rows when available. Do not reuse metrics from an older run that are not present in the current notebook outputs.
3. Read every image in `reports/figures/`, connect it to the analysis step it represents, and include it in the appropriate report section. Give each image an English title and concise caption. Do not omit, alter, or overwrite figure files. Include additional images in a clearly labeled additional-results section. If an image format cannot be embedded, stop and report the unsupported file rather than silently skipping it.
4. Write the report in clear, concise English with these sections:
   - Title and cover page.
   - Executive summary.
   - Objective and data.
   - Key findings, with all figures and captions.
   - Predictive model results and an explanation of RMSE.
   - Practical recommendations.
   - Notes and limitations.
5. Explain RMSE accurately: it measures prediction error in the target's units (orders per center-meal record), gives larger errors more weight, and is not a simple arithmetic average of errors. Compare it with the mean and range of `num_orders` calculated from the training CSV. State that there is no universal threshold for commercial acceptability without an agreed operational error tolerance. Include a numeric illustrative example and explicitly label it hypothetical, not an observed prediction. Do not confuse per-record prediction error with total weekly orders.
6. Generate the document using Python and `python-docx`. Use left-to-right direction and left alignment for paragraphs, tables, and table-cell content. Use a readable font, 12–14 pt body text, 18–24 pt headings, suitable margins and line spacing, and page numbering in the footer. Center figures, size them within page margins, and place their English captions below them. If a generation script is used, keep it in the skill folder and make failures explicit.
7. Use Western numerals (0–9). Protect the company's identity and do not make causal claims from observed associations. Explain that order data do not directly measure inventory, food waste, or stockouts, and that week numbers are not calendar dates.
8. Run the report-generation script from the project root using the configured project environment:

   ```powershell
   .\.venv\Scripts\python.exe .github\skills\client-report-en\build_report.py
   ```

   Do not assume or add unsupported environment setup commands.
9. Verify that `reports/client_report_en.docx` exists, the number of embedded images equals the number of image files in `reports/figures/`, the RMSE value and its explanation are present, and paragraphs and tables have left-to-right formatting. Stop with a clear error if any check fails or a required result is unavailable; do not present a partial document as complete.

## Evidence and interpretation rules

- Preserve source data: never edit, overwrite, delete, or upload files from `data/raw/`. Read `train.csv` only to calculate the requested target statistics.
- Report only evidence-backed values. Keep units and the aggregation level clear, particularly the difference between a center-meal-week record and a weekly company total.
- Distinguish descriptive patterns and statistical associations from causal effects. Promotion and discount comparisons do not establish that a promotion or price change caused demand to change.
- Do not infer seasonality or calendar events from week numbers.
- Keep the company, customer, and individual identities confidential. Discuss aggregated operational patterns only.
- Explain material data and model limitations, including the lack of direct stock, waste, and stockout measures and the fact that validation error is not test-period error when actual test labels are unavailable.
