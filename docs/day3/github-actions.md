# GitHub Actions

## Learning Objectives
- Understand what CI/CD means for data products
- See GitHub Actions in action
- Set up automated testing workflow
- Understand the value of automation

## Part 1: What is CI/CD?

```
WITHOUT CI/CD:
Developer writes code → Forgets to run tests → Pushes to main → Production breaks 💥

WITH CI/CD:
Developer writes code → Pushes to GitHub → Tests run automatically → 
  ✅ Pass: Can deploy
  ❌ Fail: Can't deploy (you're protected!)
```

### Key points
- **CI** (Continuous Integration): Automatically test every change
- **CD** (Continuous Delivery/Deployment): Automatically deploy when tests pass
- **Why?** Catch bugs early, deploy confidently, collaborate safely

## Part 2: GitHub Actions Overview

### What it is
- Built into GitHub
- Runs workflows on events (push, pull request, schedule)
- Free for public repos
- YAML configuration files

### GitHub File

Show the **template workflow** (already in repo): `.github/workflows/ci.txt`

- `on: push/pull_request` - When it triggers
- `runs-on: ubuntu-latest` - Where it runs (GitHub's servers)
- `steps` - What it does (checkout, setup Python, install, test)

**Rename the file to**: `.github/workflows/ci.yml`

**Change the coverage failure limit to 90%**

## Part 3: Live Demo - Trigger a Workflow

### 1. Show existing workflow
- Navigate to repo → Actions tab
- Show previous runs (if any)

### 2. Make a small change

```bash
# In ingestion.py - add a comment
# Better logging for CSV loading
   
git add src/data_processing/ingestion.py
git commit -m "Add documentation comment"
git push origin main
```

### 3. Watch it fail
- Refresh Actions tab
- Click on the running workflow
- Expand steps - see real-time logs
- Watch tests run
- Watch it fail ❌
- Show the error logs

### 4. Show success
- Edit `ci.yml` to a low coverage
- Commit and push
- Watch it pass ✅

!!! success "How this works:"
    - We didn't run tests manually.
    - GitHub ran them for us.
    - If they failed, we'd know immediately.
    - This is how teams work safely!
