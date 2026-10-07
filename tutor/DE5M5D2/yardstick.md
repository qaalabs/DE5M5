## Run commands to see code "production ready" status

Three commands:
- `python -m pytest --cov=src`
- `python -m ruff check src/`
- `python -m data_processing.run_pipeline`

Run at the start of the day to show the baseline 
- tests likely failing, coverage low, pipeline not cleaning properly

Run again after each session to show progress 
- this is the narrative spine of the day

`report.txt` and `pytest_result.txt` gets committed so trainer can see the status
