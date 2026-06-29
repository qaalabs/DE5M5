# Production Readiness Yardstick

These four checks are your measure of production-ready code. Run them now to see where you are, and again after each activity to see progress.

Run all commands in the VS Code terminal.

---

## Check 1: Tests pass and coverage

```
pytest --cov=src
```

All tests should pass and coverage should be above 70%. A failing test means broken code - below 70% means you do not have enough tests to trust the code in production.

---

## Check 2: Code quality

```
ruff check src/
```

No errors. Any errors reported here need to be fixed before the code is production ready.

---

## Check 4: Pipeline output

```
python -m data_processing.run_pipeline > report.txt
```

Open `report.txt` in VS Code. For each dataset, compare the **Raw data** numbers against the **Cleaned data** numbers:

- **Duplicates** should be 0 after cleaning
- **Missing values** should be 0 after cleaning

If the numbers are the same before and after, the cleaning functions are not working.

---

## Commit your report

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Stage `report.txt`
- Commit message: `Add pipeline report`
- Click **Sync Changes**

You can now see the report in GitHub. Each time you run the pipeline and commit, GitHub shows the change - so you can track progress through the day.
