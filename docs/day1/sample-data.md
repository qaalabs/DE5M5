# Library Pipeline: Sample Data

## Sample Data

This folder contains sample data for the library pipeline project.

- GitHub Link: https://github.com/ingwanelabs/library-pipeline-template/tree/main/data

## Data Files

1. **circulation_data.csv** (50,000 rows)
   - Transaction records from book checkouts
   - Contains quality issues: duplicates, missing values, date format inconsistencies

2. **events_data.json** (500 events)
   - Library events and attendance records
   - Nested JSON structure requiring flattening

3. **feedback.txt** (200 feedback entries)
   - Unstructured member feedback
   - Text parsing required

4. **catalogue.xlsx** (5,000 books)
   - Book catalogue with acquisition data
   - Multiple sheets, type inconsistencies


## Data Quality Issues

These files intentionally contain data quality issues:

- Duplicate records
- Missing values
- Inconsistent date formats
- Nested structures requiring flattening

**Your task is to build a pipeline that cleans and validates this data.**
