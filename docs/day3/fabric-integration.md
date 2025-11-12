# Fabric Integration

## Learning Objectives

- Deploy Python package to Fabric
- Create Fabric notebook using the package
- Understand local dev → cloud deployment flow
- Build bronze → silver pipeline in Fabric

## Part 1: The Deployment Strategy

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

!!! success "Key insight"
    - We developed locally, but it runs in the cloud. This is the modern way!

## Part 2: Start Fabric Playground

1. Navigate to Fabric portal

2. Start playground (3-hour window begins)

3. Create a workspace

4. Create Lakehouse: "library_pipeline"

## Part 3: Upload Sample Data 

### In Fabric

1. Go to Lakehouse → Files

2. Under `Files` create a directory called `bronze`

3. Upload files to the bronze folder:

- `circulation_data.csv`
- `events_data.json`
- (Optional: other files)

4. Verify they appear in Files/bronze section

## Part 4: Create Fabric Notebook

Create a notebook and add these cells:


### Cell 1: Install package from GitHub

```python
# Cell 1: Install package from GitHub
%pip install "git+https://github.com/YOUR_USERNAME/library-pipeline.git"

```

### Cell 2: Import your functions

```python
# Cell 2: Import your functions
from data_processing.ingestion import load_csv, load_json
from data_processing.cleaning import (
    remove_duplicates, 
    handle_missing_values, 
    standardize_dates
)

print("✅ Package installed and imported successfully!")
```

### Cell 3: Load data from Lakehouse Files

```python
# Cell 3: Load data from Lakehouse Files
import pandas as pd

# Read CSV from Files
file_path = "/lakehouse/default/Files/bronze/circulation_data.csv"
df_raw = pd.read_csv(file_path)

print(f"Loaded {len(df_raw)} rows")
print(df_raw.head())
```

### Cell 4: Apply your cleaning functions (BRONZE → SILVER)

```python
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
```

### Cell 5: Save as Delta table (SILVER layer)

```python
# Cell 5: Save as Delta table (SILVER layer)
#from pyspark.sql import SparkSession
#spark = SparkSession.builder.getOrCreate()

# Convert pandas to Spark DataFrame
df_spark = spark.createDataFrame(df_clean)

# Write as Delta table
table_name = "silver_circulation"
df_spark.write.format("delta").mode("overwrite").saveAsTable(table_name)

print(f"✅ Created Delta table: {table_name}")
```

### Cell 6: Query the Delta table

```python
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

---

## Key points:
- ✅ Your local code works in Fabric (same package!)
- ✅ Lakehouse Files as data source
- ✅ Delta tables for structured storage
- ✅ Can query with SQL immediately
