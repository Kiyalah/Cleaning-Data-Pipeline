# README.md

# Generic Data Cleaning Pipeline v2

A complete and educational Python pipeline for cleaning datasets before performing Exploratory Data Analysis (EDA), statistical analysis, Machine Learning, or dashboarding.

Designed for **Data Analysts**, **Data Scientists**, **Data Engineers**, students, and anyone who wants a reusable and explainable data-cleaning workflow.

---

## Features

This pipeline automates the most common data-cleaning tasks:

✅ Dataset inspection

✅ Duplicate detection and removal

✅ Missing value handling

✅ Constant column removal

✅ Outlier detection (IQR method)

✅ Numeric string cleaning

✅ Automatic type detection

✅ Boolean detection

✅ Human validation checkpoint

✅ Data type conversion

✅ Memory optimization

✅ Final quality validation

---

## Cleaning Workflow

```text
RAW DATASET
      │
      ▼
1. inspect_dataframe()
      │
      ▼
2. handle_duplicates()
      │
      ▼
3. handle_missing_values()
      │
      ▼
4. drop_constant_columns()
      │
      ▼
5. detect_outliers_iqr()
      │
      ▼
6. auto_clean_dataframe()
      │
      ▼
7. optimize_numeric_types()
      │
      ▼
8. final_validation()
      │
      ▼
 CLEAN DATASET
```

---

## Installation

### Clone or download the project

```bash
git clone https://github.com/Kiyalah/Cleaning-Data-Pipeline.git
cd data-cleaning-pipeline
```

### Install dependencies

```bash
pip install pandas numpy scikit-learn
```

---

## Dependencies

| Package      | Purpose                  |
| ------------ | ------------------------ |
| pandas       | Data manipulation        |
| numpy        | Numerical operations     |
| scikit-learn | Missing value imputation |

---

## Quick Start

### Import the pipeline

```python
import pandas as pd

from data_cleaning_pipeline_v2 import (
    clean_dataset,
    clean_numeric_strings
)
```

### Load your dataset

```python
df = pd.read_csv("data.csv")
```

### Optional: Clean currency or percentage columns

```python
df = clean_numeric_strings(
    df,
    columns=["price", "revenue", "profit_margin"]
)
```

### Run the full pipeline

```python
df_clean = clean_dataset(df)
```

### Check the result

```python
print(df_clean.head())
print(df_clean.dtypes)
```

---

# Pipeline Components

## 1. Dataset Inspection

Provides:

* Number of rows
* Number of columns
* Data types
* Missing values report
* Memory usage
* Dataset preview

```python
inspect_dataframe(df)
```

---

## 2. Duplicate Handling

Detects and removes duplicated rows.

```python
df = handle_duplicates(df)
```

Functions available:

```python
inspect_duplicates(df)

show_duplicates(df)

remove_duplicates(df)
```

---

## 3. Missing Values Handling

Strategy:

| Missing Rate | Action             |
| ------------ | ------------------ |
| > 50%        | Drop column        |
| 5% – 50%     | Impute             |
| ≤ 5%         | Drop affected rows |

### Imputation Methods

| Data Type   | Strategy            |
| ----------- | ------------------- |
| Numeric     | Median              |
| Categorical | Most Frequent Value |

```python
df = handle_missing_values(df)
```

---

## 4. Constant Column Removal

Removes columns containing only one unique value.

Example:

```text
country
-------
France
France
France
France
```

This column provides no analytical value and is removed.

```python
df = drop_constant_columns(df)
```

---

## 5. Outlier Detection

Uses the IQR (Interquartile Range) method.

Formula:

```text
IQR = Q3 - Q1

Lower Bound = Q1 - 1.5 × IQR

Upper Bound = Q3 + 1.5 × IQR
```

Outliers are reported but never removed automatically.

```python
report = detect_outliers_iqr(df)
```

This allows analysts to make business-informed decisions.

---

## 6. Numeric String Cleaning

Converts formatted text values into numeric values.

Supported examples:

```text
"$1,500"     → 1500

"€2 000"     → 2000

"30%"        → 30

"1.500,00"   → 1500
```

Usage:

```python
df = clean_numeric_strings(
    df,
    columns=["price", "salary"]
)
```

---

## 7. Automatic Type Detection

Detects:

### Numeric Columns

```python
numeric_cols = detect_numeric_columns(df)
```

### Date Columns

```python
date_cols = detect_date_columns(df)
```

### Category Columns

```python
category_cols = detect_category_columns(df)
```

### Boolean Columns

```python
boolean_cols = detect_boolean_columns(df)
```

Supported boolean formats:

```text
True / False

Yes / No

Y / N

1 / 0
```

---

## 8. Human Validation Checkpoint

Before converting data types, the pipeline pauses and displays suggested column classifications.

Example:

```text
Suggested NUMERIC columns:
['salary', 'age']

Suggested DATE columns:
['created_at']

Suggested CATEGORY columns:
['country']

Suggested BOOLEAN columns:
['is_active']
```

This step prevents common mistakes such as:

```text
zipcode
customer_id
phone_number
product_code
```

being incorrectly converted into numerical variables.

---

## 9. Type Conversion

Converts validated columns into appropriate types:

### Numeric

```python
convert_numeric_columns()
```

### Date

```python
convert_date_columns()
```

### Category

```python
convert_category_columns()
```

### Boolean

```python
convert_boolean_columns()
```

---

## 10. Memory Optimization

Reduces RAM consumption through numeric downcasting.

Examples:

| Before  | After            |
| ------- | ---------------- |
| int64   | int8/int16/int32 |
| float64 | float32          |

Usage:

```python
df = optimize_numeric_types(df)
```

Useful for large datasets and machine-learning pipelines.

---

## 11. Final Validation

Performs a final quality check:

* Remaining missing values
* Remaining duplicates
* Final data types
* Memory usage
* Dataset dimensions

```python
final_validation(df)
```

---

# Full Example

```python
import pandas as pd

from data_cleaning_pipeline_v2 import (
    clean_dataset,
    clean_numeric_strings
)

# Load dataset
df = pd.read_csv("sales_data.csv")

# Clean currency columns
df = clean_numeric_strings(
    df,
    columns=["price", "revenue"]
)

# Run pipeline
df_clean = clean_dataset(df)

# Save cleaned dataset
df_clean.to_csv(
    "sales_data_clean.csv",
    index=False
)
```

---

# Recommended Next Steps

After cleaning your data, you can proceed to:

## Exploratory Data Analysis (EDA)

```python
df_clean.describe()

df_clean.info()
```

## Visualization

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.histplot(df_clean["salary"])

sns.boxplot(x=df_clean["salary"])

sns.heatmap(df_clean.corr())
```

## Machine Learning

```python
from sklearn.model_selection import train_test_split

X = df_clean.drop("target", axis=1)

y = df_clean["target"]
```

## Dashboarding

```python
df_clean.to_csv("clean_data.csv")

df_clean.to_excel("clean_data.xlsx")
```

Use with:

* Power BI
* Tableau
* Streamlit
* Dash

---

# Target Audience

This project is suitable for:

* Data Analysts
* Data Scientists
* Data Engineers
* BI Developers
* Students learning Data Analytics
* Machine Learning practitioners

---

# Version

**Version:** 2.0

**Python:** 3.9+

**License:** MIT

---

## Author

**Fahim Coulibaly**

AI & ML Engineer

Portfolio: https://carte-virtuelle-fahim.vercel.app/

LinkedIn: https://www.linkedin.com/in/fahim-coulibaly-28638a1ba/

Email: fahimkiyalah@gmail.com


