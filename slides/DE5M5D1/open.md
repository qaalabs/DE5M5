### *Module Introduction & Setup*

The aim of day 1 is ...

https://qaalabs.github.io/DE5M5/trainer

---

## **Module 5: Day 1 - Foundation & Setup**

**Theme**: Planning, documentation, and local development environment setup

---

### **9:30-9:50 | Welcome & Day Overview (20 mins)**
**Format**: Trainer-led (WebEx screen share)

**Content**:
- Welcome to Module 5: Building Data Products
- Quick recap: Module 3 (ETL in Fabric), Module 4 (Planning/Agile)
- Today's goal: Set up for success
- Week overview: Build → Test → Deploy → Present

**Artifacts Needed**:
- 📊 Slide deck: Module 5 overview

---

### **Session 1: 9:50-10:40 | Project Introduction (50 mins)**
**Format**: Trainer-led with discussion

**Learning Objectives**:
- Understand the library data quality problem
- Identify what "production-ready" means
- Understand the assessment criteria

**Content** (30 mins):
1. **The Scenario** (10 mins)
   - Newham Public Library network problem
   - Current manual process (2-3 days/month)
   - Data quality issues across multiple sources
   - What they want: automated pipeline

2. **Your Mission** (10 mins)
   - Build a production-ready data quality pipeline
   - Local dev → GitHub → CI/CD → Fabric
   - Not just "make it work" - make it **maintainable**

3. **What "Production-Ready" Means** (10 mins)
   - ✅ Tested code (>70% coverage)
   - ✅ Version controlled
   - ✅ Documented
   - ✅ Automated deployment
   - ✅ Error handling
   - ✅ Security considered

**Activity** (15 mins):
- Review the sample data files (show them)
- Discuss: What quality issues do you see?
- Brainstorm: What would a solution look like?

**Q&A** (5 mins)

**Artifacts Needed**:
- 📊 Slides: Project scenario
- 📊 Slides: Assessment rubric
- 📄 Project brief document (written)
- 💾 Sample data files (to distribute)

---

### **10:40-11:00 | Break (20 mins)**

---

### **Session 2: 11:00-12:15 | Architecture Design & Documentation (75 mins)**
**Format**: Trainer demo (20 mins) → Learner work (55 mins)

**Learning Objectives**:
- Design a medallion architecture for the project
- Create clear architecture diagrams
- Document key decisions

**Trainer Demo** (20 mins):
1. **Architecture Principles** (10 mins)
   - Medallion pattern: Bronze → Silver → Gold
   - Why this pattern? (separation of concerns, reprocessing)
   - Where does each transformation happen?
   
   **Visual**:
   ```
   Sources (CSV, JSON, Excel)
        ↓
   BRONZE (Raw ingestion)
        ↓
   SILVER (Cleaned, validated) ← Your Python package does this
        ↓
   GOLD (Business aggregations)
        ↓
   Consumption (Reports, analysis)
   ```

2. **Creating Architecture Diagrams** (10 mins)
   - Use draw.io (in browser, free)
   - What to include: sources, layers, technologies
   - Keep it simple!
   - **Live demo**: Create a simple diagram

**Learner Activity** (50 mins):
- Open draw.io or Excalidraw
- Design your pipeline architecture
- Include:
  - Data sources
  - Bronze/Silver/Gold layers
  - Technologies (Python, Pandas, Fabric, GitHub Actions)
  - Data flow arrows
- Save as PNG or PDF
- **Keep it simple!** Don't over-engineer

**Trainer Circulates** (via breakout rooms or individual check-ins):
- Are diagrams clear and readable?
- Have they thought about error handling?
- Is the scope realistic for 4 days?

**Wrap-Up Q&A** (5 mins):
- Show 2-3 good examples
- Common patterns
- "Tomorrow you'll start building this!"

**Artifacts Needed**:
- 📊 Slides: Medallion architecture explanation
- 📊 Slides: Architecture diagram examples (good and bad)
- 🔗 Link to draw.io
- 📄 Architecture Decision Record (ADR) template

---

### **12:15-13:15 | Lunch (60 mins)**

---

### **Session 3: 13:15-14:30 | GitHub Template Setup (75 mins)**
**Format**: Trainer demo (25 mins) → Learner work (50 mins)

**Learning Objectives**:
- Use GitHub template to create project repository
- Clone to VM and set up local environment
- Understand the project structure

**Trainer Demo** (25 mins):
1. **GitHub Template Introduction** (5 mins)
   - What is a template repository?
   - Why use one? (structure, best practices, working CI/CD)

2. **Live Demo: Create from Template** (10 mins)
   - Navigate to template repo
   - Click "Use this template"
   - Create repository: `library-pipeline`
   - Show the structure
   - Explain what each folder is for

3. **Clone to VM & Setup** (10 mins)
   ```bash
   # Open terminal on VM
   cd ~/projects
   git clone https://github.com/username/library-pipeline.git
   cd library-pipeline
   
   # Create conda environment
   conda create -n library-pipeline python=3.9
   conda activate library-pipeline
   
   # Install dependencies
   pip install -r requirements.txt
   
   # Verify setup
   pytest tests/test_example.py -v
   ```

**Learner Activity** (45 mins):
1. **Create Repository** (10 mins)
   - Go to template URL
   - "Use this template" → Create repo
   - Name it: `library-pipeline`

2. **Clone to VM** (15 mins)
   - Open terminal
   - Clone repository
   - Navigate into folder
   - Explore the structure

3. **Environment Setup** (15 mins)
   - Create conda environment
   - Install dependencies
   - Run example test
   - Verify it works

4. **First Commit** (5 mins)
   - Upload architecture diagram to `docs/architecture/`
   - Commit and push
   - Verify on GitHub

**Trainer Support**:
- Help with Git authentication issues
- Conda environment problems
- Path issues on VM

**Q&A Checkpoint** (5 mins):
> - "Is your test passing?"
> - "Can you see your repo on GitHub?"
> - "Have you committed your architecture diagram?"

**Artifacts Needed**:
- 🔗 GitHub template repository (pre-built)
- 📊 Slides: GitHub template walkthrough
- 📄 Setup instructions document
- 📄 Troubleshooting guide (common VM issues)

---

### **Session 4: 14:50-15:50 | Kanban Planning & First Code (60 mins)**
**Format**: Trainer demo (15 mins) → Learner work (45 mins)

**Learning Objectives**:
- Set up Kanban board for project tracking
- Break project into manageable tasks
- Write first lines of code

**Trainer Demo** (15 mins):
1. **Kanban Introduction** (5 mins)
   - Why track work?
   - GitHub Projects (integrated with repo)
   - Columns: To Do, In Progress, Testing, Done

2. **Task Breakdown Demo** (10 mins)
   - Show sample board
   - Example tasks:
     - "Build CSV ingestion function"
     - "Write tests for cleaning module"
     - "Set up GitHub Actions"
   - Size tasks: 2-4 hours each
   - Add to board

**Learner Activity Part 1: Kanban** (20 mins)
- Set up GitHub Projects board
- Create columns: To Do, In Progress, Testing, Done
- Create task cards for all work:
  - Data ingestion (CSV, JSON, Excel)
  - Data cleaning functions
  - Data validation
  - Unit tests
  - GitHub Actions setup
  - Fabric deployment prep
  - Presentation prep
- Prioritize tasks

**Learner Activity Part 2: First Code** (20 mins)
- Open `src/data_processing/ingestion.py`
- Implement `load_csv()` function
- Test it in Jupyter notebook:
  ```python
  import sys
  sys.path.append('/path/to/library-pipeline/src')
  from data_processing.ingestion import load_csv
  
  df = load_csv('data/circulation_data.csv')
  print(df.head())
  print(df.info())
  ```
- Commit and push

**Trainer Support**:
- GitHub Projects setup
- Python import path issues
- First Git workflow

**Q&A Checkpoint** (5 mins):
> - "Show me your Kanban board"
> - "Have you moved 'Set up Kanban' to Done?"
> - "Did your first function work?"
> - "Have you pushed to GitHub?"

**Artifacts Needed**:
- 📊 Slides: Kanban principles
- 📊 Slides: Example Kanban board
- 📄 Sample task breakdown list

---


