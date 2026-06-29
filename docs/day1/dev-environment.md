# Set Up Your Development Environment

Run these commands in **Terminal** inside your repo folder.

## Step 1: Create virtual environment

```sh
python -m venv venv
```

## Step 2: Activate the environment

```sh
venv\Scripts\activate
```

Your prompt should now show `(venv)`.

!!! note "Check this works on your VM - the path may vary"

## Step 3: Install dependencies

```sh
pip install -r requirements_dev.txt
```

## Step 4: Install the package

```sh
pip install -e .
```

This installs your package in editable mode - changes you make to the code take effect immediately without reinstalling.

## Step 5: Verify the setup

```sh
pytest
```

All tests should pass. If they do, your environment is working correctly.

