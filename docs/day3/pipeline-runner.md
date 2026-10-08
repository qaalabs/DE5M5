# Activity: Run Your Package on New Data

!!! abstract "S24: Evaluate the strengths and weaknesses of prototype data products and how these integrate within an organisation's overarching data infrastructure."

A second library network has data that needs cleaning. Your package will do it.

You cloned `library-pipeline-runner` and installed its dependencies in the last activity. Carry on in the same Terminal window - it is already in the `library-pipeline-runner` folder.

## 1. Install your package

```powershell
pip install git+https://github.com/YOUR_USERNAME/YOUR_REPO.git
```

## 2. Run the pipeline

```powershell
python -m data_processing.run_pipeline
```

Cleaned files appear in `data/silver/`.
