# Demo: GitHub Actions

## What is CI/CD?

```
WITHOUT CI/CD:
Developer writes code -> pushes to main -> production breaks

WITH CI/CD:
Developer opens PR -> tests run automatically ->
  Pass: allowed to merge
  Fail: blocked until fixed
```

- **CI** (Continuous Integration): every PR is tested automatically
- **CD** (Continuous Delivery): deploy automatically when tests pass
- The team's main branch is always in a working state

## The workflows

Show the two files already in the repo under `.github/workflows/`.

### ci.yml

```yaml
on:
  pull_request:
    branches: [ main ]
```

Only runs on PRs targeting main - not on every push. Walk through the steps: checkout, set up Python, install dependencies, run pytest with coverage.

The coverage threshold line is commented out - you will uncomment it in a moment.

### lint.yml

Same trigger - runs on every PR to main. Installs ruff and runs `ruff check src/` - the same command and scope taught in `day2/linting.md`. If there are linting errors the PR is blocked. They fixed all their linting issues yesterday - this should pass cleanly.

## Demo: open a PR and watch CI run

On your (trainer) repo:

1. Go to GitHub -> **Pull requests** -> **New pull request**
2. Base: `main` | Compare: `dev`
3. Title: `Merge dev into main` -> **Create pull request**
4. Show the two CI checks appearing automatically

## Set a high coverage threshold

In GitHub, navigate to `.github/workflows/ci.yml` on the `dev` branch. Click the pencil icon to edit.

Uncomment the coverage line and set it to 90:

```yaml
pytest --cov=src --cov-fail-under=90
```

Click **Commit changes** -> commit directly to `dev`.

Go back to the PR - watch CI re-run. It fails ❌. Show the error output - the coverage is below 90%.

## Lower the threshold - watch it pass

Edit `ci.yml` in GitHub again, change to 40:

```yaml
pytest --cov=src --cov-fail-under=40
```

Commit directly to `dev`. CI re-runs - passes ✅. Both checks green. Click **Merge pull request**.

Dev is now on main, and CI confirmed it was working before it got there.
