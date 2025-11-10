# GitHub Template Setup

## Learning Objectives:

- Use GitHub template to create project repository
- Clone to VM and set up local environment
- Understand the project structure

---

## GitHub Template Introduction

- What is a template repository?
- Why use one? (structure, best practices, working CI/CD)

## Live Demo: Create from Template (10 mins)

- Navigate to template repo
- Click "Use this template"
- Create repository: library-pipeline
- Show the structure
- Explain what each folder is for

## Clone to VM & Setup (10 mins)


```bash
# Clone this repository
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO

# Create virtual environment
python -m venv venv
.\venv\Scripts\Activate.ps1

# Confirm the Python 3 version
python --version

# Install dependencies
pip install -r requirements.txt

# Run tests
pytest tests/ -v

# Run Python tests with a coverage report
pytest tests/ -v --cov=src --cov-report=term-missing
```
