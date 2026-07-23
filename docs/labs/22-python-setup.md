# Python Environment Setup

Everyone does this lab - whether your VM wiped overnight or not.

---

## Step 1: Open Terminal and navigate to your repo

Open **Terminal** and navigate to your `qa-library-pipeline` folder.

---

## Step 2: Switch to the dev branch

```
git checkout dev
```

---

## Step 3: Install dependencies

```
pip install -r requirements_dev.txt
```

```
pip install -e .
```

!!! note "This installs your package in editable mode"
    - This allows Python to import your code directly from the `src` folder
    - Any changes you make take effect immediately without reinstalling.

---

## Step 4: Open VS Code

```
code .
```

VS Code will open with the project loaded. Open the built-in terminal with **View → Terminal** (or `` Ctrl+` ``). Run:

```
pytest
```

All tests should pass. You are ready for Day 2.
