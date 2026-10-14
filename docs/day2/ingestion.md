# Data Ingestion Module

## Learning Objectives

- Understand the check-first, fail-clearly pattern used by the loaders
- See why raw data needs cleaning before it's usable

## Part 1: Walk through `load_excel()`

`load_excel()` in `src/data_processing/ingestion.py` is already complete - it's the pattern `load_csv()` and `load_json()` need to follow.

```python
def load_excel(filepath, sheet_name=0, **kwargs):
    filepath = Path(filepath)

    # Check file exists
    if not filepath.exists():
        raise FileNotFoundError(f"Excel File not found: {filepath}")

    return pd.read_excel(filepath, sheet_name=sheet_name, **kwargs)
```

Two things happening, in order:

1. **Check the file exists** before trying to read it, and raise an error with a clear message if it doesn't
2. **Read the file** and return the DataFrame - if pandas cannot read it, its own error is allowed through

## Part 2: Run the pipeline

```bash
python -m data_processing.run_pipeline
```

Watch the terminal output for **Processing Catalogue Data** - that's `load_excel()` running.

Open `report.txt`. Compare **Raw data** and **Cleaned data** for each dataset - the numbers are the same. `load_csv()` and `load_json()` are still TODO stubs, and `cleaning.py`'s functions don't clean anything yet either. Nothing is broken - this is the starting point the rest of the day builds on.

## Key Points

- Check first, fail with a clear message, then do the work - the pattern every loader in this pipeline follows
- `load_excel()` is done. `load_csv()` and `load_json()` are next, in the activity.
