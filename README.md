# Retail Transactions: End-to-End Data Quality & Exploratory Analysis

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/DebbieJara/everpeak-retail-data-quality-analysis/blob/main/retail_data_analysis.ipynb)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat&logo=pandas&logoColor=white)

**Business question:** Before any purchase behavior analysis can be trusted, the underlying data needs to be reliable. This project asks: how much can we trust this retail transactions dataset, and what does it take to make it analysis-ready?

## Context

The dataset contains 5,008 customer orders across multiple product categories, payment methods, and regions, including order values, product categories, payment methods, and demographic information.

## Process

I applied a full data quality and exploratory analysis workflow: auditing structure and missing values, investigating the pattern behind missingness with formal statistical testing rather than assuming it, building a reusable and documented cleaning pipeline to resolve three distinct hidden sentinel patterns, profiling statistics by product category, visualizing distributions, comparing two outlier detection methods, and engineering customer segments for downstream targeting.

## Key findings

| Topic | Finding |
|---|---|
| Hidden sentinels | Three sentinel patterns were disguised as valid data: `-999` in `customer_age` (25 rows), `"?"` in `product_category` (25 rows), and `0` in `quantity` (2,971 rows, 59.3% of the dataset) despite a positive `order_value` in every case, breaking the expected `order_value = price × quantity` relationship. The `quantity` values were reconstructed exactly via `order_value / price` rather than dropped or approximated |
| Missingness pattern | `city` and `state` shared the exact same 100 missing rows; a chi-square test confirmed no significant association with `payment_method` (p = 0.6336), consistent with Missing Completely At Random (MCAR) rather than MAR, so both were filled with `"unknown"` |
| Distribution shape | `order_value` (skew = 9.42) and `price` (skew = 9.63) are both strongly right-skewed. A simple mean-vs-median percentage check was misleading for `order_value`, showing only a 2.6% gap despite the strong skew, calculating skewness directly caught what the heuristic missed |
| Outlier detection | IQR flagged more records than Z-score on both skewed columns (`order_value`: 165 vs. 63; `price`: 450 vs. 78), consistent with Z-score's known tendency to underdetect outliers on skewed data |
| Customer segments | Four customer segments were created from age and purchase volume (e.g. Sr. High Volume, Jr. Low Volume), revealing a marginal `Jr. High Volume` segment (0.9%); a separate payment-behavior segmentation showed card payments dominant across both volume levels (72.6% of transactions) |

## Technical details

### Dataset

| Column | Type | Description |
|---|---|---|
| order_id | int | Unique order identifier |
| order_date | date | Purchase date |
| customer_id | int | Unique customer identifier |
| product_category | string | e.g. Fashion, Grocery, Sports |
| price | int | Unit price |
| quantity | int | Units purchased |
| order_value | int | Total transaction amount |
| payment_method | string | Payment method used |
| city | string | Customer city |
| state | string | Customer state |
| customer_age | float | Customer age |

### Analytical workflow

| Step | Description |
|---|---|
| 1. Data quality audit | Structure inspection, missing value counts, cardinality checks, hidden sentinel detection (numeric and categorical), date range validation |
| 2. Missing value analysis | Tested whether missingness patterns depended on other variables using chi-square hypothesis testing before choosing an imputation strategy |
| 3. Reusable cleaning pipeline | Modular, single-purpose, parameterized functions (sentinel replacement, numeric imputation, quantity reconstruction, text cleaning) orchestrated by one pipeline function |
| 4. Statistical profiling | Numeric and categorical summaries by product category; mean vs. median comparison and direct skewness calculation to detect distortion |
| 5. Distribution visualizations | Histograms and boxplots with Matplotlib and Seaborn, including before/after comparisons showing the impact of sentinel resolution |
| 6. Outlier detection | IQR and Z-score methods applied and compared |
| 7. Feature engineering | Customer segmentation using `np.where()` and `apply()` with custom classification functions |

### Key skills demonstrated

- Hidden data-quality issue detection: numeric and categorical sentinels invisible to standard null checks
- Missing data pattern classification (MCAR vs. MAR) validated with formal chi-square hypothesis testing
- Modular, reusable, parameterized data cleaning pipeline design
- Statistical profiling and skew detection, including recognizing when a common heuristic (mean vs. median) fails
- Outlier detection using multiple methods (IQR, Z-score)
- Feature engineering for customer segmentation

## Tools

Python · pandas · NumPy · Matplotlib · Seaborn

## Repository structure

```text
everpeak-retail-data-quality-analysis/
├── README.md
├── retail_data_analysis.ipynb
└── everpeak_retail.csv
```

---

By Deborah Jara | Business Intelligence · Data Analytics | Mexico
[LinkedIn](https://www.linkedin.com/in/deborahjara) · [GitHub](https://github.com/DebbieJara)
