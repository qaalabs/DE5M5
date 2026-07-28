# Activity: Implement ISBN Validation

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

## Commit your work

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/validation.py` to stage it
- Type a commit message: `Implement validate_isbn`
- Click **Commit**
- Click **Sync Changes** to push to GitHub

