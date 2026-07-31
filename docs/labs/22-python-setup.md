# Python Environment Setup

!!! info "If you are using Learn on Demand - make sure that you have already run: [LOD setup](lod-setup.md)"

## Step 1: Open Terminal and navigate to your repo

Open **Terminal**. Navigate to your `qa-library-pipeline` folder:

```
cd Desktop
cd qa-library-pipeline
```

---

## Step 2: Switch to the dev branch

```
git checkout dev
```

---

## Step 3: Install dependencies

Run:

```
pip install -r requirements_dev.txt
```

Then run:

```
pip install -e .
```

!!! note "This installs your package in editable mode"
    - This allows Python to import your code directly from the `src` folder
    - Any changes you make take effect immediately without reinstalling.

---

## Step 4: Open VS Code

!!! note "There's a space between `code` and `.`"
    The `.` means "this folder" - `code .` opens the current directory in VS Code.

```
code .
```

!!! info "If you see a 'Do you trust the authors of the files in this folder?' prompt"
    Click **Yes, I trust the authors** - this is your own cloned repo.

!!! success "VS Code will open with the project loaded."

---

## Step 5: Run `pytest` to make sure all tests pass

In the original **Terminal window**, run:

```
python -m pytest
```

!!! success "All tests should pass!"

