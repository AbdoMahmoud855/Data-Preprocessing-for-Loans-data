# Loan Data Preprocessing & Exploratory Analysis

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-data%20analysis-150458?logo=pandas&logoColor=white)
![Status](https://img.shields.io/badge/status-active-brightgreen)

A reproducible Jupyter notebook for cleaning, validating, and exploring a retail loans dataset. It standardizes data types, audits data quality, surfaces patterns across loan categories, and produces an automated profiling report to support downstream modeling and reporting.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Data Dictionary](#data-dictionary)
- [Results](#results)
- [Methodology](#methodology)
- [Outputs](#outputs)
- [Limitations & Next Steps](#limitations--next-steps)
- [Tech Stack](#tech-stack)
- [License](#license)

---

## Overview

Raw loan records are rarely analysis-ready. This project establishes a clear preprocessing workflow that:

1. Loads and inspects the raw dataset.
2. Enforces correct data types for identifiers, categorical flags, and dates.
3. Audits data quality (missing values, outliers, distributions).
4. Explores relationships between loan attributes through aggregation and correlation analysis.
5. Generates a shareable HTML profile of the cleaned data.

## Key Features

- **Type standardization** – identifiers, categorical variables, and date fields are cast to appropriate dtypes.
- **Data quality audit** – missing-value counts and summary statistics for numerical and categorical fields.
- **Outlier detection** – boxplots for `loan_amount` and `rate`.
- **Segment analysis** – average loan amount by `loan_type` and filtering of above-average loans.
- **Correlation analysis** – correlation matrix across numeric features.
- **Automated profiling** – full HTML report generated with `ydata-profiling`.

## Repository Structure

```
.
├── Data_Preprocessing_for_Loans_data.ipynb   # Main analysis notebook
├── loans.csv                                 # Input dataset (user-supplied)
├── loan_data_report.html                     # Generated profiling report
├── assets/                                   # Figures used in this README
└── README.md
```

> `loans.csv` is not distributed with this repository. Place your copy in the project root before running the notebook.

## Getting Started

### Prerequisites

- Python 3.8 or later
- Jupyter Notebook or JupyterLab

### Installation

```bash
# (Recommended) create an isolated environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# Install dependencies
pip install numpy pandas matplotlib seaborn ydata-profiling jupyter
```

### Running the Notebook

```bash
jupyter notebook Data_Preprocessing_for_Loans_data.ipynb
```

Then select **Kernel → Restart & Run All**.

## Data Dictionary

Sample of the raw data:

![Sample of the loans dataset](assets/data_preview.png)

| Column        | Description                         | Target Type      |
| ------------- | ----------------------------------- | ---------------- |
| `client_id`   | Unique client identifier            | numeric (ID)     |
| `loan_id`     | Unique loan identifier              | `object`         |
| `loan_type`   | Loan category                       | `object`         |
| `loan_amount` | Principal amount borrowed           | numeric          |
| `rate`        | Interest rate                       | numeric          |
| `repaid`      | Repayment status (`1` = repaid, `0` = not repaid) | `category` |
| `loan_start`  | Loan start date (`YYYY-MM-DD`)      | `datetime64[ns]` |
| `loan_end`    | Loan end date (`YYYY-MM-DD`)        | `datetime64[ns]` |

## Methodology

| Step | Stage                    | Description                                                              |
| ---- | ------------------------ | ------------------------------------------------------------------------ |
| 1    | Setup                    | Import numpy, pandas, matplotlib, and seaborn.                           |
| 2    | Ingestion & inspection   | Load `loans.csv`; review `head()`, `shape`, and `info()`.                |
| 3    | Initial filtering        | Isolate loans above the mean `loan_amount`.                              |
| 4    | Type conversion          | Cast IDs, categorical flags, and dates to correct dtypes.                |
| 5    | Summary statistics       | Describe numerical and categorical features.                             |
| 6    | Missing-value audit      | Count nulls per column with `isna().sum()`.                              |
| 7    | Trend-focused filtering  | Review high-value loans.                                                 |
| 8    | Grouping                 | Compute mean `loan_amount` by `loan_type`.                               |
| 9    | Outlier detection        | Visualize `loan_amount` and `rate` with boxplots.                        |
| 10   | Correlation & distribution | Build correlation matrix; plot `loan_amount` histogram.                |
| 11   | Profiling                | Export an interactive HTML report with `ydata-profiling`.                |

## Results

### Loan amount distribution

![Boxplot of loan_amount] <img width="665" height="452" alt="image" src="https://github.com/user-attachments/assets/e94451bf-93e7-4e6d-a7c6-0455e89a545b" />


Loan amounts range from roughly 600 to 15,000, with a median of about 8,300 and an interquartile range of approximately 4,200 to 11,700. No outliers are flagged, so no treatment is needed for this variable.

### Interest rate distribution

![Boxplot of rate](<img width="612" height="447" alt="image" src="https://github.com/user-attachments/assets/8192b501-6ada-45ef-aa37-75ad1aba313b" />
)

Interest rates are right-skewed, with a median near 2.8 and an interquartile range of roughly 1.2 to 4.8. Three points above the upper whisker (about 10.5, 10.9, and 12.6) are flagged as outliers and should be reviewed before modeling.

### Correlation analysis

![Correlation heatmap](<img width="861" height="583" alt="image" src="https://github.com/user-attachments/assets/07e9f70e-a4a5-4dad-be96-c20db12e4a7f" />
)

All pairwise correlations are close to zero (`loan_amount` vs `rate` = -0.033), indicating no meaningful linear relationship between the numeric variables. Correlations with `client_id` carry no analytical meaning because it is an identifier and should be excluded from numeric correlation analysis.

## Outputs

- In-notebook summaries, tables, and visualizations (boxplots, correlation heatmap, histogram)
- `loan_data_report.html` – standalone profiling report viewable in any browser

## Limitations & Next Steps

- Outliers (three high `rate` values) and missing values are currently **identified but not treated**. Recommended extensions include IQR-based capping or winsorization and median/mode imputation.
- `client_id` is included in the correlation matrix; exclude identifier columns from correlation analysis.
- Derived features (e.g., loan duration from `loan_start` and `loan_end`) are not yet engineered.
- Add automated data-validation checks (e.g., Great Expectations or pandera) for production use.
- Consider a modeling stage for repayment prediction on the cleaned dataset.

## Tech Stack

| Library            | Purpose                       |
| ------------------ | ----------------------------- |
| `pandas`           | Data manipulation             |
| `numpy`            | Numerical operations          |
| `matplotlib`       | Visualization                 |
| `seaborn`          | Statistical plotting          |
| `ydata-profiling`  | Automated data profiling      |

## License

Specify a license for this project (e.g., MIT) by adding a `LICENSE` file to the repository.
