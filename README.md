# DS-A2 | Inference and Uncertainty Notebook

## Goal and scope
**Understanding Statistical Inference and Uncertainty through A/B Testing**

Compare traditional and new teaching using simulated exam scores. This is a methods
demonstration, not real student data or evidence of teaching effectiveness. All report,
table and chart values come from executed simulations. The five parts in analysis.ipynb
cover a prespecified question, sampling distributions, Welch inference, power, and an
unequal-variance counterexample.

## Install and run
Python 3.12 was used for validation. From this project directory:

```bash
python -m venv .venv
# Windows PowerShell:
./.venv/Scripts/Activate.ps1
# macOS/Linux: source .venv/bin/activate
python -m pip install -r requirements.txt
python -m jupyter nbconvert --execute --to notebook --inplace analysis.ipynb --ExecutePreprocessor.timeout=600
```

The python3 kernel must use the activated environment. If an existing kernel points
elsewhere, register this environment explicitly and select it:

```bash
python -m ipykernel install --user --name ds-a2 --display-name "DS-A2"
python -m jupyter nbconvert --execute --to notebook --inplace analysis.ipynb --ExecutePreprocessor.kernel_name=ds-a2 --ExecutePreprocessor.timeout=600
```

For interactive reading: `python -m jupyterlab analysis.ipynb` (JupyterLab is included
in requirements). Running every cell recreates all numerical results, figures,
report.pdf and README.md in the current project directory.
No API, GPU, internet data source or external PDF compiler is required after installation.

## Files
```text
DS-A2-Inference-Uncertainty/
  analysis.ipynb
  README.md
  requirements.txt
  report.pdf
  .gitignore
  results/
    sample_data.csv              # 200 initial individual scores
    simulation_results.csv      # 1,000 independent sampling experiments
    power_results.csv           # 6,000 actual experiments across 12 conditions
    counterexample_results.csv  # 2,000 test/CI records; 1,000 paired experiments
    figures/                    # five PNGs, including three power panels
```

CSV files contain full-precision values. Initial scores are preserved individually;
repeated simulations preserve each replicate's sample summaries and, where relevant,
test statistics, p-values, decisions and confidence intervals. They are not just
hand-written aggregate summaries. Replicate identifiers restart within each condition;
use condition columns with replicate as the key.

## Methods and main results
Estimand: population mean B minus mean A. H0: zero; H1: nonzero; alpha=0.05;
meaningful positive improvement=5 points. Baseline population settings are means
70 and 75, SD=15, and n=100 per group. Seeds: 42 (initial), 43 (sampling), 44 (power),
45 (counterexample). Sample SDs use ddof=1. Welch's SE and Satterthwaite degrees of
freedom are calculated directly and verified against SciPy; see Part 3 for formulas.

- Sample means: A=69.246, B=74.841.
- Difference=5.595; 95% Welch CI=[1.896, 9.293];
  p=0.003226; df=188.219.
- Sampling mean=4.979, empirical SD=2.082, theoretical SE=2.121.
- Power at n=20/50/100/200 per group: 16.2%, 36.8%, 62.0%, 92.8%.
- Power at SD=5/10/15/20: 100.0%, 92.8%, 68.0%, 45.0%.
- Rejection at effect=0/2/5/8: 4.2%, 14.4%, 62.8%, 96.6%.
  The effect=0 rate is Type I error, not power against a true alternative.
- Counterexample: n_A=25, SD_A=20; n_B=100, SD_B=5; both means=70.
  Student Type I error=30.1%, coverage=69.9%;
  Welch Type I error=5.6%, coverage=94.4%.

## Validation and limitations
The notebook checks theoretical SE, manual/SciPy t, p, df and CI agreement,
all counterexample CI formulas, CSV round trips, repetition counts, p-value ranges,
and the test/coverage identity. The exact binomial interval for the null-condition
rejection rate is [0.0262, 0.0635]. This is
a calibration diagnostic, not a requirement to manufacture a 5% rejection rate.
Clean-kernel execution and file-hash reproducibility checks were completed.

All observations are independent normal draws. Welch does not fix dependence or
selection bias. Scores are unbounded; real exams may be bounded or nonnormal.
Monte Carlo rates have finite precision (500 or 1,000 repeats). Statistical significance
does not establish an improvement of at least 5 points. No real educational conclusion
can be drawn. Student must personally review the explanations and course submission requirements.

## GitHub repository
[Zhoujie-SONG/DS-A2-Inference-Uncertainty](https://github.com/Zhoujie-SONG/DS-A2-Inference-Uncertainty)
is the public remote repository. The project is on the `main` branch.

To obtain a copy:

```bash
git clone https://github.com/Zhoujie-SONG/DS-A2-Inference-Uncertainty.git
cd DS-A2-Inference-Uncertainty
```

Share the public repository URL on the course platform. No tag was created.

## Submission Summary
This project asks whether traditional and new teaching methods differ in population mean exam scores. It uses synthetic normal data, not a real teaching experiment. Before sampling, I specified the estimand as mean B minus mean A, a two-sided null of zero difference, alpha 0.05, and a meaningful improvement of 5 points. With 100 observations per group and seed 42, the observed difference was 5.595 points. The Welch test gave p=0.003226 and a 95% confidence interval [1.896, 9.293]. This supports a nonzero simulated difference but does not establish an improvement of at least 5 points. Across 1,000 repeated samples, the estimate had mean 4.979 and SD 2.082, compared with theoretical SE 2.121. Power simulations used 500 repetitions per condition. Increasing sample size from 20 to 200 per group increased rejection rates from 16.2% to 92.8%; greater noise reduced power and larger effects increased it. The zero-effect rejection rate was 4.2%, representing Type I error. In 1,000 equal-mean counterexamples with unequal variances and sample sizes, Student and Welch Type I error rates were 30.1% and 5.6%; their 95% interval coverage rates were 69.9% and 94.4%. The pooled variance assumption made Student inference unreliable here. Finite Monte Carlo precision, independence assumptions and unbounded normal scores limit generalization. These results demonstrate statistical methods and cannot show that a real teaching method is effective.

Word count (whitespace-delimited): 220.

## Reference and verification
[SciPy ttest_ind documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html)
documents the equal_var switch, df and confidence_interval method. The notebook passes
B first so the estimate and interval have the sign B minus A. The notebook
includes direct formula checks and CSV consistency checks.
