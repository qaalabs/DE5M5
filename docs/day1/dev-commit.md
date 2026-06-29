# Solution: Ingestion Functions

The pattern to follow is `load_excel()` - check the file exists, log what you're doing, wrap the read in a try/except, log success with row count.

## load_csv

```python
def load_csv(filepath):
    filepath = Path(filepath)

    if not filepath.exists():
        logger.error(f"CSV file not found: {filepath}")
        raise FileNotFoundError(f"CSV file not found: {filepath}")

    try:
        logger.info(f"Loading CSV from {filepath}")
        df = pd.read_csv(filepath)
        logger.info(f"Successfully loaded {len(df)} rows from {filepath}")
        return df
    except Exception as e:
        logger.error(f"Error loading CSV {filepath}: {e}")
        raise
```

## load_json

```python
def load_json(filepath):
    filepath = Path(filepath)

    if not filepath.exists():
        logger.error(f"JSON file not found: {filepath}")
        raise FileNotFoundError(f"JSON file not found: {filepath}")

    try:
        logger.info(f"Loading JSON from {filepath}")
        with open(filepath, 'r') as f:
            data = json.load(f)
        df = pd.json_normalize(data)
        logger.info(f"Successfully loaded {len(df)} rows from {filepath}")
        return df
    except Exception as e:
        logger.error(f"Error loading JSON {filepath}: {e}")
        raise
```

## Commit and push

Once the tests pass:

```bash
git add src/data_processing/ingestion.py
git commit -m "implement load_csv and load_json"
git push origin main
```

Then open a pull request on GitHub - base branch `main`, title something like `Implement ingestion loaders`.
