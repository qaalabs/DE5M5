## Logging Activity

- Full activity: `day2/logging.md` - four checkpoints: switch logging on and log a load, log rows removed by cleaning, log a failure, then a stretch
- This is the only place logging is taught - the ingestion and cleaning blocks are logging-free, so learners arrive with working code and add logging on top
- Two ideas to land: modules write messages with `getLogger(__name__)`, the entry point (`run_pipeline.py`) switches logging on with `basicConfig`
- Lines to expect from the pipeline run: `Loaded 5100 rows`, `Removed 100 duplicate rows`, `Dropped 815 rows with missing values`
- Checkpoint 3: stress the bare `raise` - logging an error is not handling it, and this afternoon's tests still expect the original exception
- Checkpoint 4 (stretch) picks up the date loss from the cleaning activity: 423 `checkout_date` values in the pipeline run, or no warning if they already fixed the parsing
