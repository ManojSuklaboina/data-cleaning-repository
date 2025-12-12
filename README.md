# San Francisco Building Permits — Cleaning & Structuring

**Project overview**

This repository contains a Python script that downloads the San Francisco Building Permits dataset from DataSF, performs robust cleaning and structuring, and writes out a cleaned CSV ready for analysis.

The cleaning script focuses on practical, interviewer-friendly tasks you can show during an internship demo: normalization of column names, parsing dates, extracting location coordinates, robust numeric and categorical cleaning, imputation with missingness flags, simple address parsing, and removal of low-information columns.

---

## Files

* `sf_building_permits_clean.py` — Main cleaning script (the code provided in the assistant conversation).
* `sf_building_permits_cleaned.csv` — Output cleaned dataset (generated after running the script).
* `sf_missing_report.csv` — Missingness report saved by the script.
* `README_SF_Building_Permits_Cleaning.md` — This README.

---

## Dataset source

The script downloads the dataset from DataSF (City of San Francisco Open Data). If your environment blocks external downloads, manually download the CSV from the DataSF portal and update `DATA_URL` in the script to point to the local file path.

---

## Requirements

Create a Python virtual environment and install the required packages:

```bash
python -m venv venv
source venv/bin/activate   # macOS / Linux
venv\Scripts\activate     # Windows PowerShell
pip install --upgrade pip
pip install pandas requests numpy scikit-learn
```

> Note: The script uses only `pandas`, `requests`, `numpy`. `scikit-learn` is optional and included if you plan to add modeling steps.

---

## How to run

1. Place the cleaning script (`sf_building_permits_clean.py`) in a working directory.
2. (Optional) Edit `DATA_URL` at the top of the script to change the source or point to a local CSV.
3. Run the script:

```bash
python sf_building_permits_clean.py
```

After successful run you should see two new files:

* `sf_building_permits_cleaned.csv` — cleaned dataset
* `sf_missing_report.csv` — missingness report (columns and % missing)

---

## What the script does (summary)

1. **Download & load:** Fetches the CSV from DataSF and reads it as strings for robust parsing.
2. **Normalize column names:** Lowercase, replace spaces/punctuation to underscores.
3. **Missingness report:** Writes a CSV with the percent missing for every column.
4. **Date parsing & features:** Detects date-like columns and creates year/month/dayofweek features; computes `days_to_issue` where applicable.
5. **Location extraction:** Attempts to extract latitude & longitude from `location` JSON-like fields.
6. **Numeric normalization:** Cleans currency/number text (removes `$`, `,`) and converts to numeric; adds missing flags.
7. **Categorical cleaning:** Standardizes text (trim, lowercase, remove noise), truncates long descriptions.
8. **Imputation:** Numeric → median; Categorical → mode. Creates `_missing_flag` columns to preserve missingness signals.
9. **Address parsing heuristics:** Extracts `zipcode`, `street_number`, and `street_name` from a candidate address column.
10. **Drop duplicates & sparse columns:** Drops exact duplicate rows and removes columns with >95% missing values.
11. **Type casting:** Casts small integer-like columns to nullable integer dtypes where safe.

---

## Output & deliverables to present

When demoing this project to an interviewer, show:

* The `sf_missing_report.csv` before and after cleaning to demonstrate the reduction/handling of missing values.
* A short before/after sample (first 5 rows) to show how messy columns become structured.
* A quick summary of engineered features (e.g., `days_to_issue`, `application_date_year`, `latitude`, `longitude`).
* The Python script with comments (explain each major step).

---

## Tips for interview-friendly extensions

* Add a small Jupyter notebook with EDA (missingness heatmap using `missingno`, histograms of numeric columns, map of permits using `geopandas`).
* Add unit tests (`pytest`) to check important invariants (no nulls in required columns, expected column names exist).
* Add a short `requirements.txt` and a `Makefile` or `run.sh` to simplify running the pipeline.
* Add a `data/` subfolder with a small sample of raw CSV (50–200 rows) to let reviewers run the pipeline without downloading the full dataset.

---

## Limitations & notes

* The script uses simple heuristics (regex-based address parsing, crude location extraction) — adequate for a demo but not production-grade.
* For higher fidelity spatial operations, use `geopandas` and a full geocoding pipeline.
* Advanced imputation (KNN or IterativeImputer) and text normalization (spell-correction) could improve quality for downstream ML models.

---

## License

This project is provided under the MIT License (feel free to adapt for your portfolio/demo).

---

If you want, I can now:

* Convert this README into a downloadable `README.md` file in the working directory, or
* Create a sample Jupyter notebook demonstrating the cleaned dataset and EDA.

Which would you like next, sir?
