# Activity: First Code

- Open `src/data_processing/ingestion.py`
- Implement `load_csv()` function

Test it in Jupyter notebook:

```python
import sys
sys.path.append('/path/to/library-pipeline/src')
from data_processing.ingestion import load_csv
  
df = load_csv('data/circulation_data.csv')
print(df.head())
print(df.info())
```

Commit and push
