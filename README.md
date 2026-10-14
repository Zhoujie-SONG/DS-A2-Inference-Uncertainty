# DS-A2 | Inference and Uncertainty Notebook

Compare two teaching methods using simulated exam scores. The notebook covers
sampling distributions, Welch inference, power and an unequal-variance counterexample.
These are synthetic data, not evidence of real teaching effectiveness.

## Run
Use Python 3.12 from this project directory:

```bash
python -m venv .venv
./.venv/Scripts/Activate.ps1
# macOS/Linux: source .venv/bin/activate
python -m pip install -r requirements.txt
python -m ipykernel install --user --name ds-a2 --display-name "DS-A2"
python -m jupyter nbconvert --execute --to notebook --inplace analysis.ipynb --ExecutePreprocessor.kernel_name=ds-a2 --ExecutePreprocessor.timeout=600
```

Or open `analysis.ipynb` in JupyterLab and run all cells. This recreates the CSVs,
figures, three-page `report.pdf` and this README.

## Results
- Difference B - A: **5.595 points**; 95% Welch CI
  **[1.896, 9.293]**; p=**0.003226**.
- Power at n=20/50/100/200 per group:
  16.2%, 36.8%, 62.0%, 92.8%.
- Zero-effect rejection rate: 4.2% (Type I error).
- Student/Welch counterexample false positives:
  30.1%/5.6%; coverage:
  69.9%/94.4%.

The interval extends below the 5-point meaningful-improvement threshold.
Finite Monte Carlo precision and the independent normal model limit generalization.

## Files
- `analysis.ipynb`: all five experiments, explanations and formula checks.
- `report.pdf`: short report with key figures.
- `requirements.txt`: tested dependencies.
- `results/`: initial scores, per-replicate CSVs and five PNG figures.

Public repository: [DS-A2-Inference-Uncertainty](https://github.com/Zhoujie-SONG/DS-A2-Inference-Uncertainty).
Method reference: [SciPy ttest_ind](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html).

## Submission Summary
This notebook compares mean exam scores under traditional and new teaching methods using simulated normal data. The estimand is mean B minus mean A, with a two-sided null of zero difference, alpha 0.05, and a meaningful improvement of 5 points. With 100 observations per group, the estimated difference was 5.595 points. The Welch test gave p=0.003226 and a 95% interval [1.896, 9.293]. This indicates a nonzero simulated difference, but does not establish an improvement of at least 5 points. Across 1,000 samples, the estimate averaged 4.979, with SD 2.082. Power simulations used 500 repetitions per condition. More observations, lower noise and larger effects improved detection; zero-effect rejection was 4.2%. In 1,000 unequal-variance, unequal-size counterexamples, Student/Welch false-positive rates were 30.1%/5.6%, and interval coverage was 69.9%/94.4%. All seeds were fixed, and interval formulas were checked against SciPy. Welch reduced the pooled-test problem. Finite simulation precision, independence and unbounded scores limit generalization. These results cannot establish real teaching effectiveness.

Word count: 156.
