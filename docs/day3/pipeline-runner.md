# Activity: Run Your Package on New Data

!!! abstract "S24: Evaluate the strengths and weaknesses of prototype data products and how these integrate within an organisation's overarching data infrastructure."

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
pip install git+https://github.com/YOUR_USERNAME/YOUR_REPO.git
```

## 4. Run the pipeline

```powershell
python -m data_processing.run_pipeline
```

Cleaned files appear in `data/silver/`.

