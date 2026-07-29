## Production Readiness Yardstick

Three commands:
- `pytest --cov=src`
- `ruff check src/`
- `python -m data_processing.run_pipeline > report.txt`

Run at the start of the day to show the baseline 
- tests likely failing, coverage low, pipeline not cleaning properly

Run again after each session to show progress 
- this is the narrative spine of the day

`report.txt` gets committed so learners can see before/after in GitHub
