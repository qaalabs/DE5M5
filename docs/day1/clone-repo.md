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

For example:

```bash
cd qa-library-pipeline
```


## Step 4: Confirm the connection

We need to give this local clone permission to push to GitHub. Run:

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

```bash
git status
```

!!! success "You should see `On branch dev`"

