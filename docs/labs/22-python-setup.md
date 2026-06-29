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

## Step 3: Create your virtual environment

```
python -m venv venv
```

```
venv\Scripts\activate
```

Your prompt should now show `(venv)`.

---

## Step 4: Install dependencies

```
pip install -r requirements.txt
```

```
pip install -r requirements_dev.txt
```

```
pip install -e .
```

---

## Step 5: Open VS Code

```
code .
```

VS Code will open with the project loaded. Open the built-in terminal with **View → Terminal** (or `` Ctrl+` ``). Your `(venv)` should already be active. Run:

```
pytest
```

All tests should pass. You are ready for Day 2.
