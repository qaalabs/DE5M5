# Virtual Machine ~ LOD Daily Setup Steps

Use this whenever you land on a fresh environment. These steps get you back to a working environment.

!!! note "Do all the steps below in your Virtual Machine"


## Step 1: Log in to GitHub

1. Open a browser and go to [github.com](https://github.com)
2. Sign in with your GitHub account.
3. Find your `qa-library-pipeline` repo.
4. Copy the **HTTPS clone URL** from the **Code** button.


## Step 2: Open Terminal

Then:

```
cd Desktop
```

You should now be in the Desktop directory: `PS C:\Users\Admin\Desktop`


## Step 3: Git config

!!! note "Use the same email address as your GitHub account"
    If it doesn't match, GitHub won't link the commits to your profile - they won't show on your contribution graph.

```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```


## Step 4: Clone your repo

```
git clone https://github.com/YOUR_USERNAME/qa-library-pipeline.git
```

!!! note "Replace `YOUR_USERNAME` and `qa-library-pipeline` with your own details."

Change to the repo directory. For example: 

```
cd qa-library-pipeline
```


## Step 5: Switch to the dev branch

```
git checkout dev
```


## Step 6: Authenticate with GitHub

```
git push --dry-run
```

!!! info "A browser window may open asking you to sign in - sign in to complete authentication."


## Step 7: Install dependencies

```
pip install -r requirements_dev.txt
```

Tell python to run as a package:

```
pip install -e .
```


## Step 8: Open VS Code

```
code .
```

!!! info "Click **Yes, I trust the authors** if prompted."

## Step 9: Verify the setup

In the original Terminal run:

```
python -m pytest
```

!!! success "All tests should pass!"

