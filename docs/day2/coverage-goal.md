# Activity: Achieve Test Coverage

!!! abstract "S26: Identify data quality metrics and track them to ensure the quality, accuracy and reliability of the data product."

## Task 1: Test ingestion module

Create `tests/test_ingestion.py`:

```python
"""Tests for data ingestion functions."""

import pytest
import pandas as pd
from data_processing.ingestion import load_csv, load_json

def test_load_csv_success():
    """Test loading real CSV file."""
    df = load_csv('data/circulation_data.csv')
    
    assert len(df) > 0
    assert 'transaction_id' in df.columns

def test_load_csv_file_not_found():
    """Test error handling when file doesn't exist."""
    with pytest.raises(FileNotFoundError):
        load_csv('data/nonexistent.csv')

def test_load_json_success():
    """Test loading real JSON file."""
    df = load_json('data/events_data.json')
    
    assert len(df) > 0
    assert isinstance(df, pd.DataFrame)

# Add more tests using tmp_path to create test files on the fly:

def test_load_csv_empty_file(tmp_path):
    """Test that an empty CSV raises an error."""
    empty = tmp_path / "empty.csv"
    empty.write_text("")
    with pytest.raises(pd.errors.EmptyDataError):
        load_csv(str(empty))

def test_load_json_file_not_found():
    """Test error handling when JSON file doesn't exist."""
    with pytest.raises(FileNotFoundError):
        load_json('data/nonexistent.json')

def test_load_json_invalid(tmp_path):
    """Test that invalid JSON raises an error."""
    import json
    bad = tmp_path / "bad.json"
    bad.write_text("not valid json {{{")
    with pytest.raises(json.JSONDecodeError):
        load_json(str(bad))
```

`tmp_path` is a built-in pytest fixture - it creates a temporary directory for your test files automatically.

## Task 2: Run coverage report

```
python -m pytest --cov=src
```

To see which lines are not covered:

```
python -m pytest --cov=src --cov-report=html
```

Open `htmlcov/index.html` in a browser.

## Task 3: Strategies for improving coverage

Not all code is equally easy to test. Here are some strategies:

**Test the happy path first** - get the main flow working before edge cases.

**Test error cases** - if a function raises an error, test that it raises it:
```python
with pytest.raises(FileNotFoundError):
    load_csv('data/nonexistent.csv')
```

**Exclude code you didn't write** - the `load_excel` function was provided for you and has complex error handling that is hard to test. Tell coverage to ignore it by adding `# pragma: no cover` to the function definition:

```python
def load_excel(filepath, sheet_name=0, **kwargs):  # pragma: no cover
```

**Target: 60% is good, 70% is excellent** - don't chase 100%. A meaningful 60% is better than meaningless tests written just to hit a number.

## Task 4: Commit your work

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to the `tests/` folder to stage all test files
- Type a commit message: `Add tests - coverage improved`
- Click **Commit**
- Click **Sync Changes** to push to GitHub

