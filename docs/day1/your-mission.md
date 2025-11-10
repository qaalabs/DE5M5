# Your Mission

This project uses sample data from our fictional library network.

## Your Task

Build a data quality pipeline that:

- ✅ Ingests all data sources
- ✅ Cleans and validates the data
- ✅ Produces analysis-ready datasets
- ✅ Handles errors gracefully

## Files

- `circulation_data.csv` - Book circulation transactions (5,000 rows)
- `events_data.json` - Library events data (nested JSON)
- `feedback.txt` - Unstructured member feedback
- `catalogue.xlsx` - Book catalogue with metadata

## Data Quality Issues

These files intentionally contain data quality issues:

- Duplicate records
- Missing values
- Inconsistent date formats
- Nested structures requiring flattening

**Your task is to build a pipeline that cleans and validates this data.**

---

```python
# In Jupyter notebook 
import pandas as pd

# Show the messiness
df = pd.read_csv('data/circulation_data.csv')

print("First few rows:")
print(df.head())

print("\nData info:")
print(df.info())

print("\nCheck for issues:")
print(f"Duplicates: {df.duplicated().sum()}")
print(f"Missing ISBNs: {df['isbn'].isna().sum()}")
print(f"Date types: {df['checkout_date'].dtype}")

# Show the problems!
print("\nSample problematic rows:")
print(df[df['isbn'].isna()].head())
```

