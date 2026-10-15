# Build Your Docs Site

!!! abstract "S5: Produce and maintain technical documentation explaining the data product, that meets organisational, technical and non-technical user requirements, retaining critical information."

You've had `mkdocs` sitting in `requirements_dev.txt` since Day 1. Today you'll use it - and finally get your Day 1 architecture diagram rendering properly instead of sitting as a wall of Mermaid text.

!!! note "Your VM has reset, or your LOD session has expired"
    Re-clone your repo and switch to `dev` as in [VM / LOD Setup](lod-setup.md) if you haven't already.


## Step 1: Switch to `dev` and pull the latest changes

1. Open **Terminal** and navigate to your `qa-library-pipeline` folder:

    ```
    cd qa-library-pipeline
    ```

2. Switch to the `dev` branch and pull the latest changes from GitHub:

    ```
    git checkout dev
    git pull
    ```


## Step 2: Install the dev requirements

```
pip install -r requirements_dev.txt
```

!!! success "This installs `mkdocs`, `mkdocs-material`, and `mkdocs-glightbox` - already listed in your `requirements_dev.txt` since Day 1."


## Step 3: Check your architecture diagram

1. Open VS Code:

```
code .
```

!!! note "There's a space between `code` and `.`"
    The `.` means "this folder" - `code .` opens the current directory in VS Code.

2. Navigate to `docs/architecture/design-pipeline.md` and confirm that your Mermaid diagram from Day 1 is there.


## Step 4: Preview the site

```
python -m mkdocs serve
```

1. Open a browser and enter: `http://127.0.0.1:8000`

2. Navigate to **Architecture > Pipeline Design**.

    !!! success "Your diagram should render as an actual flowchart, not a code block."

3. Leave `python -m mkdocs serve` running - it live-reloads as you edit.


## Step 5: Change the site colours

1. Open `mkdocs.yml` and find the `palette` block under `theme:`.

2. Change `primary` and `accent` to a different colour, e.g. `teal`.

    !!! tip "Full list of colours and other theme options in the [mkdocs-material reference](https://squidfunk.github.io/mkdocs-material/reference/)"

3. Save the file and check your browser - `python -m mkdocs serve` should live-reload with the new colours.


## Step 6: Commit and push

```
git add .
git commit -m "Add docs site config and architecture diagram"
git push origin dev
```


## Step 7: Publish to GitHub Pages

```
python -m mkdocs gh-deploy
```

1. This builds the site and pushes it to a `gh-pages` branch on GitHub.

2. If this is the first time, check **Settings > Pages** on GitHub shows the source set to the `gh-pages` branch.

    !!! success "Your site is now live at `https://<your-org>.github.io/<your-repo>/` - it can take a few minutes to appear."

---

## Optional: Merge into main

If you have time, open a pull request from `dev` into `main` on GitHub, same as Day 3, and merge once CI passes.

