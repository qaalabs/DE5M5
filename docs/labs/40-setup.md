# Day 4 Setup

Your VM has reset overnight. Everything is gone except your GitHub repo. This activity gets you back to a working environment.

---

## Step 1: Log in to GitHub

Open a browser and go to [github.com](https://github.com)

Sign in with your GitHub account. Find your `qa-library-pipeline` repo and copy the HTTPS clone URL from the **Code** button.

---

## Step 2: Open Terminal

Open **Terminal** from the Desktop.

Run:

```
cd Desktop
```

---

## Step 3: Git config

```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

Use the same email and name as your GitHub account.

---

## Step 4: Clone your repo

```
git clone https://github.com/YOUR_USERNAME/qa-library-pipeline.git
```

Navigate to the directory you have just created:
```
cd qa-library-pipeline
```

---

## Step 5: Switch to the dev branch

```
git checkout dev
```

---

## Step 6: Authenticate with GitHub

```
git push origin dev
```

Nothing has changed so Git will say "Everything up to date" - but it has to connect to GitHub to check. A browser window will open asking you to sign in. Complete the login.

!!! success "If you are not prompted, your credentials are already cached and you are good to go."

