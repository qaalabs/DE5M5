# Activity: Write Your First Functions

## Part 1 - Explore the data

Create a new notebook at `notebooks/sandbox.ipynb` and load each raw file to see what you're working with before writing any production code.

```python
import pandas as pd, json

pd.read_csv("data/circulation_data.csv").head()
```

```python
with open("data/events_data.json") as f:
    data = json.load(f)
print(type(data), len(data))
```

```python
pd.read_excel("data/catalogue.xlsx", sheet_name=0).head()
```

Note the shape, column names, and any obvious issues.

## Part 2 - Implement the loaders

Open `src/data_processing/ingestion.py`. Two functions need work:

- `load_csv()` - runs but has no error handling or logging. Use `load_excel()` as your model and add the same pattern.
- `load_json()` - has a basic body but needs the flattening tested. Try `pd.json_normalize()` in your notebook first to see what it produces, then commit to the implementation.

`load_excel()` is already complete - read it, it shows the standard to follow.

## Part 3 - Test and commit

```bash
pytest tests/ -v
```

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/ingestion.py` to stage it
- Type a commit message: `Implement load_csv and load_json`
- Click **Commit**
- Click **Sync Changes** to push to GitHub
