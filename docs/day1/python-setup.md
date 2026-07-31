# Python Environment Setup

!!! note "Before you start"
    You should already have your repo cloned, on the `dev` branch, with Terminal open in your `qa-library-pipeline` folder. If not, run [Clone your Repository](clone-repo.md) first.

## Step 1: Install dependencies

Run:

```bash
pip install -r requirements_dev.txt
```

Then run:

```bash
pip install -e .
```

!!! note "This installs your package in editable mode"
    - This allows Python to import your code directly from the `src` folder
    - Any changes you make take effect immediately without reinstalling.

---

## Step 2: Open VS Code

!!! note "There's a space between `code` and `.`"
    The `.` means "this folder" - `code .` opens the current directory in VS Code.

```bash
code .
```

!!! info "If you see a 'Do you trust the authors of the files in this folder?' prompt"
    Click **Yes, I trust the authors** - this is your own cloned repo.

!!! success "VS Code will open with the project loaded."

---

## Step 3: Verify the setup

In the original **Terminal window**, run:

```bash
python -m pytest
```

!!! success "All tests should pass! If they do, your environment is working correctly."
