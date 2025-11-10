# The Scenario

**Newham Public Library Network** manages 15 branches across East London. 

They collect data from multiple sources:

### Circulation system (CSV exports):
- Book loans
- Returns
- Reservations

### Event management system (JSON API):
- Community events
- Attendance

### Member feedback (unstructured text files):
- Survey responses
- Complaints

### Digital catalogue (Excel files)
- Book metadata
- Acquisition records


## Current Problems:

- Data quality issues: duplicates, missing values, inconsistent formats
- Manual consolidation takes 2-3 days per month
- No automated quality checks

### Difficult to answer questions like:

- Which books are most popular by borough?
- What's our member retention rate?
- Which events drive library visits?

*Data analysts spend 60% of time cleaning, 40% analysing*

---

## Library's Request

**They want an automated data quality pipeline that:**

- Ingests data from multiple sources
- Cleans and validates data automatically
- Produces analysis-ready datasets
- Runs on a schedule
- Has quality monitoring and alerts

*They've heard that modern data engineering practices and CI/CD can help but don't know where to start.*

---

## Your Task

**Design and build a production-ready data quality pipeline that solves the library's problem.**

### Phase 1: Planning & Documentation (Day 1)

1. Analyse requirements and identify data quality issues

2. Design system architecture using medallion pattern:

    - Bronze: Raw data ingestion
    - Silver: Cleaned and validated data
    - Gold: Analysis-ready aggregations

3. Create documentation:

    - Architecture diagrams
    - Data flow documentation
    - ADRs (Architecture Decision Records)

4. Set up Kanban board for sprint planning

    - Initialise GitHub repository

### Phase 2: Development (Day 2)

1. Build Python package for data processing:

    - Extract data from CSV, JSON, Excel sources (S16)
    - Clean data using Pandas
    - Implement validation rules
    - Handle errors gracefully

2. Write unit tests for all functions

3. Document your code

### Phase 3: Pipeline & Automation (Day 3)

1. Build data pipeline in Microsoft Fabric:

    - Ingest from multiple sources
    - Apply your Python package for transformations
    - Implement bronze → silver → gold layers
    - Add data quality checks

2. Implement CI/CD:

    - Automated testing on Git push
    - Deploy through dev → test → prod
    - Use Fabric deployment pipelines OR Azure DevOps OR GitHub Actions

### Phase 4: Security & Presentation (Day 4 Morning)

1. Implement security controls:

    - Row-level security for branch-specific data
    - Access controls for sensitive member data
    - Document security practices

2. Create monitoring and alerting

3. Finalise documentation

### Phase 5: Presentation (Day 4 Afternoon)

1. Present your solution:

    - Business problem and requirements
    - Architecture design and decisions
    - Live demo of the working pipeline
    - Code quality and testing approach
    - CI/CD implementation
    - Security implementation
    - Challenges and learnings

## Provided Assets

You will receive:

### Sample datasets:

- `circulation_data.csv` (50,000 rows with quality issues)
- `events_data.json` (API responses, nested structure)
- `feedback.txt` (unstructured survey responses)
- `catalogue.xlsx` (book metadata with formatting issues)

### Other 

- Data dictionary describing expected schemas
- Business rules for validation
- Quality requirements (e.g., "circulation data must have < 1% missing ISBN")
