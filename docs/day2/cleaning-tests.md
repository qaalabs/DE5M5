# Activity: Write Python Tests

## Task 1: Write tests for cleaning module

Create `tests/test_cleaning.py` with:

- At least 2 tests for `remove_duplicates()`
- At least 2 tests for `handle_missing_values()`
- At least 1 test for `standardize_dates()`
- Use fixtures for test data
- Use `pandas.testing.assert_frame_equal()`

## Task 2: Run tests and check coverage

```powershell
# Run tests
pytest tests/test_cleaning.py -v

# Check coverage
pytest tests/ --cov=src --cov-report=term-missing

```

!!! success "Aim for 70%+ coverage!"
