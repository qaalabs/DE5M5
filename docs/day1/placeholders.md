# Replace Template Placeholders

!!! note "Your repo has two files still using placeholder text from the template."
    - Update both now, before you start writing any real code.


## Step 1: Update your README

Open `README.md`.

- Find and replace `YOUR_USERNAME` with your GitHub username.
- Find and replace `YOUR_REPO` with the name of your repo.


## Step 2: Update your Day 4 notebook

Open `notebooks/01_bronze_to_silver.ipynb` in VS Code.

In Cell 1, replace `YOUR_USERNAME` and `YOUR_REPO_NAME` with the same details:

```python
%pip install "git+https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git"
```

!!! success "You'll import this notebook into Fabric on Day 4 - since it already has your details, there's nothing to edit there."


## Step 3: Commit

In VS Code open the **Source Control** panel (`Ctrl+Shift+G`).

- Stage `README.md` and `notebooks/01_bronze_to_silver.ipynb`
- Type a commit message: `Update README and notebook with repo details`
- Click **Commit**
- Click **Sync Changes** to push to GitHub

