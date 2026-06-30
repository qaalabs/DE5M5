## Linting Demo

- Run `ruff check src/` live - show silence as the passing state
- Add `import os` to cleaning.py, run again, show the F401 error, delete and run again - silence returns
- Explain the error codes: F401 unused import, E711 wrong None comparison
- Show `ruff format --check src/` briefly - not a gate, just awareness
- Mention flake8 as context - ruff is the modern replacement
