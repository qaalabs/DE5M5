# Solution: ISBN Validation

`validate_isbn()` returns the cleaned ISBN-13 string if valid, or `None` if invalid - not `True`/`False`. This matters later: the cleaned value becomes the join key between catalogue and circulation data.

```python
def validate_isbn(isbn):
    if isbn is None:
        return None

    cleaned = str(isbn).replace("-", "")

    if not cleaned.isdigit():
        return None

    if len(cleaned) != 13:
        return None

    digits = []
    for character in cleaned:
        digits.append(int(character))

    total = 0
    for i in range(12):
        digit = digits[i]
        if i % 2 == 0:
            total += digit * 1
        else:
            total += digit * 3

    check_digit = (10 - (total % 10)) % 10

    if check_digit != digits[12]:
        return None

    return cleaned
```

Walking through it:

- **`None` in, `None` out** - handle the missing-value case first, before any string operations.
- **Check it's all digits** - `cleaned.isdigit()` is `True` only if every character in the string is a digit. If there's a letter, a stray space, or any other punctuation left over after stripping hyphens, this catches it.
- **Check it's the right length** - a valid ISBN-13 is exactly 13 digits, so anything shorter or longer is rejected here.
- **Turn the string into a list of numbers** - `cleaned` is a string like `"9783161484100"`. The loop goes through it one character at a time and builds up a list of actual integers, e.g. `[9, 7, 8, 3, ...]`, so they can be used in arithmetic.
- **Weight and sum the first 12 digits** - loop over positions `0` to `11` (`range(12)`). `i % 2 == 0` is `True` for positions 0, 2, 4, ... (the 1st, 3rd, 5th digit, and so on, since counting starts at 0) - those get multiplied by 1. The rest get multiplied by 3. Add each result to a running `total`.
- **Work out the check digit** - `total % 10` is the remainder when dividing by 10. `10 - that remainder` gives the check digit, except when the remainder is 0 - the second `% 10` handles that edge case so the result is always `0`-`9`.
- **Compare to the 13th digit** - `digits[12]` is the last digit (position 12, since the list is 0-indexed and has 13 items). If it doesn't match the check digit you just calculated, the ISBN is invalid.
- **Return the cleaned string, not the original input** - the caller shouldn't have to re-strip hyphens themselves.

## Commit

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Click **+** next to `src/data_processing/validation.py` to stage it
- Type a commit message: `Implement validate_isbn`
- Click **Commit**
- Click **Sync Changes** to push to GitHub
