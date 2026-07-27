# Clone your Repository

Open **Terminal** from the Desktop.

## Step 1: Git config

```bash
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

## Step 2: Clone your repo

```bash
git clone https://github.com/YOUR_USERNAME/qa-library-pipeline.git
```

!!! note "Replace `YOUR_USERNAME` with your GitHub username."

!!! note "Replace `qa-library-pipeline` with what you called your repo."

!!! info "A browser window may open asking you to sign in to GitHub"
    This is Git asking permission to work with your GitHub account on this VM. Sign in and allow access - you only need to do this once today.

```bash
cd qa-library-pipeline
```

## Step 3: Confirm the connection

```bash
git remote -v
```

!!! success "You should see `origin` listed twice (fetch and push), both pointing at your repo."

## Step 4: Switch to the dev branch

```bash
git checkout dev
```

## Step 5: Explore the structure

```bash
dir
```

!!! success "You should see the same folders you explored on GitHub earlier."

