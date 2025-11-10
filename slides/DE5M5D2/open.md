### *title*

The aim of day 2 is ...

https://qaalabs.github.io/DE5M5/trainer

---

## **Module 5: Day 2 - Build & Test Python Package**

**Theme**: Writing production-quality, testable Python code locally on VM

---

### **9:30-9:50 | Welcome & Day Overview (20 mins)**
**Format**: Trainer-led (WebEx screen share)

**Content**:
- Welcome back! Quick check-in
- Day 1 recap: What we accomplished
  - ✅ Architecture designed
  - ✅ GitHub repo set up
  - ✅ Kanban board created
  - ✅ First function written
- Today's goals:
  - Build complete data processing package
  - Write comprehensive tests
  - Achieve 70%+ test coverage
  - Everything working locally on VM
- **Key philosophy**: "Build it right, not just make it work"

**Quick Standup** (10 mins):
Each learner (or pair) briefly shares:
- What did you complete yesterday?
- What are you working on today?
- Any blockers?

**Artifacts Needed**:
- 📊 Slide deck: Day 2 overview
- 📊 Slide: Day 1 recap with accomplishments

---

### **Session 1: 9:50-10:40 | Data Ingestion Module (50 mins)**
**Format**: Trainer demo (20 mins) → Learner work (30 mins)

**Learning Objectives**:
- Build functions to load CSV, JSON, and Excel files
- Implement proper error handling and logging
- Write clean, documented code
- Test functions locally in Jupyter

**Trainer Demo** (20 mins):

**Part 1: Principles** (5 mins)
> "We're building a **Python package**, not just scripts. This means:
> - Functions do ONE thing well
> - Docstrings for every function
> - Error handling (what if file doesn't exist?)
> - Logging (so we can debug later)
> - Pure functions (don't modify inputs)"

**Part 2: Live Coding** (15 mins)

Open `src/data_processing/ingestion.py` and code:

```python
"""Data ingestion functions for library pipeline.

This module handles loading data from various file formats.
"""

import pandas as pd
import json
import logging
from pathlib import Path

# Set up logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def load_csv(filepath, **kwargs):
    """Load CSV file into DataFrame.
    
    Args:
        filepath (str): Path to CSV file
        **kwargs: Additional arguments for pd.read_csv()
        
    Returns:
        pd.DataFrame: Loaded data
        
    Raises:
        FileNotFoundError: If file doesn't exist
        pd.errors.EmptyDataError: If file is empty
        
    Example:
        >>> df = load_csv('data/circulation.csv')
        >>> print(len(df))
    """
    filepath = Path(filepath)
    
    # Check file exists
    if not filepath.exists():
        logger.error(f"File not found: {filepath}")
        raise FileNotFoundError(f"File not found: {filepath}")
    
    try:
        logger.info(f"Loading CSV from {filepath}")
        df = pd.read_csv(filepath, **kwargs)
        logger.info(f"Successfully loaded {len(df)} rows from {filepath}")
        return df
        
    except pd.errors.EmptyDataError:
        logger.error(f"CSV file is empty: {filepath}")
        raise
    except Exception as e:
        logger.error(f"Error loading CSV {filepath}: {e}")
        raise

def load_json(filepath):
    """Load JSON file and flatten to DataFrame.
    
    Args:
        filepath (str): Path to JSON file
        
    Returns:
        pd.DataFrame: Flattened data
        
    Raises:
        FileNotFoundError: If file doesn't exist
        json.JSONDecodeError: If file is not valid JSON
        
    Example:
        >>> df = load_json('data/events.json')
    """
    filepath = Path(filepath)
    
    if not filepath.exists():
        logger.error(f"File not found: {filepath}")
        raise FileNotFoundError(f"File not found: {filepath}")
    
    try:
        logger.info(f"Loading JSON from {filepath}")
        with open(filepath, 'r') as f:
            data = json.load(f)
        
        # Flatten nested structure
        # Adjust based on your JSON structure
        if isinstance(data, dict) and 'events' in data:
            df = pd.json_normalize(data['events'])
        else:
            df = pd.json_normalize(data)
            
        logger.info(f"Successfully loaded {len(df)} records from {filepath}")
        return df
        
    except json.JSONDecodeError as e:
        logger.error(f"Invalid JSON in {filepath}: {e}")
        raise
    except Exception as e:
        logger.error(f"Error loading JSON {filepath}: {e}")
        raise
```

**Key Points to Emphasize**:
- ✅ Type hints in docstrings
- ✅ Error handling with try/except
- ✅ Logging at INFO and ERROR levels
- ✅ Raising exceptions (don't hide errors!)
- ✅ Docstring format (Google style)

**Part 3: Test in Jupyter** (5 mins)
```python
# In Jupyter notebook
import sys
sys.path.append('/home/user/projects/library-pipeline/src')

from data_processing.ingestion import load_csv, load_json

# Test CSV loading
df_circ = load_csv('../data/circulation_data.csv')
print(f"Loaded {len(df_circ)} circulation records")
print(df_circ.head())

# Test JSON loading
df_events = load_json('../data/events_data.json')
print(f"Loaded {len(df_events)} events")
print(df_events.head())
```

---

**Learner Activity** (30 mins):

**Task 1: Complete ingestion.py** (20 mins)
- Implement or improve `load_csv()` and `load_json()`
- Add `load_excel()` function for Excel files:
  ```python
  def load_excel(filepath, sheet_name=0, **kwargs):
      """Load Excel file into DataFrame."""
      # TODO: Implement this
      pass
  ```
- Test each function in Jupyter notebook
- Verify they work with sample data

**Task 2: Commit Your Work** (10 mins)
```bash
git add src/data_processing/ingestion.py
git commit -m "Implement data ingestion functions for CSV, JSON, and Excel"
git push origin main
```

**Trainer Circulates**:
- Check code quality (readable? documented?)
- Help with file path issues
- Verify logging is working
- Check error handling

**Q&A Checkpoint**:
> - "Can you load all three file types?"
> - "Show me your logging output"
> - "What happens if file doesn't exist?"
> - "Have you committed to GitHub?"
> - "Have you updated your Kanban?"

**Artifacts Needed**:
- 📊 Slides: Principles of good Python functions
- 📊 Slides: Error handling patterns
- 💾 Completed `ingestion.py` (reference implementation)
- 📄 Code quality checklist

---

### **10:40-11:00 | Break (20 mins)**

---

### **Session 2: 11:00-12:15 | Data Cleaning Module (75 mins)**
**Format**: Trainer demo (15 mins) → Learner work (60 mins)

**Learning Objectives**:
- Build reusable data cleaning functions
- Work with copies (avoid mutating inputs)
- Handle common data quality issues
- Apply cleaning to sample data

**Trainer Demo** (15 mins):

**Key Principle**:
> "Always work on a COPY of the DataFrame. Never modify the input!"

**Live Coding** - Open `src/data_processing/cleaning.py`:

```python
"""Data cleaning functions for library pipeline.

This module contains functions for cleaning and standardizing data.
All functions return new DataFrames without modifying the input.
"""

import pandas as pd
import logging
from typing import List, Optional

logger = logging.getLogger(__name__)

def remove_duplicates(df, subset=None):
    """Remove duplicate rows from DataFrame.
    
    Args:
        df (pd.DataFrame): Input DataFrame
        subset (list, optional): Columns to consider for duplicates
        
    Returns:
        pd.DataFrame: DataFrame with duplicates removed
        
    Example:
        >>> df_clean = remove_duplicates(df, subset=['transaction_id'])
    """
    df = df.copy()  # ✅ Work on a copy!
    
    initial_rows = len(df)
    df = df.drop_duplicates(subset=subset, keep='first')
    removed = initial_rows - len(df)
    
    if removed > 0:
        logger.info(f"Removed {removed} duplicate rows")
    
    return df

def handle_missing_values(df, strategy='drop', fill_value=None, columns=None):
    """Handle missing values in DataFrame.
    
    Args:
        df (pd.DataFrame): Input DataFrame
        strategy (str): 'drop', 'fill', or 'forward_fill'
        fill_value: Value to fill if strategy='fill'
        columns (list, optional): Specific columns to handle
        
    Returns:
        pd.DataFrame: DataFrame with missing values handled
        
    Example:
        >>> df_clean = handle_missing_values(df, strategy='drop')
        >>> df_filled = handle_missing_values(df, strategy='fill', fill_value=0)
    """
    df = df.copy()
    
    if columns:
        target_cols = columns
    else:
        target_cols = df.columns
    
    initial_rows = len(df)
    
    if strategy == 'drop':
        df = df.dropna(subset=target_cols)
        logger.info(f"Dropped {initial_rows - len(df)} rows with missing values")
        
    elif strategy == 'fill':
        if fill_value is None:
            raise ValueError("fill_value must be provided when strategy='fill'")
        df[target_cols] = df[target_cols].fillna(fill_value)
        logger.info(f"Filled missing values with {fill_value}")
        
    elif strategy == 'forward_fill':
        df[target_cols] = df[target_cols].fillna(method='ffill')
        logger.info("Forward filled missing values")
        
    else:
        raise ValueError(f"Unknown strategy: {strategy}")
    
    return df

def standardize_dates(df, date_columns, date_format='%Y-%m-%d'):
    """Standardize date columns to consistent format.
    
    Args:
        df (pd.DataFrame): Input DataFrame
        date_columns (list): Column names containing dates
        date_format (str): Target date format
        
    Returns:
        pd.DataFrame: DataFrame with standardized dates
        
    Example:
        >>> df_clean = standardize_dates(df, ['checkout_date', 'return_date'])
    """
    df = df.copy()
    
    for col in date_columns:
        if col not in df.columns:
            logger.warning(f"Column {col} not found in DataFrame")
            continue
            
        try:
            df[col] = pd.to_datetime(df[col], errors='coerce')
            logger.info(f"Standardized dates in column: {col}")
        except Exception as e:
            logger.error(f"Error standardizing dates in {col}: {e}")
            raise
    
    return df
```

**Key Teaching Points**:
- ✅ Always `df = df.copy()` at start
- ✅ Log what you did
- ✅ Return new DataFrame
- ✅ Handle edge cases (column doesn't exist)
- ✅ Type hints in docstrings

---

**Learner Activity** (60 mins):

**Task 1: Build cleaning.py** (40 mins)

Implement these functions:
1. `remove_duplicates()` (already shown)
2. `handle_missing_values()` (already shown)
3. `standardize_dates()` (already shown)
4. **Your own**: `validate_isbn()` - check if ISBN is valid format
5. **Your own**: `standardize_text()` - trim whitespace, lowercase, etc.

**Task 2: Test in Jupyter** (15 mins)
```python
from data_processing.cleaning import (
    remove_duplicates,
    handle_missing_values,
    standardize_dates
)

# Load data
df = load_csv('../data/circulation_data.csv')

# Apply cleaning pipeline
df_clean = remove_duplicates(df, subset=['transaction_id'])
df_clean = handle_missing_values(df_clean, strategy='drop')
df_clean = standardize_dates(df_clean, ['checkout_date', 'return_date'])

# Check results
print(f"Original rows: {len(df)}")
print(f"Clean rows: {len(df_clean)}")
print(df_clean.info())
```

**Task 3: Commit** (5 mins)
```bash
git add src/data_processing/cleaning.py
git commit -m "Implement data cleaning functions"
git push
```

**Trainer Circulates**:
- Verify they're using `.copy()`
- Check logging is in place
- Help debug any issues
- Ensure functions are tested

**Q&A Checkpoint**:
> - "Show me your cleaning results - how many duplicates removed?"
> - "Are you working on copies or modifying originals?"
> - "Is your code readable to others?"
> - "Have you tested each function?"
> - "Kanban updated?"

**Artifacts Needed**:
- 📊 Slides: Why use .copy()
- 📊 Slides: Common data cleaning patterns
- 💾 Completed `cleaning.py` (reference implementation)
- 📄 Sample cleaning pipeline (Jupyter notebook)

---

### **12:15-13:15 | Lunch (60 mins)**

---

### **Session 3: 13:15-14:30 | Introduction to Testing (75 mins)**
**Format**: Trainer demo (30 mins) → Learner work (45 mins)

**Learning Objectives**:
- Understand why we test data code
- Learn pytest basics and fixtures
- Use `pandas.testing.assert_frame_equal()`
- Write first tests for cleaning module

**Trainer Demo** (30 mins):

**Part 1: Why Test?** (5 mins)

**Show the problem**:
```python
# What if someone changes your function?
def remove_duplicates(df):
    return df.drop_duplicates()  # ❌ But what if it's wrong?
```

**Without tests**: You won't know until production breaks!

**With tests**: Tests fail immediately!

---

**Part 2: The Pandas Testing Problem** (10 mins)

**Live demo in Jupyter**:
```python
import pandas as pd

result = pd.DataFrame({'col': [1, 2, 3]})
expected = pd.DataFrame({'col': [1, 2, 3]})

# Try the wrong way
print("result == expected:")
print(result == expected)  # DataFrame of bools!

# Try to assert (this will fail)
try:
    assert result == expected
except ValueError as e:
    print(f"Error: {e}")
```

**The solution**:
```python
import pandas.testing as pdt

# The right way
pdt.assert_frame_equal(result, expected)
print("✅ Test passed!")
```

---

**Part 3: Writing Tests** (15 mins)

**Live coding** - Create `tests/test_cleaning.py`:

```python
"""Tests for data cleaning functions.

This module demonstrates proper testing patterns for Pandas code.
"""

import pytest
import pandas as pd
import pandas.testing as pdt
from src.data_processing.cleaning import (
    remove_duplicates,
    handle_missing_values,
    standardize_dates
)

# ========================================
# FIXTURES - Reusable test data
# ========================================

@pytest.fixture
def sample_df_with_duplicates():
    """Sample DataFrame with duplicate rows."""
    return pd.DataFrame({
        'id': [1, 2, 2, 3, 3, 3],
        'name': ['Alice', 'Bob', 'Bob', 'Charlie', 'Charlie', 'Charlie'],
        'value': [10, 20, 20, 30, 30, 30]
    })

@pytest.fixture
def sample_df_with_missing():
    """Sample DataFrame with missing values."""
    return pd.DataFrame({
        'id': [1, 2, 3, 4],
        'name': ['Alice', None, 'Charlie', 'David'],
        'value': [10, 20, None, 40]
    })

# ========================================
# TESTS FOR remove_duplicates()
# ========================================

def test_remove_duplicates_exact(sample_df_with_duplicates):
    """Test duplicate removal using exact DataFrame comparison."""
    result = remove_duplicates(sample_df_with_duplicates, subset=['id'])
    
    expected = pd.DataFrame({
        'id': [1, 2, 3],
        'name': ['Alice', 'Bob', 'Charlie'],
        'value': [10, 20, 30]
    })
    
    # Reset index for comparison
    result = result.reset_index(drop=True)
    
    pdt.assert_frame_equal(result, expected)

def test_remove_duplicates_properties(sample_df_with_duplicates):
    """Test duplicate removal using property assertions."""
    result = remove_duplicates(sample_df_with_duplicates, subset=['id'])
    
    # Test properties instead of exact values
    assert len(result) == 3
    assert result['id'].is_unique
    assert set(result['id']) == {1, 2, 3}

def test_remove_duplicates_no_changes():
    """Test with DataFrame that has no duplicates."""
    df_unique = pd.DataFrame({
        'id': [1, 2, 3],
        'name': ['A', 'B', 'C']
    })
    
    result = remove_duplicates(df_unique, subset=['id'])
    
    pdt.assert_frame_equal(result, df_unique)

def test_remove_duplicates_empty():
    """Test with empty DataFrame."""
    empty_df = pd.DataFrame({'id': [], 'name': []})
    result = remove_duplicates(empty_df)
    
    assert len(result) == 0
    pdt.assert_frame_equal(result, empty_df)

# ========================================
# TESTS FOR handle_missing_values()
# ========================================

def test_handle_missing_drop(sample_df_with_missing):
    """Test dropping rows with missing values."""
    result = handle_missing_values(sample_df_with_missing, strategy='drop')
    
    # Should only have rows without any NaN
    assert len(result) == 2
    assert result['name'].notna().all()
    assert result['value'].notna().all()

def test_handle_missing_fill(sample_df_with_missing):
    """Test filling missing values."""
    result = handle_missing_values(
        sample_df_with_missing, 
        strategy='fill', 
        fill_value=0
    )
    
    # Should have all 4 rows
    assert len(result) == 4
    # No missing values
    assert result['name'].notna().all() or (result['name'] == 0).any()
    assert result['value'].notna().all()

def test_handle_missing_invalid_strategy(sample_df_with_missing):
    """Test that invalid strategy raises error."""
    with pytest.raises(ValueError, match="Unknown strategy"):
        handle_missing_values(sample_df_with_missing, strategy='invalid')
```

**Run the tests**:
```bash
cd ~/projects/library-pipeline

# Run all tests
pytest tests/test_cleaning.py -v

# Run with coverage
pytest tests/test_cleaning.py -v --cov=src.data_processing.cleaning --cov-report=term-missing
```

**Key Teaching Points**:
- ✅ Use `@pytest.fixture` for reusable test data
- ✅ Use `pdt.assert_frame_equal()` for exact comparison
- ✅ Test properties when exact comparison is too strict
- ✅ Test edge cases (empty, no changes needed)
- ✅ Test error cases with `pytest.raises()`
- ✅ Descriptive test names: `test_<function>_<scenario>`

---

**Learner Activity** (45 mins):

**Task 1: Write tests for cleaning module** (35 mins)

Create `tests/test_cleaning.py` with:
- At least 2 tests for `remove_duplicates()`
- At least 2 tests for `handle_missing_values()`
- At least 1 test for `standardize_dates()`
- Use fixtures for test data
- Use `pandas.testing.assert_frame_equal()`

**Task 2: Run tests and check coverage** (10 mins)
```bash
# Run tests
pytest tests/test_cleaning.py -v

# Check coverage
pytest tests/ --cov=src --cov-report=term-missing

# Aim for 70%+ coverage!
```

**Trainer Circulates**:
- Help with pytest syntax
- Verify they're using `pandas.testing`
- Check test quality (meaningful tests, not just passing)
- Help achieve coverage targets

**Q&A Checkpoint**:
> - "Are all your tests passing?"
> - "What's your coverage percentage?"
> - "Show me a test that uses assert_frame_equal()"
> - "Do you have tests for edge cases?"

**Artifacts Needed**:
- 📊 Slides: Why test data code?
- 📊 Slides: The Pandas testing problem (visual)
- 📊 Slides: pytest basics and fixtures
- 💾 Completed `test_cleaning.py` (reference implementation)
- 📄 pytest cheat sheet
- 📄 Pandas testing quick reference

---

### **Session 4: 14:50-15:50 | Achieve Test Coverage (60 mins)**
**Format**: Learner-led work with trainer support

**Learning Objectives**:
- Write comprehensive tests for ingestion module
- Reach 70%+ test coverage target
- Understand what good coverage means
- Fix any failing tests

**Trainer Introduction** (10 mins):

**What is Test Coverage?**
```
Coverage = (Lines of code executed by tests) / (Total lines of code)

Example:
Your code:     10 lines
Tests run:      7 lines
Coverage:      70%
```

**But**: Coverage isn't everything!
- ✅ 70% coverage with good tests > 90% coverage with meaningless tests
- ✅ Test behavior, not just lines
- ✅ Edge cases matter more than happy path

**Stretch Goal**: 80% coverage
**Acceptable**: 70% coverage
**Minimum**: All critical functions tested

---

**Learner Activity** (45 mins):

**Task 1: Test ingestion module** (25 mins)

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

**Task 2: Run coverage report** (10 mins)
```bash
# Full coverage report
pytest tests/ -v --cov=src.data_processing --cov-report=term-missing

# See which lines aren't covered
pytest tests/ --cov=src.data_processing --cov-report=html
# Open htmlcov/index.html in browser
```

**Task 3: Improve coverage** (10 mins)
- Identify uncovered lines
- Write tests for those lines
- Focus on error handling and edge cases

**Task 4: Document and commit** (5 mins)
```bash
# Update README with coverage
echo "Test Coverage: 75%" >> README.md

git add tests/
git commit -m "Add comprehensive tests - 75% coverage achieved"
git push
```

**Trainer Circulates**:
- Check coverage reports
- Suggest missing test cases
- Help with failing tests
- Verify test quality

**Q&A Checkpoint**:
> - "What's your final coverage percentage?"
> - "Show me your coverage report"
> - "Which functions aren't fully tested? Why?"
> - "Are all tests passing?"
> - "Have you committed your tests?"
> - "Kanban board updated?"

**Artifacts Needed**:
- 📊 Slides: Understanding test coverage
- 📊 Slides: What to test (priority guide)
- 📄 Testing checklist
- 📄 Coverage report interpretation guide

---

### **15:50-16:00 | Day 2 Wrap-Up (10 mins)**
**Format**: Trainer-led

**Content**:

**What We Built Today**:
- ✅ Complete data ingestion module (CSV, JSON, Excel)
- ✅ Complete data cleaning module
- ✅ Comprehensive test suite
- ✅ 70%+ test coverage
- ✅ Everything working locally

**Check-In**:
- Quick poll: "Hands up if all tests passing?"
- Quick poll: "Hands up if 70%+ coverage?"
- Quick poll: "Hands up if code is on GitHub?"

**Tomorrow (Day 3)**:
- Morning: Integrate with Fabric (notebooks)
- Afternoon: GitHub Actions CI/CD (see automation in action!)
- Docker awareness (brief demo)

**Homework** (Optional):
- Refactor any messy code
- Improve test coverage toward 80%
- Review GitHub Actions documentation

**Key Reminder**:
> "You now have production-quality Python code that's tested and documented. Tomorrow we automate the testing and deployment!"

**Artifacts Needed**:
- 📊 Slides: Day 2 accomplishments
- 📊 Slides: Day 3 preview

---

## Day 2 Artifacts Summary

### **Slides/Presentations** 📊
- [ ] Day 2 overview and Day 1 recap
- [ ] Principles of good Python functions
- [ ] Error handling patterns
- [ ] Why use .copy() in Pandas
- [ ] Common data cleaning patterns
- [ ] Why test data code?
- [ ] The Pandas testing problem (visual)
- [ ] pytest basics and fixtures
- [ ] Understanding test coverage
- [ ] What to test (priority guide)
- [ ] Day 2 accomplishments and Day 3 preview

### **Code/Reference Implementations** 💾
- [ ] Completed `src/data_processing/ingestion.py`
- [ ] Completed `src/data_processing/cleaning.py`
- [ ] Completed `tests/test_cleaning.py`
- [ ] Completed `tests/test_ingestion.py`
- [ ] Sample Jupyter notebook (testing pipeline)

### **Documents/Guides** 📄
- [ ] Code quality checklist
- [ ] Sample cleaning pipeline (Jupyter notebook example)
- [ ] pytest cheat sheet
- [ ] Pandas testing quick reference card
- [ ] Testing checklist (what to test)
- [ ] Coverage report interpretation guide

---

## Key Pedagogical Elements in Day 2

### **1. Progressive Complexity**
- Session 1: Simple ingestion (reading files)
- Session 2: More complex cleaning (transformations)
- Session 3: Testing concepts
- Session 4: Applied testing

### **2. Repetition of Best Practices**
Every session reinforces:
- Work on copies
- Add logging
- Document with docstrings
- Test your code
- Commit frequently

### **3. Immediate Application**
- Demo → Build → Test → Commit
- See it work in Jupyter before writing tests
- Tangible progress each session

### **4. Support Structure**
- Trainer demos first (I do)
- Learners practice (We do)
- Trainer circulates (You do with support)
- Q&A checkpoints (Verify understanding)

---