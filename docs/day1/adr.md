# Architecture Decision Record

!!! abstract "S5: Produce and maintain technical documentation explaining the data product, that meets organisational, technical and non-technical user requirements, retaining critical information."

An ADR is a short document that captures an important decision, why you made it, and what the consequences are. One page, plain language.

---

## ADR-001: Pipeline Architecture

**Date:** {{ today }}

**Status:** Accepted

### Context

We need to process data from four sources (CSV, JSON, text, Excel) for Newham Public Library. The data has quality issues and needs to be cleaned before it can be used for analysis.

### Decision

We will use a medallion architecture with three layers:

- **Bronze** - raw data ingested exactly as received
- **Silver** - cleaned and validated data
- **Gold** - analysis-ready aggregations

### Reasons

- (write your own reasons here)

### Consequences

- Raw data is always preserved in bronze - we can reprocess if cleaning logic changes
- Silver is the trust boundary - gold always reads from silver, never bronze
- (add any other consequences you can think of)

---

*Write this in Notepad for now. You will commit it to your repository this afternoon.*
