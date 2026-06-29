# Activity: Complete and improve the Ingestion function

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

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/ingestion.py` to stage it
- Type a commit message: `Implement data ingestion functions`
- Click **Commit**
- Click **Sync Changes** to push to GitHub
