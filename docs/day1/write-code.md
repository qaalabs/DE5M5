# Activity: Write Your First Function

Open `src/data_processing/ingestion.py` in your editor.

Implement `load_csv()`:

```python
import pandas as pd
from pathlib import Path

def load_csv(filepath):
    filepath = Path(filepath)
    if not filepath.exists():
        raise FileNotFoundError(f"File not found: {filepath}")
    return pd.read_csv(filepath)
```

## Test it

Run the tests in Git Bash:

```sh
pytest tests/ -v
```

If the tests pass, commit your work - you're ready for Day 2.
