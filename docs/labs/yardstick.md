# Production Readiness Yardstick

These four checks are your measure of production-ready code. Run them now to see where you are, and again after each activity to see your progress.

!!! note "Run all commands either in a Windows Terminal or in the VS Code terminal."

---

## Check 1: Run the pipeline

Run your pipeline:

```
python -m data_processing.run_pipeline
```

!!! success "The pipeline should run successfully. There should be no syntax errors."

---

## Check 2: Run pytest

Run PyTest with coverage:

```
python -m pytest --cov=src
```

!!! success "All tests should pass. A failing test means broken code."

!!! success "Coverage should be over 70%"

---

## Check 3: Inspect code quality

Run the Ruff linter:

```
python -m ruff check src/
```

!!! success "There should be no issues reported. Any issues reported here need to be fixed before the code is production ready."

---

## Final steps: Write the output to a file

## 1. Run the Pipeline and save to a file:

```
python -m data_processing.run_pipeline > report.txt
```

### 2. Run pytest and coverage and save to a file:

```
python -m pytest --cov=src --cov-report=term-missing --color=no -q > pytest_result.txt
```

### 3. Push the report files to GitHub:

```
git add *.txt
git commit -m "Add pipeline reports"
git push
```

---

!!! abstract "S26: Identify data quality metrics and track them to ensure the quality, accuracy and reliability of the data product."

!!! abstract "K24: Processes for evaluating prototypes and taking them to implementation within a production environment."

