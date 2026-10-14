# Solution: Ingestion Functions

The pattern to follow is `load_excel()` - check the file exists, raise a clear error if it doesn't, then read it.

## load_csv

```python
def load_csv(filepath):
    filepath = Path(filepath)

    if not filepath.exists():
        raise FileNotFoundError(f"CSV file not found: {filepath}")

    return pd.read_csv(filepath)
```

## load_json

```python
def load_json(filepath):
    filepath = Path(filepath)

    if not filepath.exists():
        raise FileNotFoundError(f"JSON file not found: {filepath}")

    with open(filepath, 'r') as f:
        data = json.load(f)

    return pd.json_normalize(data)
```

## Commit

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/ingestion.py` to stage it
- Type a commit message: `Implement load_csv and load_json`
- Click **Commit**
- Click **Sync Changes** to push to GitHub
