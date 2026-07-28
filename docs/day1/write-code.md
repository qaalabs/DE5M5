# Activity: Write Your First Functions

## Part 1 - Explore the data

Create a new notebook at `notebooks/sandbox.ipynb` and load each raw file to see what you're working with before writing any production code.

```python
import pandas as pd, json

pd.read_csv("../data/circulation_data.csv").head()
```

```python
with open("../data/events_data.json") as f:
    data = json.load(f)
print("Number of records:",len(data))

data
```

```python
pd.read_excel("../data/catalogue.xlsx", sheet_name=0).head()
```

Note the shape, column names, and any obvious issues.


## Part 2 - Implement the loaders

Open `src/data_processing/ingestion.py`. Two functions need work: `load_csv()` and `load_json()`. Both need the same three ingredients:

1. **Check the file exists** before reading it - raise `FileNotFoundError` with a clear message if it doesn't.
2. **Wrap the read in try/except** - log the problem with `logger.error(...)` and re-raise it, so callers see it and it shows up in the logs.
3. **Log success** with `logger.info(...)`, including the row count.

`load_excel()` is already complete and shows this pattern in real code - if you get stuck on the shape of the try/except, it's there to check against. But CSV and JSON fail in their own specific ways, covered below.

### `load_csv()`

It currently just calls `pd.read_csv(filepath)` with no error handling. The specific things that go wrong with a CSV, and what pandas raises for each:

| Problem | Exception |
|---|---|
| File has no columns / is empty | `pd.errors.EmptyDataError` |
| Rows don't match up (inconsistent field count, broken quoting) | `pd.errors.ParserError` |
| File isn't valid UTF-8 (wrong encoding) | `UnicodeDecodeError` |

All three of these are actually subclasses of `ValueError`, so one `except ValueError` catches all of them:

```python
try:
    ...
except ValueError as e:
    logger.error(f"...: {e}")
    raise
except Exception as e:
    logger.error(f"...: {e}")
    raise
```

### `load_json()`

Two separate problems to handle here: the file might not be valid JSON, and even if it is, the structure might not "flatten" the way you expect.

- **Invalid JSON** - `json.load()` raises `json.JSONDecodeError` (also a `ValueError` subclass) if the file is malformed. Same `except ValueError` / `except Exception` shape as above works here too.
- **Flattening** - `json.load()` gives you a Python list/dict. If any of those dicts have nested objects (e.g. `{"book": {"title": ..., "isbn": ...}}`), you get nested dicts, not flat columns. `pd.json_normalize()` turns each nested key into its own column (e.g. `book.title`, `book.isbn`).
- Before writing the real implementation, load `events_data.json` in your `sandbox.ipynb` and run `pd.json_normalize(data)` on it. Look at the columns it produces - any nested fields you expected? Anything come out as a list instead of a single value? Once you've seen what it actually does, write the error-handled version in `load_json()`.


## Part 3 - Test and commit

```bash
pytest tests/ -v
```

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/ingestion.py` to stage it
- Type a commit message: `Implement load_csv and load_json`
- Click **Commit**
- Click **Sync Changes** to push to GitHub

