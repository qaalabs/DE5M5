# Activity: Complete and improve the Cleaning function

## Task 1: Build `cleaning.py`

Implement these functions:

1. `remove_duplicates()` (already shown)
2. `handle_missing_values()` (already shown)
3. `standardize_dates()` (already shown)
4. **Your own**: `validate_isbn()` - check if ISBN is valid format
5. **Your own**: `standardize_text()` - trim whitespace, lowercase, etc.

## Task 2: Test in Jupyter

Add another cell to the notebook you used earlier:

```python
from data_processing.cleaning import (
    remove_duplicates,
    handle_missing_values,
    standardize_dates
)

# Load data
df = load_csv('../data/circulation_data.csv')

# Apply cleaning pipeline
df_clean = remove_duplicates(df, subset=['transaction_id'])
df_clean = handle_missing_values(df_clean, strategy='drop')
df_clean = standardize_dates(df_clean, ['checkout_date', 'return_date'])

# Check results
print(f"Original rows: {len(df)}")
print(f"Clean rows: {len(df_clean)}")
print(df_clean.info())
```

## Task 2: Commit Your Work

From the command line:

```bash
git add src/data_processing/cleaning.py
git commit -m "Implement data cleaning functions"
git push
```

Or commit using GitHub Desktop
