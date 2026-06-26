# Creating Architecture Diagrams with Mermaid

Mermaid is built into GitHub - any `.md` file in a repo renders Mermaid diagrams automatically. No tools to install.

## Basic syntax

````markdown
```mermaid
flowchart TD
    A[Source] --> B[Bronze]
    B --> C[Silver]
    C --> D[Gold]
```
````

## Demo: Library pipeline

Show this rendering in the repo README:

````markdown
```mermaid
flowchart TD
    CSV[circulation_data.csv] --> B[Bronze Layer]
    JSON[events_data.json] --> B
    TXT[feedback.txt] --> B
    XLS[catalogue.xlsx] --> B

    B --> S[Silver Layer\nCleaned & Validated]
    S --> G[Gold Layer\nAnalysis-Ready]

    G --> R[Reports & Dashboards]
```
````

## Key points

- `TD` = top-down flow. Use `LR` for left-right if it fits better
- Square brackets `[]` for process steps, `()` for rounded, `{}` for decisions
- Edit in the repo, preview renders on GitHub instantly
- They'll add this to their README during the DESIGN activity
