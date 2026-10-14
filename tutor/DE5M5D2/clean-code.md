## Cleaning Activity

- Full activity: `day2/cleaning-code.md` - four checkpoints, one per function, then a stretch; learners post "1 done" etc. in the chat and carry straight on
- Checks run in Jupyter Notebook (`notebooks/sandbox.ipynb`) against the circulation data - help anyone who cannot get Jupyter started or hits `ModuleNotFoundError`
- Row counts to expect: 5100 raw, 5024 with no subset, 5000 on `transaction_id`, 4185 after `strategy='drop'`
- Checkpoint 3 is the point to land: the demo's `standardise_dates()` silently turns 500 `MM/DD/YYYY` checkout dates into `NaT` - runs clean, loses data
- Checkpoint 4 (stretch) fixes it with `format='mixed'`; the branch whitespace task (30 values, 15 branches) is for anyone finished early
- Worth a minute in the debrief: `drop` removed 815 loans, 501 with no `return_date` - those books are still out on loan
