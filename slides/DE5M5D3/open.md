### *title*

The aim of day 3 is ...

https://qaalabs.github.io/DE5M5/trainer

---

## **Module 5: Day 3 - Automation & Integration**

**Theme**: GitHub Actions CI/CD + Fabric Integration + Docker Awareness

---

### **9:30-9:50 | Welcome & Day Overview (20 mins)**
**Format**: Trainer-led (WebEx screen share)

**Content**:
- Welcome back! Check-in with 7 learners
- Day 2 recap: What we built
  - ✅ Complete Python package (ingestion + cleaning)
  - ✅ Comprehensive test suite
  - ✅ 70%+ coverage achieved
  - ✅ Everything tested locally
- Today's goals:
  - **Morning**: See CI/CD in action (GitHub Actions)
  - **Afternoon**: Integrate with Fabric (deploy our package)
  - **Brief**: Docker awareness (what it is, when to use it)
- **Key theme**: "From local development to automated deployment"

**Quick Standup** (10 mins):
Each learner shares:
- Tests all passing? ✅
- Coverage achieved? ✅
- Any remaining issues?
- What are you most excited/nervous about today?

**Artifacts Needed**:
- 📊 Slide deck: Day 3 overview
- 📊 Slide: Day 2 recap
- 📊 Slide: Modern data engineering workflow diagram

---

### **Session 1: 9:50-10:40 | GitHub Actions - Introduction (50 mins)**
**Format**: Trainer demo (25 mins) → Learner setup (25 mins)

**Learning Objectives**:
- Understand what CI/CD means for data products
- See GitHub Actions in action
- Set up automated testing workflow
- Understand the value of automation

**Trainer Demo** (25 mins):

**Part 1: What is CI/CD?** (5 mins)

**Visual - Show the problem**:
```
WITHOUT CI/CD:
Developer writes code → Forgets to run tests → Pushes to main → Production breaks 💥

WITH CI/CD:
Developer writes code → Pushes to GitHub → Tests run automatically → 
  ✅ Pass: Can deploy
  ❌ Fail: Can't deploy (you're protected!)
```

**Key points**:
- **CI** (Continuous Integration): Automatically test every change
- **CD** (Continuous Delivery/Deployment): Automatically deploy when tests pass
- **Why?** Catch bugs early, deploy confidently, collaborate safely

---

**Part 2: GitHub Actions Overview** (5 mins)

**What it is**:
- Built into GitHub
- Runs workflows on events (push, pull request, schedule)
- Free for public repos
- YAML configuration files

**Show the template workflow** (already in repo):
```yaml
# .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.9'
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
        pip install pytest pytest-cov
    
    - name: Run tests
      run: |
        pytest tests/ -v --cov=src --cov-report=term-missing
    
    - name: Check coverage threshold
      run: |
        pytest tests/ --cov=src --cov-report=term --cov-fail-under=70
```

**Explain each part**:
- `on: push/pull_request` - When it triggers
- `runs-on: ubuntu-latest` - Where it runs (GitHub's servers)
- `steps` - What it does (checkout, setup Python, install, test)

---

**Part 3: Live Demo - Trigger a Workflow** (15 mins)

**On screen share**:

1. **Show existing workflow**:
   - Navigate to repo → Actions tab
   - Show previous runs (if any)

2. **Make a small change**:
   ```bash
   # In ingestion.py - add a comment
   # Better logging for CSV loading
   
   git add src/data_processing/ingestion.py
   git commit -m "Add documentation comment"
   git push origin main
   ```

3. **Watch it run**:
   - Refresh Actions tab
   - Click on the running workflow
   - Expand steps - see real-time logs
   - Watch tests run
   - See it pass ✅

4. **Show a failure** (intentional):
   - Edit a test to fail:
   ```python
   def test_example():
       assert 1 == 2  # Intentional failure
   ```
   - Commit and push
   - Watch it fail ❌
   - Show the error logs
   - Fix it and push again
   - Watch it pass ✅

**Key teaching moment**:
> "See? We didn't run tests manually. GitHub ran them for us. If they failed, we'd know immediately. This is how teams work safely!"

---

**Learner Activity** (25 mins):

**Task: Trigger Your First Workflow** (20 mins)

**Instructions**:
```markdown
1. Verify workflow file exists in your repo:
   - Check `.github/workflows/ci.yml`
   - If missing, copy from template

2. Make a small change to trigger workflow:
   - Add a comment to any Python file
   - Or update your README

3. Commit and push:
   git add .
   git commit -m "Test CI/CD workflow"
   git push origin main

4. Watch GitHub Actions:
   - Go to your repo → Actions tab
   - Click on the running workflow
   - Watch it execute (takes 1-2 minutes)
   - Verify tests pass ✅

5. Explore the logs:
   - Click on "Run tests" step
   - See your test output
   - See coverage report
```

**Stretch goal**: Make a test fail intentionally, push, fix, push again

**Trainer Support**:
- Help with Git authentication
- Troubleshoot failing workflows
- Explain error messages
- Screen share to show examples

**Q&A Checkpoint**:
> - "Is your workflow running?"
> - "Show me the green checkmark on your commit"
> - "What did the workflow do?"
> - "Did all tests pass?"

**Artifacts Needed**:
- 📊 Slides: What is CI/CD?
- 📊 Slides: GitHub Actions overview
- 📊 Slides: Workflow YAML explained
- 📄 Quick reference: GitHub Actions commands
- 📄 Troubleshooting guide: Common workflow errors

---

### **10:40-11:00 | Break (20 mins)**

---

### **Session 2: 11:00-12:15 | GitHub Actions - Pull Request Workflow (75 mins)**
**Format**: Trainer demo (20 mins) → Learner practice (55 mins)

**Learning Objectives**:
- Understand feature branch workflow
- Create and use pull requests
- See CI/CD protect the main branch
- Experience real-world Git collaboration

**Trainer Demo** (20 mins):

**Part 1: Why Pull Requests?** (5 mins)

**Show the workflow**:
```
Feature Branch Workflow:
1. Create branch for new feature
2. Make changes
3. Create Pull Request (PR)
4. CI/CD runs automatically
5. If tests pass → Merge
6. If tests fail → Fix and try again
```

**Why this matters**:
- ✅ Main branch always has passing tests
- ✅ Review before merging
- ✅ Safe experimentation
- ✅ How real teams work

---

**Part 2: Live Demo - Complete PR Workflow** (15 mins)

**On screen share**:

1. **Create feature branch**:
   ```bash
   git checkout -b feature/add-validation
   ```

2. **Add a new function** (in `validation.py`):
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

3. **Write a test** (intentionally add a bug):
   ```python
   def test_validate_isbn():
       assert validate_isbn('978-0-123456-78-9') == True
       assert validate_isbn('invalid') == False
       assert validate_isbn('123') == False  # Too short
       assert validate_isbn('') == False
       assert validate_isbn(None) == True  # ❌ BUG! Should be False
   ```

4. **Commit and push**:
   ```bash
   git add src/data_processing/validation.py tests/test_validation.py
   git commit -m "Add ISBN validation function"
   git push origin feature/add-validation
   ```

5. **Create Pull Request on GitHub**:
   - Go to repo → Pull Requests → New PR
   - Select `feature/add-validation` → `main`
   - Title: "Add ISBN validation"
   - Description: "Implements ISBN-13 format validation"
   - Click "Create Pull Request"

6. **Watch CI/CD run on PR**:
   - See "Some checks haven't completed yet"
   - Wait for tests to run
   - Tests FAIL ❌ (because of the bug!)
   - Show the failed check on PR

7. **Fix the bug**:
   ```python
   # Fix the test
   assert validate_isbn(None) == False  # ✅ Fixed
   ```
   
   ```bash
   git add tests/test_validation.py
   git commit -m "Fix validation test for None input"
   git push origin feature/add-validation
   ```

8. **Watch PR update**:
   - Tests re-run automatically
   - All checks pass ✅
   - Green checkmark appears
   - "All checks have passed"

9. **Merge PR**:
   - Click "Merge pull request"
   - Confirm merge
   - Delete branch (optional)

10. **Show main branch**:
    - Go to main branch
    - New function is there
    - All tests still passing

**Key teaching moment**:
> "See how the PR protected us? We couldn't merge until tests passed. This is how you work safely in teams - main branch is always working!"

---

**Learner Activity** (55 mins):

**Task: Complete Your Own PR Workflow**

**Instructions**:
```markdown
## Part 1: Create Feature Branch (10 mins)

1. Ensure you're on main and up to date:
   git checkout main
   git pull

2. Create a new feature branch:
   git checkout -b feature/improve-cleaning

3. Make an improvement to your cleaning.py:
   - Add a new function, OR
   - Improve an existing function, OR
   - Add better error handling

4. Write a test for your change

## Part 2: Create Pull Request (15 mins)

1. Commit your changes:
   git add .
   git commit -m "Improve data cleaning function"
   
2. Push your branch:
   git push origin feature/improve-cleaning

3. Go to GitHub → Create Pull Request:
   - Base: main
   - Compare: feature/improve-cleaning
   - Add title and description
   - Create PR

4. Watch CI/CD run on your PR
   - Wait for tests to complete
   - Check if they pass

## Part 3: Handle Test Results (20 mins)

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

## Part 4: Create Another PR (10 mins)

Practice again! Create a second feature branch:
- Add another function OR
- Improve documentation OR
- Add more tests

Go through the full PR workflow again.
```

**Trainer Support**:
- Help with Git branch commands
- Review PRs (can leave comments!)
- Help debug failing tests
- Celebrate when PRs merge successfully

**Q&A Checkpoint**:
> - "Show me your merged PR"
> - "Did CI/CD catch any issues?"
> - "How many times did you need to push before tests passed?"
> - "Do you understand why this is useful?"
> - "Kanban updated?"

**Artifacts Needed**:
- 📊 Slides: Feature branch workflow
- 📊 Slides: Pull request anatomy
- 📄 Git branching cheat sheet
- 📄 PR workflow step-by-step guide
- 📄 Common Git commands reference

---

### **12:15-13:15 | Lunch (60 mins)**

---

### **Session 3: 13:15-14:30 | Fabric Integration (75 mins)**
**Format**: Trainer demo (30 mins) → Learner practice (45 mins)

**Learning Objectives**:
- Deploy Python package to Fabric
- Create Fabric notebook using the package
- Understand local dev → cloud deployment flow
- Build bronze → silver pipeline in Fabric

**Trainer Demo** (30 mins):

**Part 1: The Deployment Strategy** (5 mins)

**Show the flow**:
```
Local Development (VM):
  ↓
GitHub (Version Control):
  ↓
Fabric (Cloud Execution):
  - Installs package from GitHub
  - Runs transformation code
  - Creates Delta tables
```

**Key insight**:
> "We developed locally, but it runs in the cloud. This is the modern way!"

---

**Part 2: Start Fabric Playground** (5 mins)

**Live demo**:
1. Navigate to Fabric portal
2. Start playground (3-hour window begins)
3. Create or navigate to workspace
4. Create Lakehouse: "library_pipeline"

**Show structure**:
```
Lakehouse:
  ├── Files/ (upload raw data here)
  ├── Tables/ (Delta tables appear here)
  └── Notebooks/
```

---

**Part 3: Upload Sample Data** (5 mins)

**In Fabric**:
1. Go to Lakehouse → Files
2. Upload files:
   - `circulation_data.csv`
   - `events_data.json`
   - (Optional: other files)
3. Verify they appear in Files section

---

**Part 4: Create Fabric Notebook** (15 mins)

**Live coding in Fabric notebook**:

```python
# Cell 1: Install package from GitHub
%pip install git+https://github.com/YOUR_USERNAME/library-pipeline.git

# Cell 2: Import your functions
from data_processing.ingestion import load_csv, load_json
from data_processing.cleaning import (
    remove_duplicates, 
    handle_missing_values, 
    standardize_dates
)

print("✅ Package installed and imported successfully!")

# Cell 3: Load data from Lakehouse Files
import pandas as pd

# Read CSV from Files
file_path = "/lakehouse/default/Files/circulation_data.csv"
df_raw = pd.read_csv(file_path)

print(f"Loaded {len(df_raw)} rows")
print(df_raw.head())

# Cell 4: Apply your cleaning functions (BRONZE → SILVER)
print("Applying data cleaning pipeline...")

# Remove duplicates
df_clean = remove_duplicates(df_raw, subset=['transaction_id'])
print(f"After removing duplicates: {len(df_clean)} rows")

# Handle missing values
df_clean = handle_missing_values(df_clean, strategy='drop')
print(f"After handling missing values: {len(df_clean)} rows")

# Standardize dates
df_clean = standardize_dates(df_clean, ['checkout_date', 'return_date'])
print("Dates standardized")

print(f"\n✅ Cleaning complete! {len(df_raw)} → {len(df_clean)} rows")

# Cell 5: Save as Delta table (SILVER layer)
from pyspark.sql import SparkSession

# Convert pandas to Spark DataFrame
spark = SparkSession.builder.getOrCreate()
df_spark = spark.createDataFrame(df_clean)

# Write as Delta table
table_name = "silver_circulation"
df_spark.write.format("delta").mode("overwrite").saveAsTable(table_name)

print(f"✅ Created Delta table: {table_name}")

# Cell 6: Query the Delta table
query = f"""
SELECT 
    COUNT(*) as total_transactions,
    COUNT(DISTINCT member_id) as unique_members,
    COUNT(DISTINCT isbn) as unique_books,
    COUNT(DISTINCT branch_id) as branches
FROM {table_name}
"""

result = spark.sql(query)
result.show()

print("✅ Silver layer ready for analysis!")
```

**Key teaching points**:
- ✅ Your local code works in Fabric (same package!)
- ✅ Lakehouse Files as data source
- ✅ Delta tables for structured storage
- ✅ Can query with SQL immediately

---

**Learner Activity** (45 mins):

**Task: Deploy Your Package to Fabric**

**Instructions**:
```markdown
## Part 1: Fabric Setup (10 mins)

1. Start your Fabric playground (if not already running)

2. Create Lakehouse:
   - Name: "library_pipeline"
   - Note the workspace name

3. Upload sample data to Files:
   - circulation_data.csv
   - (Optional: other data files)

## Part 2: Create Notebook (25 mins)

1. Create new Fabric notebook: "01_bronze_to_silver"

2. Install your package:
   %pip install git+https://github.com/YOUR_USERNAME/library-pipeline.git

3. Import and test:
   from data_processing.cleaning import remove_duplicates
   print("✅ Import successful!")

4. Build bronze → silver pipeline:
   - Load data from Files
   - Apply your cleaning functions
   - Save as Delta table "silver_circulation"

5. Query your table:
   SELECT * FROM silver_circulation LIMIT 10

## Part 3: Verify Results (10 mins)

1. Check Tables section:
   - silver_circulation should appear

2. Query the table:
   - Count rows
   - Check data types
   - View sample data

3. Compare with local results:
   - Same number of rows?
   - Same cleaning applied?
```

**Trainer Support**:
- Help with Fabric authentication
- Troubleshoot package installation
- Debug path issues
- Help with Spark DataFrame conversion
- Screen share examples

**Q&A Checkpoint**:
> - "Is your package installed in Fabric?"
> - "Show me your Delta table"
> - "How many rows in silver vs bronze?"
> - "Can you query your table?"
> - "Does it match your local results?"

**Artifacts Needed**:
- 📊 Slides: Local dev → Cloud deployment flow
- 📊 Slides: Fabric Lakehouse structure
- 💾 Sample Fabric notebook (complete reference)
- 📄 Fabric deployment guide
- 📄 Common Fabric issues and solutions

---

### **Session 4: 14:50-15:50 | Docker Awareness + Prep for Tomorrow (60 mins)**
**Format**: Trainer demo (30 mins) → Learner work (30 mins)

**Learning Objectives**:
- Understand what Docker is and when to use it
- See Docker in action (demo only, not hands-on)
- Understand Docker vs Fabric notebooks
- Prepare for Day 4 presentations

**Trainer Demo - Docker Awareness** (30 mins):

**Part 1: What is Docker?** (10 mins)

**Visual explanation**:
```
WITHOUT DOCKER:
"It works on my machine!" 
↓
Different Python versions
Different library versions
Different OS
= Problems! 💥

WITH DOCKER:
Package everything together:
  ├── Your code
  ├── Python 3.9
  ├── All dependencies
  └── OS environment
= Works everywhere! ✅
```

**Key concept**:
> "Docker is like a shipping container for software. Everything your code needs is packaged together."

**When you'd use Docker**:
- ✅ Deploying to Kubernetes
- ✅ Microservices architecture
- ✅ Running on different machines/clouds
- ✅ Isolating environments

**When you DON'T need Docker**:
- ❌ Fabric notebooks (Fabric manages the environment)
- ❌ Simple Python scripts
- ❌ When PaaS handles it for you

---

**Part 2: Docker Demo** (15 mins)

**IF Docker is installed on VM**:

**Live demo**:
```bash
# Show a simple Dockerfile
cat Dockerfile

# Content:
# FROM python:3.9-slim
# WORKDIR /app
# COPY requirements.txt .
# RUN pip install -r requirements.txt
# COPY src/ ./src/
# CMD ["python", "-m", "src.data_processing"]

# Build image
docker build -t library-pipeline:v1 .

# Show images
docker images

# Run container
docker run library-pipeline:v1

# Explain what happened
```

**Key points**:
- Image = Template (like a class)
- Container = Running instance (like an object)
- Portable across environments

---

**IF Docker is NOT installed**:

**Conceptual explanation with visuals**:
- Show Dockerfile structure
- Show DockerHub (pre-built images)
- Explain when companies use Docker
- Show architecture diagram

**Time**: 15 mins

---

**Part 3: Docker vs Fabric** (5 mins)

**Comparison**:
```
DOCKER:
- You manage everything
- Portable to any platform
- More control
- More complexity

FABRIC NOTEBOOKS:
- Fabric manages environment
- Tied to Fabric platform
- Less control
- Much simpler
- Perfect for data pipelines

FOR OUR PROJECT: Fabric notebooks are the right choice! ✅
```

---

**Learner Activity - Prep for Tomorrow** (30 mins):

**Task: Finalize Your Project**

**Checklist**:
```markdown
## Code Quality
- [ ] All tests passing locally
- [ ] 70%+ test coverage
- [ ] GitHub Actions passing
- [ ] Code committed and pushed

## Fabric Deployment
- [ ] Package installs in Fabric
- [ ] Bronze → Silver pipeline working
- [ ] Delta table created successfully
- [ ] Can query results

## Documentation
- [ ] README.md complete
- [ ] Architecture diagram in docs/
- [ ] Functions have docstrings
- [ ] ADRs documented

## Presentation Prep
- [ ] Think about what to demo
- [ ] Identify challenges you faced
- [ ] Note key learnings
- [ ] What would you do differently?

## GitHub Kanban
- [ ] Most tasks in "Done"
- [ ] Document any remaining work
- [ ] Update status
```

**Trainer Circulates**:
- Do final checks of everyone's work
- Help fix any remaining issues
- Answer questions about presentations
- Provide encouragement

**Q&A Checkpoint**:
> - "Are you ready to present tomorrow?"
> - "What's your biggest accomplishment?"
> - "What was most challenging?"
> - "Any blockers for tomorrow?"

**Artifacts Needed**:
- 📊 Slides: What is Docker?
- 📊 Slides: When to use Docker vs Fabric
- 📊 Slides: Docker architecture diagram
- 📄 Dockerfile example (for reference)
- 📄 Day 4 presentation checklist

---

### **15:50-16:00 | Day 3 Wrap-Up (10 mins)**
**Format**: Trainer-led

**Content**:

**What We Achieved Today**:
- ✅ GitHub Actions CI/CD working (automated testing!)
- ✅ Pull request workflow mastered
- ✅ Python package deployed to Fabric
- ✅ Bronze → Silver pipeline in production
- ✅ Understanding of Docker concepts

**Celebration Moment**:
> "You've built a production-ready data product with automated testing and cloud deployment. This is real-world data engineering!"

**Tomorrow (Day 4)**:
- Morning: Polish and final testing
- Afternoon: Presentations (15 mins each)
- Be ready to demo your working pipeline
- Discuss challenges and learnings

**Homework** (Optional):
- Practice your presentation
- Test your demo environment
- Prepare for questions
- Review what you've learned

**Final Reminder**:
> "Tomorrow is about showing what you've built and what you've learned. Be proud of your work - you've come a long way!"

**Artifacts Needed**:
- 📊 Slides: Day 3 accomplishments
- 📊 Slides: Day 4 preview

---

## Day 3 Artifacts Summary

### **Slides/Presentations** 📊
- [ ] Day 3 overview and Day 2 recap
- [ ] Modern data engineering workflow diagram
- [ ] What is CI/CD?
- [ ] GitHub Actions overview
- [ ] Workflow YAML explained
- [ ] Feature branch workflow
- [ ] Pull request anatomy
- [ ] Local dev → Cloud deployment flow
- [ ] Fabric Lakehouse structure
- [ ] What is Docker?
- [ ] When to use Docker vs Fabric
- [ ] Docker architecture diagram
- [ ] Day 3 accomplishments and Day 4 preview

### **Code/Reference Implementations** 💾
- [ ] Sample Fabric notebook (complete pipeline)
- [ ] Dockerfile example (for reference)

### **Documents/Guides** 📄
- [ ] GitHub Actions quick reference
- [ ] Troubleshooting guide: GitHub Actions errors
- [ ] Git branching cheat sheet
- [ ] PR workflow step-by-step guide
- [ ] Common Git commands reference
- [ ] Fabric deployment guide
- [ ] Common Fabric issues and solutions
- [ ] Day 4 presentation checklist

---

## Key Pedagogical Elements in Day 3

### **1. Seeing Automation in Action**
- Not just theory - watch CI/CD work
- Real-time feedback (tests pass/fail)
- "Aha!" moment when PR blocks bad code

### **2. Professional Workflows**
- Feature branches
- Pull requests
- Code review process (you can comment on their PRs!)
- This is how real teams work

### **3. Local → Cloud Bridge**
- Code developed locally
- Tested automatically
- Deployed to cloud
- Complete modern workflow

### **4. Appropriate Tool Selection**
- Docker: Awareness, not expertise
- Fabric: Right tool for data pipelines
- GitHub Actions: Industry standard CI/CD

---