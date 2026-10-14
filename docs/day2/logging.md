# Activity: Add Logging to the Pipeline

Your loaders and cleaning functions work. Now make them say what they did.

When you run the pipeline yourself you can watch it and check the numbers. From tomorrow it runs without you - in GitHub Actions, then in Microsoft Fabric. A log is the record of what happened on a run nobody watched: how many rows came in, how many were removed, and what went wrong.

This activity has four checkpoints. Work through them at your own pace.

!!! note "At each checkpoint"
    1. Run the pipeline and compare the output with the expected lines
    2. Commit and push your work
    3. Post the checkpoint number in the chat, e.g. **1 done**
    4. Carry straight on to the next checkpoint - do not wait

Checkpoint 4 is a stretch task. Not everyone will reach it, and that is fine.

---

## How logging works

Two separate jobs, done in two different places:

| Job | Where | Code |
|---|---|---|
| **Write** log messages | Every module that has something to say | `logger = logging.getLogger(__name__)` then `logger.info(...)` |
| **Switch logging on** and choose what is shown | Once, in the program that runs everything | `logging.basicConfig(level=logging.INFO)` |

The level says how serious a message is:

| Level | Use it for |
|---|---|
| `logger.info(...)` | Normal progress - what was loaded, how many rows were removed |
| `logger.warning(...)` | It worked, but something looks wrong |
| `logger.error(...)` | It failed |

---

## Checkpoint 1 - Switch it on and log a load

### Switch logging on

Open `src/data_processing/run_pipeline.py`. Add `import logging` with the other imports at the top, then add this line underneath the imports:

```python
logging.basicConfig(level=logging.INFO)
```

### Log from `load_csv()`

Open `src/data_processing/ingestion.py`. If these two lines are not already there, add `import logging` with the other imports, and this underneath them:

```python
logger = logging.getLogger(__name__)
```

Now change `load_csv()` so that it logs the row count after a successful read, instead of returning the DataFrame straight away:

```python
df = pd.read_csv(filepath)
logger.info(f"Loaded {len(df)} rows from {filepath}")
return df
```

### Check it

In the VS Code terminal:

```
python -m data_processing.run_pipeline
```

!!! success "Near the top of the output you should see `INFO:data_processing.ingestion:Loaded 5100 rows from data/circulation_data.csv`."

Read that line from left to right: the level, the module it came from, then your message. You did not write the first two parts - `getLogger(__name__)` did.

### Commit and flag

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Stage `src/data_processing/run_pipeline.py` and `src/data_processing/ingestion.py`
- Type a commit message: `Add logging to load_csv`
- Click **Commit**
- Click **Sync Changes** to push to GitHub

Post **1 done** in the chat, then carry on.

---

## Checkpoint 2 - Log what the cleaning removed

A cleaning function that removes rows should say how many. Open `src/data_processing/cleaning.py` and add the same `import logging` and `logger = logging.getLogger(__name__)` lines at the top if they are not already there.

- In `remove_duplicates()`, log how many duplicate rows were removed.
- In `handle_missing_values()`, log how many rows were dropped when `strategy='drop'`.

To report a count you need the number of rows before and after:

```python
rows_before = len(df)
...
logger.info(f"Removed {rows_before - len(df)} duplicate rows")
```

### Check it

```
python -m data_processing.run_pipeline
```

!!! success "You should now also see `Removed 100 duplicate rows` and `Dropped 815 rows with missing values` for the circulation data, with more lines further down for the other datasets."

### Commit and flag

Commit `src/data_processing/cleaning.py` with the message `Log rows removed by cleaning` and **Sync Changes**.

Post **2 done** in the chat, then carry on.

---

## Checkpoint 3 - Log a failure

When a read fails, the error should reach the log before the pipeline stops. Wrap the read in `load_csv()`:

```python
try:
    df = pd.read_csv(filepath)
except Exception as e:
    logger.error(f"Could not load {filepath}: {e}")
    raise
```

The bare `raise` matters. It passes the original error on unchanged, so whoever called `load_csv()` still sees it. Logging an error is not the same as handling it.

Then do the same for `load_json()`, including an `info` line with the row count on success.

### Check it

Give `load_csv()` a file that is not a CSV. In your `sandbox` notebook, run this in a new cell:

```python
import logging
logging.basicConfig(level=logging.INFO, force=True)

from data_processing.ingestion import load_csv

load_csv('../data/catalogue.xlsx')
```

!!! success "The first line should be your `ERROR:data_processing.ingestion:Could not load ../data/catalogue.xlsx ...` message, followed by the usual traceback ending in `UnicodeDecodeError`."

Then run the pipeline again and confirm that `Loaded 500 rows from data/events_data.json` appears.

### Commit and flag

Commit `src/data_processing/ingestion.py` with the message `Log load failures` and **Sync Changes**.

Post **3 done** in the chat, then carry on.

---

## Checkpoint 4 - Stretch: log the data you lose

In the cleaning activity, `standardise_dates()` turned good dates into missing ones and said nothing. This is the kind of problem a log exists to catch.

### Your task

Change `standardise_dates()` so that, for each column, it counts how many values were missing before the conversion and how many are missing after. If the number has gone up, log a `warning` saying how many values could not be converted and in which column.

There is no model answer to copy this time.

### Check it

```
python -m data_processing.run_pipeline
```

!!! success "If your `standardise_dates()` is still the version from the demo, you should see a `WARNING` reporting 423 values in `checkout_date`. If you already fixed the date parsing in the cleaning stretch task, there should be no warning - which is your proof that the fix worked."

### Commit and flag

Commit `src/data_processing/cleaning.py` with the message `Warn when dates cannot be converted` and **Sync Changes**.

Post **4 done** in the chat.
