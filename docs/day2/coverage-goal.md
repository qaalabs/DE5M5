# Activity: Achieve high Test coverage

## Task 1: Test ingestion module

Create `tests/test_ingestion.py`:

```python
"""Tests for data ingestion functions."""

import pytest
import pandas as pd
from pathlib import Path
from src.data_processing.ingestion import load_csv, load_json, load_excel

# Test with actual sample files
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

# Add more tests...
```

## Task 2: Run coverage report

```powershell
# Full coverage report
pytest tests/ -v --cov=src.data_processing --cov-report=term-missing

# See which lines aren't covered
pytest tests/ --cov=src.data_processing --cov-report=html

# Open htmlcov/index.html in browser
```

## Task 3: Improve coverage

- Identify uncovered lines
- Write tests for those lines
- Focus on error handling and edge cases

## Task 4: Document and commit

```powershell
# Update README with coverage
echo "Test Coverage: 75%" >> README.md

git add tests/
git commit -m "Add comprehensive tests - 75% coverage achieved"
git push
```

Or make these changes in **Visual Studio Code** and **GitHub Desktop**
