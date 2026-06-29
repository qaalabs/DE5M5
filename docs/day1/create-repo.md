# Activity: Set Up Your Repository

Everything in this session happens in GitHub - no cloning yet.

## Step 1: Create your repo

Go to the template:

- https://github.com/QAADE5/library-pipeline-template

Click **"Use this template"** → **"Create a new repository"**

- Owner: your GitHub account
- Name: `qa-library-pipeline`
- Visibility: Public
- Click **"Create repository"**

## Step 2: Explore the structure

Spend a few minutes clicking around your new repo. Find:

- Where the data files live
- Where your Python package will go
- The GitHub Actions workflows already set up

## Step 3: Create a dev branch

- Click the branch dropdown (shows **main**)
- Type `dev` and click **"Create branch: dev from main"**

This is the branch you'll work on.

## Step 4: Protect the main branch

- Go to **Settings** → **Branches** → **"Add branch protection rule"**
- Branch name pattern: `main`
- Check **"Require a pull request before merging"**
- Uncheck **"Require approvals"** - the default is on, which means you cannot merge your own PRs
- Click **"Create"**

If you accidentally edit a file on `main`, GitHub will create a branch and a PR - that is expected. Close the PR and delete the branch.

## Step 5: Add your architecture diagram

First, make sure you're on the `dev` branch - check the branch dropdown shows **dev**, not **main**.

- Navigate to `docs/architecture/index.md`
- Click the pencil icon to edit
- Paste your Mermaid diagram from the design activity
- Click **"Commit changes"** - in the dialog, confirm the branch is **dev**
- Go back to the file - check the diagram renders correctly

## Step 6: Add your ADR

Stay on the **dev** branch.

- Click **"Add file"** → **"Create new file"**
- Name it: `docs/architecture/ADR-001.md`
- Paste your ADR from Notepad
- Click **"Commit changes"** - confirm the branch is **dev**

You now have two artefacts in your repo - architecture diagram and first decision record.
