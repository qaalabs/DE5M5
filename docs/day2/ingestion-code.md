# Activity: Write Your First Functions

!!! abstract "S16: Develop algorithms and processes to extract structured data from unstructured sources."

This activity has three checkpoints. Work through them at your own pace.

!!! note "At each checkpoint"
    1. Run the check in your notebook and compare it with the expected output
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

`load_csv()` currently just calls `pd.read_csv(filepath)` with no error handling. It needs the same two steps as `load_excel()`:

1. **Check the file exists** before reading it - raise `FileNotFoundError` with a clear message if it doesn't.
2. **Read the file** and return the DataFrame.

A missing file is not the only thing that can go wrong with a CSV. These errors come straight from pandas, and your function should let them through:

| Problem | Exception |
|---|---|
| File has no columns / is empty | `pd.errors.EmptyDataError` |
| Rows don't match up (inconsistent field count, broken quoting) | `pd.errors.ParserError` |
| File isn't valid UTF-8 (wrong encoding) | `UnicodeDecodeError` |

### Check it

The checks run in a Jupyter notebook, so you can see the data your function returns.

Run Jupyter Notebook in your Virtual Machine.

Find the `notebooks` folder inside your repo, create a new Python 3 notebook there and rename it `sandbox`. You will keep using this notebook for the rest of the day.

Put this in a cell and run it with **Shift + Enter**:

```python
%load_ext autoreload
%autoreload 2

from data_processing.ingestion import load_csv, load_json

df = load_csv('../data/circulation_data.csv')
df.head()
```

!!! success "You should see the first five rows of the circulation data. Run `len(df)` in a new cell - it should be `5100`."

!!! info "The two `%autoreload` lines mean the notebook picks up your changes each time you save `ingestion.py` - no need to restart anything. If a check still shows old behaviour, use **Kernel > Restart** and run the cells again."

!!! note "If you get `ModuleNotFoundError: No module named 'data_processing'`, add `import sys; sys.path.insert(0, '../src')` as the first line of the cell and run it again."

Now, in a new cell, ask for a file that does not exist:

```python
load_csv('../data/nope.csv')
```

!!! success "The last line should be a `FileNotFoundError` with **your** message. If it says `[Errno 2] No such file or directory`, the error is still coming from pandas, not from your check."

Finally, give it a file that exists but is not a CSV:

```python
load_csv('../data/catalogue.xlsx')
```

!!! success "The last line should be a `UnicodeDecodeError`. This one is from pandas, not from you - it is the third row of the table above."

### Commit and flag

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/ingestion.py` to stage it
- Type a commit message: `Implement load_csv`
- Click **Commit**
- Click **Sync Changes** to push to GitHub

Leave `notebooks/sandbox.ipynb` unstaged - it is your scratchpad, not part of the package.

Post **1 done** in the chat, then carry on.

---

## Checkpoint 2 - `load_json()`

`load_json()` needs the same file check as `load_csv()`. There are two more things to know about here: the file might not be valid JSON, and even if it is, the structure might not "flatten" the way you expect.

- **Invalid JSON** - `json.load()` raises `json.JSONDecodeError` if the file is malformed. Let it through, the same as the pandas errors above.
- **Flattening** - `json.load()` gives you a Python list/dict. If any of those dicts have nested objects (e.g. `{"book": {"title": ..., "isbn": ...}}`), you get nested dicts, not flat columns. `pd.json_normalize()` turns each nested key into its own column (e.g. `book.title`, `book.isbn`).

### See the flattening for yourself

Before writing the real implementation, run this in a new cell in your notebook:

```python
import pandas as pd, json
with open("../data/events_data.json") as f:
    data = json.load(f)
pd.json_normalize(data)
```

Look at the columns it produces - any nested fields you expected? Anything come out as a list instead of a single value? Once you've seen what it actually does, write the real version in `load_json()`.

### Check it

Load the real file in your notebook:

```python
df_events = load_json('../data/events_data.json')
df_events.head()
```

!!! success "You should see the first five rows, with the nested fields as their own columns. `len(df_events)` should be `500`."

Now ask for a file that does not exist:

```python
load_json('../data/nope.json')
```

!!! success "The last line should be a `FileNotFoundError` with **your** message."

Finally, confirm nothing else is broken. In the VS Code terminal:

```
python -m pytest tests/ -v
```

!!! success "All tests should still pass."

### Commit and flag

Commit `src/data_processing/ingestion.py` with the message `Implement load_json` and **Sync Changes**.

Post **2 done** in the chat, then carry on.

---

## Checkpoint 3 - Stretch: `load_text()`

The library has four data sources, but `ingestion.py` only has three loaders. The fourth source, `data/feedback.txt`, is read directly inside the pipeline with a bare `open()` - no file check and no clear error message.

Open `src/data_processing/run_pipeline.py` and find `process_feedback_data()` to see it.

### Your task

1. **Write `load_text(filepath)`** in `ingestion.py`. It returns the contents of the file as a single string, and checks the file exists in the same way as the other loaders.
2. **Use it in the pipeline.** In `run_pipeline.py`, import `load_text` and replace the bare `open()` in `process_feedback_data()` with a call to it.

There is no model answer to copy this time - `load_excel()` reads a different kind of file. Work out what can go wrong when reading text. Hint: the file is opened as UTF-8.

### Check it

In your notebook:

```python
from data_processing.ingestion import load_text

text = load_text('../data/feedback.txt')
text.count('Feedback #')
```

!!! success "You should see `200`. Then try `load_text('../data/nope.txt')` - the last line should be a `FileNotFoundError` with **your** message."

Then, in the VS Code terminal:

```
python -m data_processing.run_pipeline
```

!!! success "The pipeline should still report `Found 200 feedback entries`."

### Commit and flag

Commit both files with the message `Add load_text for feedback data` and **Sync Changes**.

Post **3 done** in the chat.
