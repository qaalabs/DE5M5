# Build Your Docs Site

You've had `mkdocs` sitting in `requirements_dev.txt` since Day 1. Today you'll use it - and finally get your Day 1 architecture diagram rendering properly instead of sitting as a wall of Mermaid text.

!!! note "Your VM has reset overnight"
    Re-clone your repo and switch to `dev` as in [Day 4 Setup](40-setup.md) if you haven't already.


## Step 1: Get on `dev` and up to date

```
cd qa-library-pipeline
```

Make sure that your local repo has all the latest changes from GitHub:
```
git checkout dev
git pull
```


## Step 2: Create and activate a virtual environment

```
python -m venv venv
```
```
venv\Scripts\activate
```


## Step 3: Install the dev requirements

```
pip install -r requirements_dev.txt
```

!!! success "This installs `mkdocs`, `mkdocs-material`, and `mkdocs-glightbox` - already listed in your `requirements_dev.txt` since Day 1."


## Step 4: Fix `mkdocs.yml` to render Mermaid diagrams

Your repo was forked before this fix landed in the template, so you need to add it by hand.

1. Open `mkdocs.yml`.

2. Find the `pymdownx.superfences` line under `markdown_extensions` and replace it with:

    ```yaml
    - pymdownx.superfences:
        custom_fences:
          - name: mermaid
            class: mermaid
            format: !!python/name:pymdownx.superfences.fence_code_format
    ```

3. Add this to the bottom of the file:

    ```yaml
    extra_javascript:
      - https://unpkg.com/mermaid/dist/mermaid.min.js
    ```

    !!! note "Without this, a ` ```mermaid ` code block just prints as text instead of drawing the diagram."


## Step 5: Add your architecture diagram

1. Open `docs/architecture/design-pipeline.md`.

2. Paste in the Mermaid diagram you created on Day 1 (from your ADR or design notes), inside a fenced code block:

    ```` 
    ```mermaid
    flowchart LR
        ...
    ```
    ````

3. Save the file.


## Step 6: Preview the site

```
mkdocs serve
```

1. Open a browser to `http://127.0.0.1:8000`.

2. Navigate to **Architecture > Pipeline Design**.

    !!! success "Your diagram should render as an actual flowchart, not a code block."

3. Leave `mkdocs serve` running - it live-reloads as you edit.


## Step 7: Commit and push

```
git add .
git commit -m "Add docs site config and architecture diagram"
git push origin dev
```

---

## Optional: Merge into main

If you have time, open a pull request from `dev` into `main` on GitHub, same as Day 3, and merge once CI passes.
