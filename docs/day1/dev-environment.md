# Set Up Your Development Environment

Run these commands in **Terminal** inside your repo folder.

## Step 1: Install dependencies

```sh
pip install -r requirements_dev.txt
```

## Step 2: Install the package

```sh
pip install -e .
```

This installs your package in editable mode - changes you make to the code take effect immediately without reinstalling.

## Step 3: Verify the setup

```sh
python -m pytest
```

All tests should pass. If they do, your environment is working correctly.

