# Activity: Design your Pipeline Architecture

Add a Mermaid diagram to your repository README showing your pipeline architecture.

## Include

- Your four data sources
- Bronze, Silver and Gold layers
- The flow between them

## Example structure

````markdown
```mermaid
flowchart TD
    CSV[circulation_data.csv] --> B[Bronze Layer]
    ...
```
````

!!! success "Keep it simple - sources in, layers through, outputs out. You can add detail later."
