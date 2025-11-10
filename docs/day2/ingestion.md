# Ingestion

## Learning Objectives

- Build functions to load CSV, JSON, and Excel files
- Implement proper error handling and logging
- Write clean, documented code
- Test functions locally in Jupyter

## Part 1: Principles

We're building a **Python package**, not just scripts. This means:

- Functions do ONE thing well
- Docstrings for every function
- Error handling (what if file doesn't exist?)
- Logging (so we can debug later)
- Pure functions (don't modify inputs)

## Part 2: Live Coding

Open `src/data_processing/ingestion.py` and add this code:

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

### Key Points to note:
- Type hints in docstrings
- Error handling with try/except
- Logging at INFO and ERROR levels
- Raising exceptions (don't hide errors!)
- Docstring format (Google style)


## Part 3: Test in Jupyter

- Create a `notebooks` folder
- Create a new Jupyter Notebook in this folder
- Copy and run the following code:

```python
# In Jupyter notebook
import sys
sys.path.append('C:\\Users\\Admin\\Documents\\GitHub\\library-pipeline\\src')

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
