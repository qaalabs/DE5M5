## Demo the Data Ingestion function

- Walk through `load_excel()` - it's already complete in the template, don't build it live. It's the model for what learners write next.
- Key pattern: check file exists, log what you're doing, wrap in try/except, log success with row count
- Run the pipeline (`python -m data_processing.run_pipeline`) and open `report.txt` - Raw vs Cleaned numbers match, since `load_csv`/`load_json` are still stubs and `cleaning.py` isn't implemented yet either. This is the baseline the day builds from.
- Do NOT live-code `load_csv`/`load_json` here - that's the activity right after (`INGEST-CODE` / `day2/ingestion-code.md`). If they see the finished versions in the demo, the activity is just copy-typing.
- Note: there's no dedicated test-writing activity for ingestion (unlike cleaning, which gets `TESTS-CODE` later) - `pytest` in the activity just confirms the functions work, it isn't teaching testing.

