# Activity: Merge Your Dev Branch

!!! abstract "K24: Processes for evaluating prototypes and taking them to implementation within a production environment."

!!! abstract "S17: Apply and advocate for software development best practice when working with other data professionals throughout the business. Contribute to standards and ways of working that support software development principles."

Your code is on `dev`. The goal is to get it onto `main` - with CI confirming it works first. Everything happens in GitHub - no command line needed.

## 1. Create a pull request

- Click **Pull requests** -> **New pull request**
- Base: `main` | Compare: `dev`
- Title: `Merge dev into main`
- Click **Create pull request**

You will see two CI checks appear. Leave them running.

## 2. Require status checks before merging

Now that the checks have run once, add real teeth to them:

- Go to **Settings** -> **Branches** -> edit the `main` protection rule
- Check **"Require status checks to pass before merging"**
- Search for and select both: `test` and `ruff`
- Leave **"Require branches to be up to date before merging"** unticked - nothing else is pushing to `main`, so it can never be out of date, and ticking it just adds an unnecessary extra step
- Check **"Do not allow bypassing the above settings"** - without this, you're an admin on your own repo and can just click "Merge without waiting for requirements to be met (bypass rules)" regardless of whether checks pass
- Click **Save changes**

## 3. Set a coverage threshold

In GitHub, navigate to `.github/workflows/ci.yml` on the `dev` branch. Click the pencil icon to edit.

Uncomment the coverage line and set it to 90:

```yaml
pytest --cov=src --cov-fail-under=90
```

Click **Commit changes** -> commit directly to `dev`.

## 4. Watch it fail

Go back to your pull request. Click **Details** on the CI check and watch it run.

It will fail ❌. Read the output - what is it telling you?

## 5. Lower the threshold and merge

Edit `.github/workflows/ci.yml` in GitHub again. Change the threshold to 40:

```yaml
pytest --cov=src --cov-fail-under=40
```

Commit directly to `dev`. Both checks pass ✅.

Click **Merge pull request** -> **Confirm merge**.

Your `dev` branch is now on `main`.
