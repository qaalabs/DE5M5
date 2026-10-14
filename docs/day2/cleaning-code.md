# Activity: Complete and improve the Cleaning functions

!!! abstract "S9: Query and manipulate data using tools and programming such as Python. Implement automated validation checks."

This activity has four checkpoints. Work through them at your own pace.

!!! note "At each checkpoint"
    1. Run the check in your notebook and compare it with the expected output
    2. Commit and push your work
    3. Post the checkpoint number in the chat, e.g. **1 done**
    4. Carry straight on to the next checkpoint - do not wait

Checkpoints 1 to 3 are the core task: get the three functions from the demo working and prove it on real data. Checkpoint 4 is a stretch task. Not everyone will reach it, and that is fine.

The functions live in `src/data_processing/cleaning.py`. The checks run in a Jupyter notebook, so you can see the data before and after each function.

---

## Set up your notebook

Run Jupyter Notebook in your Virtual Machine.

Find the `notebooks` folder inside your repo. Open `sandbox.ipynb` if you already have one, otherwise create a new Python 3 notebook there and rename it `sandbox`.

Put this in a cell and run it with **Shift + Enter**:

```python
%load_ext autoreload
%autoreload 2

from data_processing.ingestion import load_csv
from data_processing.cleaning import (
    remove_duplicates,
    handle_missing_values,
    standardise_dates,
)

df = load_csv('../data/circulation_data.csv')
len(df)
```

!!! success "You should see `5100`."

!!! info "The two `%autoreload` lines mean the notebook picks up your changes each time you save `cleaning.py` - no need to restart anything. If a check still shows old behaviour, use **Kernel > Restart** and run the cells again."

!!! note "If you get `ModuleNotFoundError: No module named 'data_processing'`, add `import sys; sys.path.insert(0, '../src')` as the first line of the cell and run it again."

---

## Checkpoint 1 - `remove_duplicates()`

Add `remove_duplicates()` to `cleaning.py` - it was shown in the demo. Uncomment `import pandas as pd` at the top of the file while you are there.

### Check it

```python
print(len(remove_duplicates(df)))
print(len(remove_duplicates(df, subset=['transaction_id'])))
print(len(df))
```

!!! success "You should see `5024`, then `5000`, then `5100`."

- The last number proves the key principle: `df` itself has not changed, because your function worked on a copy.
- The first two numbers are different. Why does `subset=['transaction_id']` remove 24 more rows? Run this and compare the pairs of rows:

```python
df[df.duplicated(subset=['transaction_id'], keep=False)].sort_values('transaction_id').head(10)
```

Keep the properly de-duplicated data for the next checkpoints:

```python
deduped = remove_duplicates(df, subset=['transaction_id'])
```

### Commit and flag

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/cleaning.py` to stage it
- Type a commit message: `Implement remove_duplicates`
- Click **Commit**
- Click **Sync Changes** to push to GitHub

Leave `notebooks/sandbox.ipynb` unstaged - it is your scratchpad, not part of the package.

Post **1 done** in the chat, then carry on.

---

## Checkpoint 2 - `handle_missing_values()`

Add `handle_missing_values()` to `cleaning.py`.

### Check it

First see what is missing:

```python
deduped.isna().sum()
```

!!! success "You should see `member_id 102`, `isbn 258` and `return_date 501`. The other columns are `0`."

Now try each route through the function:

```python
print(len(handle_missing_values(deduped, strategy='drop')))

filled = handle_missing_values(deduped, strategy='fill', fill_value='Unknown', columns=['member_id'])
print(len(filled), filled['member_id'].isna().sum())
```

!!! success "You should see `4185`, then `5000 0`."

Finally, ask for a strategy that does not exist:

```python
handle_missing_values(deduped, strategy='oops')
```

!!! success "The last line should be `ValueError: Unknown strategy: oops`."

!!! question "Think about it"
    `strategy='drop'` threw away 815 loans. 501 of them had no `return_date`. What does a missing return date mean in a library? Was dropping them the right call?

### Commit and flag

Commit `src/data_processing/cleaning.py` with the message `Implement handle_missing_values` and **Sync Changes**.

Post **2 done** in the chat, then carry on.

---

## Checkpoint 3 - `standardise_dates()`

Add `standardise_dates()` to `cleaning.py`.

### Check it

```python
dated = standardise_dates(deduped, ['checkout_date', 'return_date'])
dated.dtypes
```

!!! success "`checkout_date` and `return_date` should both now be `datetime64`, not text."

Before you trust it, count the missing dates again:

```python
print(deduped['checkout_date'].isna().sum())
print(dated['checkout_date'].isna().sum())
```

!!! warning "Every loan had a checkout date going in. How many have one coming out?"

Look at what those rows looked like before the function touched them:

```python
deduped[dated['checkout_date'].isna()].head()
```

The function ran without an error and still lost data. Post what you think went wrong in the chat along with your **3 done**.

### Commit and flag

Commit `src/data_processing/cleaning.py` with the message `Implement standardise_dates` and **Sync Changes**.

Post **3 done** in the chat, then carry on.

---

## Checkpoint 4 - Stretch: stop losing dates

`standardise_dates()` is quietly turning good dates into missing ones. Fix it.

### Your task

1. **Find the cause.** Look at the raw `checkout_date` values that were lost. What is different about them? What does `errors='coerce'` do with a value it cannot read?
2. **Fix `standardise_dates()`** so those dates are converted instead of thrown away.

There is no model answer to copy this time. Hint: look at the `format` argument in the `pd.to_datetime()` documentation.

### Check it

```python
dated = standardise_dates(deduped, ['checkout_date', 'return_date'])
print(dated['checkout_date'].isna().sum())
print(dated['return_date'].isna().sum())
```

!!! success "You should see `0`, then `501` - no checkout dates lost, and the return dates that were missing to begin with are still missing."

### Commit and flag

Commit `src/data_processing/cleaning.py` with the message `Fix date parsing in standardise_dates` and **Sync Changes**.

Post **4 done** in the chat.

### Still have time?

Run `deduped['branch_id'].nunique()`. The library has 15 branches - how many does the data say there are? Find out why, then write a new cleaning function that fixes it, following the same pattern as the others: copy, transform, return.
