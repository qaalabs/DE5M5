# Activity: Fix Linting Errors

!!! abstract "S17: Apply and advocate for software development best practice when working with other data professionals throughout the business. Contribute to standards and ways of working that support software development principles."

## Step 1: Run the linter

```
ruff check src/
```

Work through each error reported. The output tells you the file, line number, and what is wrong.

## Step 2: Fix the errors

Open the file in VS Code, go to the line number, and fix the issue. Common fixes:

- **F401 unused import** - delete the import line
- **E711 comparison to None** - change `== None` to `is None`
- **F841 unused variable** - remove the variable or use it
- **E501 line too long** - break the line into two

Run `ruff check src/` again after each fix to see progress.

## Step 3: Check formatting

```
ruff format --check src/
```

If there are formatting issues, fix them automatically:

```
ruff format src/
```

## Step 4: Commit your work

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Stage any files you changed
- Commit message: `Fix linting errors`
- Click **Sync Changes** to push to GitHub
