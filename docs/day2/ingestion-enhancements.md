# Activity: Complete and improve the ingestion function

## Task 1: Complete `ingestion.py`

- Implement or improve `load_csv()` and `load_json()`
- Add `load_excel()` function for Excel files:

```python
def load_excel(filepath, sheet_name=0, **kwargs):
    """Load Excel file into DataFrame."""
    # TODO: Implement this
    pass
```

- Test each function in Jupyter notebook
- Verify they work with sample data

## Task 2: Commit Your Work

From the command line:

```bash
git add src/data_processing/ingestion.py
git commit -m "Implement data ingestion functions for CSV, JSON, and Excel"
git push origin main
```

Or commit using GitHub Desktop
