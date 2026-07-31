## Coverage Demo

- `python -m pytest --cov=src` shows percentage per file - aim is 70%+
- `python -m pytest --cov=src --cov-report=html` generates `htmlcov/index.html` - open in browser to see exactly which lines are not covered
- Coverage is a measure of trust, not quality - 70% good tests beats 90% meaningless ones
