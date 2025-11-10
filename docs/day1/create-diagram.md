# Architecture Design & Documentation

## Learning Objectives

- Design a medallion architecture for the project
- Create clear architecture diagrams
- Document key decisions

## Architecture Principles

Medallion pattern: Bronze → Silver → Gold

Why this pattern? (separation of concerns, reprocessing)

Where does each transformation happen?

Visual:

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

## Creating Architecture Diagrams

- Use draw.io (in browser, free)
- What to include: sources, layers, technologies
- Keep it simple!

### Live demo: Create a simple diagram