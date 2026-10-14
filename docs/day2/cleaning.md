# Data Cleaning Module

## Learning Objectives

- Build reusable data cleaning functions
- Work with copies (avoid mutating inputs)
- Handle common data quality issues
- Apply cleaning to sample data

## Key Principle

Always **work on a COPY of the DataFrame**. Never modify the input!

## Part 1: Live Coding

Open `src/data_processing/cleaning.py` and add this code

### def remove_duplicates
```python
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
    df = df.copy()  # Work on a copy!

    df = df.drop_duplicates(subset=subset, keep='first')

    return df

```

### def handle_missing_values
```python
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
    
    if strategy == 'drop':
        df = df.dropna(subset=target_cols)

    elif strategy == 'fill':
        if fill_value is None:
            raise ValueError("fill_value must be provided when strategy='fill'")
        df[target_cols] = df[target_cols].fillna(fill_value)

    elif strategy == 'forward_fill':
        df[target_cols] = df[target_cols].ffill()

    else:
        raise ValueError(f"Unknown strategy: {strategy}")
    
    return df
```

### def standardise_dates
```python
def standardise_dates(df, date_columns, date_format='%Y-%m-%d'):
    """Standardise date columns to consistent format.
    
    Args:
        df (pd.DataFrame): Input DataFrame
        date_columns (list): Column names containing dates
        date_format (str): Target date format
        
    Returns:
        pd.DataFrame: DataFrame with standardised dates
        
    Example:
        >>> df_clean = standardise_dates(df, ['checkout_date', 'return_date'])
    """
    df = df.copy()
    
    for col in date_columns:
        if col not in df.columns:
            raise KeyError(f"Column {col} not found in DataFrame")

        df[col] = pd.to_datetime(df[col], errors='coerce')

    return df
```

## Key Points
- Always `df = df.copy()` at start
- Return new DataFrame
- Fail loudly on bad input (unknown strategy, column doesn't exist)
- Type hints in docstrings

