# Activity: Merge Your Dev Branch

!!! abstract "K24: Processes for evaluating prototypes and taking them to implementation within a production environment."

!!! abstract "S17: Apply and advocate for software development best practice when working with other data professionals throughout the business. Contribute to standards and ways of working that support software development principles."

Your code is on `dev`. The goal is to get it onto `main` - with CI confirming it works first. Everything happens in GitHub - no command line needed.

## 1. Create a pull request

- Click **Pull requests** -> **New pull request**
- Base: `main` | Compare: `dev`
- Title: `Merge dev into main`
- Click **Create pull request**

You will see two CI checks appear. Leave them running - one of them is going to fail, and that is expected.

## 2. Require status checks before merging

Now that the checks have run once, add real teeth to them:

- Go to **Settings** -> **Branches** -> edit the `main` protection rule
- Check **"Require status checks to pass before merging"**
- Search for and select both: `test` and `ruff`
- Leave **"Require branches to be up to date before merging"** unticked - nothing else is pushing to `main`, so it can never be out of date, and ticking it just adds an unnecessary extra step
- Check **"Do not allow bypassing the above settings"** - without this, you're an admin on your own repo and can just click "Merge without waiting for requirements to be met (bypass rules)" regardless of whether checks pass
- Click **Save changes**

## 3. Find the coverage threshold

In GitHub, navigate to `.github/workflows/ci.yml` on the `dev` branch and find this line:

```yaml
pytest --cov=src --cov-report=term-missing --cov-fail-under=90
```

Note that `--cov-fail-under=90` makes the check fail if test coverage is under 90%. Nothing to change yet.

## 4. See why it failed

Go back to your pull request. The `test` check has failed ❌ - your tests all passed, but your coverage is below the 90% threshold.

Click **Details** on the failed check and read the output - what is it telling you?

## 5. Lower the threshold and merge

Back in `.github/workflows/ci.yml` on the `dev` branch, click the pencil icon to edit. Change `90` to `40`:

```yaml
pytest --cov=src --cov-report=term-missing --cov-fail-under=40
```

Click **Commit changes** -> commit directly to `dev`. CI re-runs on your pull request and both checks pass ✅.

Click **Merge pull request** -> **Confirm merge**.

Your `dev` branch is now on `main`.


!!! success "Finished early? Carry on to [Explore Your GitHub Repo](github-explore.md)."
