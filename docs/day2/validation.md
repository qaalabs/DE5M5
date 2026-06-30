# Activity: Implement ISBN Validation

Open `src/data_processing/validation.py`. The `validate_isbn()` function currently returns `True` for everything - it needs real validation logic.

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

Implement `validate_isbn()` so it returns `True` for a valid ISBN-13 and `False` for anything invalid.

## Commit your work

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/validation.py` to stage it
- Type a commit message: `Implement validate_isbn`
- Click **Commit**
- Click **Sync Changes** to push to GitHub

