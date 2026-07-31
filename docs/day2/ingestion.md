# Data Ingestion Module

## Learning Objectives

- Understand the check-exists / try-except / log pattern used across the pipeline
- See why raw data needs cleaning before it's usable

## Part 1: Walk through `load_excel()`

`load_excel()` in `src/data_processing/ingestion.py` is already complete - it's the pattern `load_csv()` and `load_json()` need to follow.

```python
def load_excel(filepath, sheet_name=0, **kwargs):
    filepath = Path(filepath)

    # Check file exists
    if not filepath.exists():
        logger.error(f"Excel File not found: {filepath}")
        raise FileNotFoundError(f"Excel File not found: {filepath}")

    try:
        logger.info(f"Loading Excel from {filepath} (sheet_name={sheet_name})")
        df = pd.read_excel(filepath, sheet_name=sheet_name, **kwargs)
        logger.info(f"Successfully loaded {len(df)} rows from {filepath}")
        return df

    except ValueError as e:
        logger.error(f"Value error loading Excel {filepath}: {e}")
        raise
    except ImportError as e:
        logger.error(f"Missing Excel engine for {filepath}: {e}")
        raise
    except Exception as e:
        logger.error(f"Error loading Excel {filepath}: {e}")
        raise
```

Three things happening, in order:

1. **Check the file exists** before trying to read it
2. **Wrap the read in try/except** - specific exceptions first, generic `Exception` last as a catch-all
3. **Log**, both on success (`logger.info`) and failure (`logger.error`)

## Part 2: Run the pipeline

```bash
python -m data_processing.run_pipeline
```

Watch the terminal output for **Processing Catalogue Data** - that's `load_excel()` running, and its `INFO` log lines are what you're seeing.

Open `report.txt`. Compare **Raw data** and **Cleaned data** for each dataset - the numbers are the same. `load_csv()` and `load_json()` are still TODO stubs, and `cleaning.py`'s functions don't clean anything yet either. Nothing is broken - this is the starting point the rest of the day builds on.

## Key Points

- Check exists, try/except, log - the pattern every load/clean/validate function in this pipeline follows
- `load_excel()` is done. `load_csv()` and `load_json()` are next, in the activity.
