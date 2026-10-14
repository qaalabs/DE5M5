# Activity: Implement ISBN Validation

!!! abstract "S9: Query and manipulate data using tools and programming such as SQL and Python. Manage database access, and implement automated validation checks."

Open `src/data_processing/validation.py`. The `validate_isbn()` function currently just returns whatever it was given - it needs real validation and cleaning logic.

## ISBN-13 rules

- Must be exactly 13 digits (hyphens may appear in the input - strip them first)
- All characters must be digits

### Stretch addition

The 13th digit is a check digit calculated from the first 12:

- Multiply each digit alternately by 1 and 3
- Sum the results
- Check digit = `(10 - (sum % 10)) % 10`
- If your calculated check digit matches the 13th digit, the ISBN is valid

## Your task

Implement `validate_isbn()` so it returns the cleaned ISBN-13 string (hyphens removed) if it's valid, or `None` if it's invalid. This is different from a plain `True`/`False` check - callers need the cleaned value back so it can be used as a join key later in the pipeline.

## Check it

Open your `sandbox` notebook in Jupyter Notebook and run this in a new cell:

```python
from data_processing.validation import validate_isbn

print(validate_isbn('978-0-551-16436-9'))
print(validate_isbn('978-0-551-1643'))
print(validate_isbn('97A-0-551-16436-9'))
```

!!! success "You should see `9780551164369`, then `None` (too short), then `None` (not all digits)."

If you did the stretch addition, these two should also pass:

```python
print(validate_isbn('978-0-551-16436-0'))
print(validate_isbn(None))
```

!!! success "You should see `None` (wrong check digit), then `None` again - without an error. 258 loans in the circulation data have no ISBN at all."

## Commit your work

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/validation.py` to stage it
- Type a commit message: `Implement validate_isbn`
- Click **Commit**
- Click **Sync Changes** to push to GitHub

