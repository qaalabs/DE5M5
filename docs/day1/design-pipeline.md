# Activity: Design your Pipeline Architecture

Work as a group to create a Mermaid diagram of your pipeline architecture. Your trainer will share a HedgeDoc link for your group.

## Include

- Your four data sources
- Bronze, Silver and Gold layers
- The flow between them

## Example structure

````markdown
```mermaid
flowchart LR
    Source[Source data] --> Bronze[Bronze layer]
    Bronze --> Silver[Silver layer]
    Silver --> Gold[Gold layer]
```
````

!!! success "Keep it simple - sources in, layers through, outputs out. You can add detail later."
