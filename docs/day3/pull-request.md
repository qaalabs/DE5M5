# Activity: Complete Your Own PR Workflow

## Part 1: Create Feature Branch

### 1. Ensure you're on main and up to date:

```powershell
git checkout main
git pull
```

### 2. Create a new feature branch:

```powershell
git checkout -b feature/improve-cleaning
```

### 3. Make an improvement to your `cleaning.py`:
- Add a new function, OR
- Improve an existing function, OR
- Add better error handling

### 4. Write a test for your change
- Write a new test

## Part 2: Create Pull Request

### 1. Commit your changes:

```powershell
git add .
git commit -m "Improve data cleaning function"
```

### 2. Push your branch:

```powershell
git push origin feature/improve-cleaning
```

### 3. Go to GitHub → Create Pull Request:
- Base: main
- Compare: feature/improve-cleaning
- Add title and description
- Create PR

### 4. Watch CI/CD run on your PR
- Wait for tests to complete
- Check if they pass

## Part 3: Handle Test Results

If tests PASS ✅:

- Merge your PR
- Delete the branch
- Pull main locally
- Move to Part 4

If tests FAIL ❌:

- Read the error logs
- Fix the issue locally
- Commit and push
- Watch tests re-run
- Merge when green

## Part 4: Create Another PR

Practice again! Create a second feature branch:

- Add another function OR
- Improve documentation OR
- Add more tests

Go through the full PR workflow again.
