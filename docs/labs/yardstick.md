# Production Readiness Yardstick

!!! abstract "K24: Processes for evaluating prototypes and taking them to implementation within a production environment."

!!! abstract "S26: Identify data quality metrics and track them to ensure the quality, accuracy and reliability of the data product."

These four checks are your measure of production-ready code. Run them now to see where you are, and again after each activity to see progress.

!!! note "Run all commands either in a Windows Terminal or in a in the VS Code terminal."

---

## Step 1: Tests pass and coverage

1. Run PyTest with coverage:

    ```
    pytest --cov=src
    ```

2. Run PyTest with coverage, saving the result to a file:

    ```
    pytest --cov=src --cov-report=term-missing --color=no -q > pytest_result.txt
    ```

    !!! success "All tests should pass. A failing test means broken code."

    !!! info "Coverage should be above 70%."
        Below 70% means you do not have enough tests to trust the code in production.


## Step 2: Code quality

1. Run the Ruff linter:

    ```
    ruff check src/
    ```

    !!! success "There should be no issues reported"
        Any issues reported here need to be fixed before the code is production ready.


## Step 3: Pipeline output

1. Run your pipeline:

    ```
    python -m data_processing.run_pipeline
    ```

2. Run and Save the output to a file:

    ```
    python -m data_processing.run_pipeline > report.txt
    ```

3. Open `report.txt` in VS Code.

    For each dataset, compare the **Raw data** numbers against the **Cleaned data** numbers:

    - **Duplicates** should be 0 after cleaning
    - **Missing values** should be 0 after cleaning

    !!! warning "If the numbers are the same before and after, then the cleaning functions are not working."


## Step 4: Commit your report

1. In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

    - Stage: `report.txt` and `pytest_result.txt`
    - Commit message: `Add pipeline report`
    - Click: Sync Changes

    !!! success "You should now see the report in GitHub."
        - Each time you run the pipeline and commit, GitHub shows the change.
        - This will help you track your progress throughout the day.

