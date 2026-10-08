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

The pytest line ends with `--cov-fail-under=90` - the check fails if coverage is under 90%. The template ships with it set that high on purpose.

### lint.yml

Same trigger - runs on every PR to main. Installs ruff and runs `ruff check src/` - the same command and scope taught in `day2/linting.md`. If there are linting errors the PR is blocked. They fixed all their linting issues yesterday - this should pass cleanly.

## Demo: open a PR and watch CI fail

On your (trainer) repo:

1. Go to GitHub -> **Pull requests** -> **New pull request**
2. Base: `main` | Compare: `dev`
3. Title: `Merge dev into main` -> **Create pull request**
4. Show the two CI checks appearing automatically

The `ruff` check passes ✅. The `test` check fails ❌ - and it is meant to. Yesterday's target was 70% coverage, so nobody is above 90%.

Click **Details** on the failed check and show the output: every test passed, but coverage is below the threshold. CI has blocked the merge, which is exactly its job.

## Lower the threshold - watch it pass

The PR cannot merge until the check passes. In GitHub, navigate to `.github/workflows/ci.yml` on the `dev` branch and find the line that caused the fail:

```yaml
pytest --cov=src --cov-report=term-missing --cov-fail-under=90
```

Click the pencil icon to edit and change `90` to `40`:

```yaml
pytest --cov=src --cov-report=term-missing --cov-fail-under=40
```

40 is deliberately low - everyone clears it, whatever coverage they reached yesterday.

Commit directly to `dev`. CI re-runs - passes ✅. Both checks green. Click **Merge pull request**.

Dev is now on main, and CI confirmed it was working before it got there.
