---
description: "Use when working with raw datasets, cleaning, joins, filtering, or analysis notebooks. Protect source files, use derived copies, and report row counts after joins or drops."
---
# Rule 1: Protect the source data

- Keep the raw dataset in the `data/` folder as the original source of truth.
- Do not edit, overwrite, delete, or upload the raw data files to GitHub.
- Use copies or derived files for cleaning and analysis work.
- Print the number of rows after either join or drop processes.
- If a file must be changed, keep the original version untouched and preserve a clear record of what was modified.
- Add a clarifying comment within each step.
