## Ingestion Activity

- Full activity: `day2/ingestion-code.md`
- Learners implement `load_csv()` (easier) then `load_json()` (harder - needs `pd.json_normalize()` to flatten nested fields) in `ingestion.py`, following the `load_excel()` pattern from the demo
- Encourage them to check the raw data files first (Part 1) before writing code - they should know what they're handling
- The JSON REPL check (`pd.json_normalize()` on `events_data.json`) is worth doing live if anyone's stuck on flattening
- Commit via VS Code Source Control when done
- No dedicated test-writing here - `pytest` just confirms the functions work
