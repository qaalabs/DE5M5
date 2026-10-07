# Activity: Write Your First Functions

!!! abstract "S16: Develop algorithms and processes to extract structured data from unstructured sources."

This activity has three checkpoints. Work through them at your own pace.

!!! note "At each checkpoint"
    1. Run the check and compare it with the expected output
    2. Commit and push your work
    3. Post the checkpoint number in the chat, e.g. **1 done**
    4. Carry straight on to the next checkpoint - do not wait

Checkpoint 3 is a stretch task. Not everyone will reach it, and that is fine.

Everything happens in `src/data_processing/ingestion.py`. `load_excel()` is already complete and is your model: if you get stuck on the shape of a function, check it against that one.

---

## Checkpoint 1 - `load_csv()`

### Look at the raw data

Before writing any code, open each file directly and see what you're working with:

- `data/circulation_data.csv` - open in VS Code and check the column headers and a few rows.
- `data/catalogue.xlsx` - open in Excel and check the column headers and sheet.
- `data/events_data.json` - open in VS Code and look at the structure. Is it a flat list, or are there nested objects?

Note anything that looks like it'll need cleaning.

### Implement it

`load_csv()` currently just calls `pd.read_csv(filepath)` with no error handling. It needs three ingredients:

1. **Check the file exists** before reading it - raise `FileNotFoundError` with a clear message if it doesn't.
2. **Wrap the read in try/except** - log the problem with `logger.error(...)` and re-raise it, so callers see it and it shows up in the logs.
3. **Log success** with `logger.info(...)`, including the row count.

The specific things that go wrong with a CSV, and what pandas raises for each:

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

### Check it

Load the real file:

```
python -c "from data_processing.ingestion import load_csv; df = load_csv('data/circulation_data.csv'); print(len(df))"
```

!!! success "You should see your own log line with the row count, something like `INFO:data_processing.ingestion:Successfully loaded 5100 rows ...`"

Now ask for a file that does not exist:

```
python -c "from data_processing.ingestion import load_csv; load_csv('data/nope.csv')"
```

!!! success "The last line should be a `FileNotFoundError` with **your** message. If it says `[Errno 2] No such file or directory`, the error is still coming from pandas, not from your check."

### Commit and flag

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/ingestion.py` to stage it
- Type a commit message: `Implement load_csv`
- Click **Commit**
- Click **Sync Changes** to push to GitHub

Post **1 done** in the chat, then carry on.

---

## Checkpoint 2 - `load_json()`

`load_json()` needs the same three ingredients as `load_csv()`. There are two separate problems to handle here: the file might not be valid JSON, and even if it is, the structure might not "flatten" the way you expect.

- **Invalid JSON** - `json.load()` raises `json.JSONDecodeError` (also a `ValueError` subclass) if the file is malformed. Same `except ValueError` / `except Exception` shape as above works here too.
- **Flattening** - `json.load()` gives you a Python list/dict. If any of those dicts have nested objects (e.g. `{"book": {"title": ..., "isbn": ...}}`), you get nested dicts, not flat columns. `pd.json_normalize()` turns each nested key into its own column (e.g. `book.title`, `book.isbn`).

### See the flattening for yourself

Before writing the real implementation, check this in the terminal:

```bash
python
```

```python
import pandas as pd, json
with open("data/events_data.json") as f:
    data = json.load(f)
pd.json_normalize(data)
```

Look at the columns it produces - any nested fields you expected? Anything come out as a list instead of a single value? Once you've seen what it actually does, `exit()` the REPL and write the error-handled version in `load_json()`.

### Check it

Load the real file:

```
python -c "from data_processing.ingestion import load_json; df = load_json('data/events_data.json'); print(len(df))"
```

!!! success "You should see your own log line with the row count, something like `INFO:data_processing.ingestion:Successfully loaded 500 rows ...`"

Now ask for a file that does not exist:

```
python -c "from data_processing.ingestion import load_json; load_json('data/nope.json')"
```

!!! success "The last line should be a `FileNotFoundError` with **your** message."

Finally, confirm nothing else is broken:

```
python -m pytest tests/ -v
```

!!! success "All tests should still pass."

### Commit and flag

Commit `src/data_processing/ingestion.py` with the message `Implement load_json` and **Sync Changes**.

Post **2 done** in the chat, then carry on.

---

## Checkpoint 3 - Stretch: `load_text()`

The library has four data sources, but `ingestion.py` only has three loaders. The fourth source, `data/feedback.txt`, is read directly inside the pipeline with a bare `open()` - no file check, no error handling, no logging.

Open `src/data_processing/run_pipeline.py` and find `process_feedback_data()` to see it.

### Your task

1. **Write `load_text(filepath)`** in `ingestion.py`. It returns the contents of the file as a single string, and has the same three ingredients as the other loaders. Decide for yourself what is worth logging on success - there are no rows in a text file.
2. **Use it in the pipeline.** In `run_pipeline.py`, import `load_text` and replace the bare `open()` in `process_feedback_data()` with a call to it.

There is no model answer to copy this time - `load_excel()` reads a different kind of file. Work out what can go wrong when reading text. Hint: the file is opened as UTF-8.

### Check it

```
python -m data_processing.run_pipeline
```

!!! success "The pipeline should still report `Found 200 feedback entries`, and your new log line for `data/feedback.txt` should appear alongside the other three loaders."

### Commit and flag

Commit both files with the message `Add load_text for feedback data` and **Sync Changes**.

Post **3 done** in the chat.
