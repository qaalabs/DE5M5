# Activity: Merge Your Dev Branch

Your code is on `dev`. The goal is to get it onto `main` - with CI confirming it works first. Everything happens in GitHub - no command line needed.

## 1. Create a pull request

- Click **Pull requests** -> **New pull request**
- Base: `main` | Compare: `dev`
- Title: `Merge dev into main`
- Click **Create pull request**

You will see two CI checks appear. Leave them running.

## 2. Set a coverage threshold

In GitHub, navigate to `.github/workflows/ci.yml` on the `dev` branch. Click the pencil icon to edit.

Uncomment the coverage line and set it to 90:

```yaml
pytest --cov=src --cov-fail-under=90
```

Click **Commit changes** -> commit directly to `dev`.

## 3. Watch it fail

Go back to your pull request. Click **Details** on the CI check and watch it run.

It will fail ❌. Read the output - what is it telling you?

## 4. Lower the threshold and merge

Edit `.github/workflows/ci.yml` in GitHub again. Change the threshold to 40:

```yaml
pytest --cov=src --cov-fail-under=40
```

Commit directly to `dev`. Both checks pass ✅.

Click **Merge pull request** -> **Confirm merge**.

Your `dev` branch is now on `main`.
