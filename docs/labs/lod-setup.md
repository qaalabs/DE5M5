# VM / LOD Setup

Use this whenever you land on a fresh environment - your VM has reset overnight, or your LOD lab session has expired. Everything is gone except your GitHub repo. These steps get you back to a working environment.

!!! note "Do all the steps below in your Virtual Machine"

## Start your Virtual Machine

## Step 1: Log in to GitHub

1. Open a browser and go to [github.com](https://github.com)

2. Sign in with your GitHub account.

3. Find your `qa-library-pipeline` repo

4. Copy the **HTTPS clone URL** from the **Code** button.


## Step 2: Open Terminal

1. Open **Terminal** from the Desktop.

2. Run:

    ```
    cd Desktop
    ```


## Step 3: Git config

1. Set your GitHub Email and Name

    ```
    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
    ```

    !!! note "You do not have to use the same email and name as your GitHub account."


## Step 4: Clone your repo

1. In the same Terminal window, run:

    ```
    git clone https://github.com/YOUR_USERNAME/qa-library-pipeline.git
    ```

2. Navigate to the directory you have just created:

    ```
    cd qa-library-pipeline
    ```


## Step 5: Switch to the dev branch

1. In the same Terminal window, run:

    ```
    git checkout dev
    ```


## Step 6: Authenticate with GitHub

1. In the same Terminal window, run:

    ```
    git push origin dev
    ```

    !!! note "No code has changed so Git should say: **Everything up to date**"
        - But it has to connect to GitHub to confirm.
        - A browser window will open asking you to sign in.
        - Signin to complete the authentication process.

    !!! success "If you are not prompted, your credentials are already cached and you are good to go."


## Step 7: Install dependencies

1. In the same Terminal window, run:

    ```
    pip install -r requirements_dev.txt
    ```

2. Then run:

    ```
    pip install -e .
    ```

    !!! note "This installs your package in editable mode"
        - This allows Python to import your code directly from the `src` folder
        - Any changes you make take effect immediately without reinstalling.


## Step 8: Open VS Code

1. In the same Terminal window, run:

    ```
    code .
    ```

    !!! note "There's a space between `code` and `.`"
        The `.` means "this folder" - `code .` opens the current directory in VS Code.

    !!! info "If you see a 'Do you trust the authors of the files in this folder?' prompt"
        Click **Yes, I trust the authors** - this is your own cloned repo.

    !!! success "VS Code will open with the project loaded."


## Step 9: Verify the setup

1. In the original **Terminal window**, run:

    ```
    python -m pytest
    ```

    !!! success "All tests should pass! If they do, your environment is working correctly."
