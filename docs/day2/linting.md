# Code Linting

Linting checks your code for errors and bad patterns before you run it. In a production pipeline, linting runs automatically on every commit - if it fails, the code does not merge.

## Run the linter

```
ruff check src/
```

A clean result looks like this:

```
All checks passed!
```

If there are issues, ruff tells you exactly where:

```
src/data_processing/cleaning.py:12:1: F401 'pandas as pd' imported but unused
src/data_processing/ingestion.py:34:5: E711 comparison to None (use 'is' or 'is not')
```

Common errors you will see:

| Code | Meaning |
|------|---------|
| F401 | Imported but unused |
| E711 | Wrong comparison to None |
| E501 | Line too long |
| F841 | Variable assigned but never used |

## Check formatting

```
ruff format --check src/
```

This checks whether your code is consistently formatted - spacing, quotes, line breaks. Unlike `ruff check`, formatting issues are not errors in your logic. You can auto-fix all of them in one command:

```
ruff format src/
```

## What about flake8?

You may see `flake8` in other projects - it does a similar job. `ruff` is the modern replacement: it is faster and catches more issues. If you see a project using `flake8`, `ruff check` will catch the same problems.
