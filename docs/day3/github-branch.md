# Activity: Pull Request Workflow

## Learning Objectives
- Understand feature branch workflow
- Create and use pull requests
- See CI/CD protect the main branch
- Experience real-world Git collaboration

## Part 1: Why Pull Requests?

**Feature Branch Workflow:**

1. Create branch for new feature
2. Make changes
3. Create Pull Request (PR)
4. CI/CD runs automatically
5. If tests pass → Merge
6. If tests fail → Fix and try again

### Why this matters

- ✅ Main branch always has passing tests
- ✅ Review before merging
- ✅ Safe experimentation
- ✅ How real teams work

---

## Part 2: Complete PR Workflow

### 1. Create feature branch

```bash
git checkout -b feature/add-validation
```

### Add a new function (in `validation.py`)

```python
def validate_isbn(isbn):
    """Validate ISBN-13 format.
       
    Args:
        isbn (str): ISBN string to validate
           
    Returns:
        bool: True if valid, False otherwise
    """
    if not isbn:
        return False
       
    # Remove hyphens
    isbn = isbn.replace('-', '')
       
    # Check length
    if len(isbn) != 13:
        return False
       
    # Check if all digits
    if not isbn.isdigit():
        return False
       
    return True
```

### 3. Write a test (intentionally add a bug):

```python
def test_validate_isbn():
    assert validate_isbn('978-0-123456-78-9') == True
    assert validate_isbn('invalid') == False
    assert validate_isbn('123') == False  # Too short
    assert validate_isbn('') == False
    assert validate_isbn(None) == True  # ❌ BUG! Should be False
```

### 4. Commit and push

```bash
git add src/data_processing/validation.py tests/test_validation.py
git commit -m "Add ISBN validation function"
git push origin feature/add-validation
```

### 5. Create Pull Request on GitHub
- Go to repo → Pull Requests → New PR
- Select `feature/add-validation` → `main`
- Title: "Add ISBN validation"
- Description: "Implements ISBN-13 format validation"
- Click "Create Pull Request"

### 6. Watch CI/CD run on PR
- See "Some checks haven't completed yet"
- Wait for tests to run
- Tests FAIL ❌ (because of the bug!)
- Show the failed check on PR

### 7. Fix the bug

```python
# Fix the test
assert validate_isbn(None) == False  # ✅ Fixed
```
   
```bash
git add tests/test_validation.py
git commit -m "Fix validation test for None input"
git push origin feature/add-validation
```

### 8. Watch PR update
- Tests re-run automatically
- All checks pass ✅
- Green checkmark appears
- "All checks have passed"

### 9. Merge PR
- Click "Merge pull request"
- Confirm merge
- Delete branch (optional)

### 10. Show main branch
- Go to main branch
- New function is there
- All tests still passing

!!! success "See how the PR protected us?"
    - We couldn't merge until tests passed.
    - This is how you work safely in teams
    - The main branch always has working code!
