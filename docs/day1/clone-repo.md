# Clone your Repository

## Step 1: Open Terminal from the Desktop

This will get you to a command prompt:
```
PS C:\Users\Admin
```

Type:
```bash
cd Desktop
```
Press Enter. You should now be in the Desktop directory:
```
PS C:\Users\Admin\Desktop
```

## Step 2: Git config

Git tags every commit you make with an author - this is what shows up in `git log`, `git blame`, and your commit history on GitHub. Use your real GitHub email and name, replacing the placeholders below:

!!! note "Use the same email address as your GitHub account"
    If it doesn't match, GitHub won't link the commits to your profile - they won't show on your contribution graph.

```bash
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

## Step 3: Clone your repo

```bash
git clone https://github.com/YOUR_USERNAME/qa-library-pipeline.git
```

!!! note "Replace `YOUR_USERNAME` with your GitHub username."

!!! note "Replace `qa-library-pipeline` with what you called your repo."

```bash
cd qa-library-pipeline
```

## Step 4: Confirm the connection

```bash
git remote -v
```

!!! success "You should see `origin` listed twice (fetch and push), both pointing at your repo."

```bash
git push --dry-run
```

!!! info "A browser window may open asking you to sign in to GitHub"
    This is Git checking you have permission to push. Sign in and allow access - you only need to do this once today. Nothing is actually pushed yet.

## Step 5: Switch to the dev branch

!!! note "Note: All development must be done on the `dev` branch because your `main` branch is protected."

```bash
git checkout dev
```

!!! note "Run `git status` to confirm that there are no outstanding code changes need a commit."

```bash
git status
```

!!! success "On branch dev, nothing to commit, working tree clean"

## Step 6: Explore the structure

```bash
dir
```

!!! success "You should see the same folders you explored on GitHub earlier."

