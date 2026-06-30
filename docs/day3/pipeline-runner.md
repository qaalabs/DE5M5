# Activity: Run Your Package on New Data

A second library network has data that needs cleaning. Your package will do it.

## 1. Clone the runner repo

```powershell
git clone https://github.com/QAADE5/library-pipeline-runner.git
cd library-pipeline-runner
```

## 2. Install dependencies

```powershell
pip install -r requirements.txt
```

## 3. Install your package

```powershell
pip install git+https://github.com/YOUR_ORG/YOUR_REPO.git
```

## 4. Run the pipeline

```powershell
python -m data_processing.run_pipeline
```

Cleaned files appear in `data/silver/`.

## 5. Load to SQL Server

```powershell
python load_to_sql.py
```

Open SSMS, connect to `localhost`, and explore the `library_warehouse` database.
