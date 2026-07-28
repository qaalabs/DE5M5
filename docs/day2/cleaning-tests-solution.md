# Solution: Write Python Tests

## tests/test_cleaning.py

```python
import pytest
import pandas as pd
import pandas.testing as pdt
from data_processing.cleaning import (
    remove_duplicates,
    handle_missing_values,
    standardise_dates
)

@pytest.fixture
def sample_with_duplicates():
    return pd.DataFrame({
        'id': [1, 2, 2, 3],
        'name': ['Alice', 'Bob', 'Bob', 'Charlie']
    })

@pytest.fixture
def sample_with_missing():
    return pd.DataFrame({
        'id': [1, 2, 3],
        'name': ['Alice', None, 'Charlie'],
        'value': [10, None, 30]
    })

def test_remove_duplicates_reduces_rows(sample_with_duplicates):
    result = remove_duplicates(sample_with_duplicates, subset=['id'])
    assert len(result) == 3

def test_remove_duplicates_ids_are_unique(sample_with_duplicates):
    result = remove_duplicates(sample_with_duplicates, subset=['id'])
    assert result['id'].is_unique

def test_handle_missing_drop(sample_with_missing):
    result = handle_missing_values(sample_with_missing, strategy='drop')
    assert result.isnull().sum().sum() == 0

def test_handle_missing_fill(sample_with_missing):
    result = handle_missing_values(sample_with_missing, strategy='fill', fill_value=0)
    assert len(result) == 3

def test_standardise_dates():
    df = pd.DataFrame({'date': ['2024-01-01', '2024-06-15']})
    result = standardise_dates(df, date_columns=['date'])
    assert pd.api.types.is_datetime64_any_dtype(result['date'])
```

Walking through it:

- **`test_remove_duplicates_reduces_rows`** - the fixture has 4 rows with `id` 2 appearing twice. Deduplicating on `subset=['id']` keeps the first occurrence and drops the rest, so 3 rows remain.
- **`test_remove_duplicates_ids_are_unique`** - a row count check alone wouldn't catch a duplicate slipping through elsewhere. `Series.is_unique` confirms the actual deduplication key has no repeats.
- **`test_handle_missing_drop`** - `strategy='drop'` removes any row with a missing value, so nothing left should be null. `.isnull().sum()` gives a per-column count; the second `.sum()` collapses that to a single total.
- **`test_handle_missing_fill`** - unlike `'drop'`, `'fill'` keeps every row and replaces missing values with `fill_value` instead. The row count should stay at 3 (nothing removed).
- **`test_standardise_dates`** - the function's job is to convert a column of date strings into real `datetime64` values. `pd.api.types.is_datetime64_any_dtype` checks the dtype directly rather than trying to match formatted strings.

## tests/test_validation.py

```python
from data_processing.validation import validate_isbn

def test_valid_isbn():
    result = validate_isbn('9780306406157')
    assert result == '9780306406157'

def test_invalid_isbn():
    result = validate_isbn('not-an-isbn')
    assert result is None

def test_wrong_length():
    result = validate_isbn('123456789')
    assert result is None
```

Walking through it:

- **`test_valid_isbn`** - `9780306406157` is a real, check-digit-valid ISBN-13. A valid input should come back unchanged (no hyphens to strip here).
- **`test_invalid_isbn`** - `'not-an-isbn'` contains letters, so it fails the `isdigit()` check inside `validate_isbn` and the function should return `None`, not raise or return the input as-is.
- **`test_wrong_length`** - `'123456789'` is all digits but only 9 characters, not the required 13, so it's rejected on length before check-digit maths ever runs.

See [Solution: ISBN Validation](validation-solution.md) for the `validate_isbn` implementation these tests exercise.

## Run your tests

```
pytest --cov=src
```
