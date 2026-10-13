# Execution and Verification Log

Build date: 13 October 2026 (Asia/Singapore). This log records actual execution
and programmatic validation. Student review and platform submission remain pending.

## Software and scope

Python 3.12.14 was used with NumPy, SciPy, Pandas, Matplotlib, Jupyter/nbconvert,
ReportLab, pypdf, Poppler and Git. Tested direct package versions are pinned in
requirements.txt. Data are independent synthetic normal draws generated with
numpy.random.default_rng(); no real student records were used.

All five experiments are in analysis.ipynb. Fixed seeds are 42 (initial sample),
43 (sampling distribution), 44 (power), and 45 (counterexample). Parameters and
decision rules are stated before simulation. Reported results come from actual runs.

## Actual setup and code issues

- Existing Python installations lacked SciPy; the isolated project environment
  was completed with the required packages. Initial ensurepip setup failed,
  and pip's --python option successfully populated the environment.
- A restricted dependency download failed connecting to the configured proxy
  (WinError 10013). Subsequent installation succeeded. Later SSL/time-out retries
  also resolved without disabling certificate verification.
- Initial kernel registration failed in a temporary directory (WinError 5).
  Using a writable workspace temporary directory fixed registration.
- Restricted notebook execution stalled in Windows asyncio socket.accept while
  creating its local socket pair. A 15-second diagnostic established the cause;
  execution with local kernel communication available succeeded.
- The first numerical run reached the report cell, where an apostrophe in
  Student's caused an unterminated string. Corrected quoting and compilation of
  every code cell fixed this. A README path escape warning was also removed.
- Power error bars were changed to Wilson binomial intervals after one condition
  produced 500/500 rejections. A zero-width plug-in normal error bar would obscure
  finite Monte Carlo uncertainty. Simulated outcomes were not changed.
- An HTML export accidentally used a global Anaconda subcommand and failed in
  its certificate store. Invoking the project environment's nbconvert module
  directly fixed the export.
- Git initially treated the ASCII-encoded PDF as text. PDF and PNG files are
  marked binary in .gitattributes to preserve their bytes across checkouts.

## Commands actually executed

The commands below used the isolated Python environment. Kernel configuration,
temporary files and scratch checks were kept outside the delivered project.

| Command | Observed result |
|---|---|
| python -m pip check | No broken requirements found. |
| python -m pip install --no-index --dry-run -r requirements.txt | Every pinned direct requirement was already satisfied. |
| python -m jupyter nbconvert --execute --to notebook --inplace analysis.ipynb --ExecutePreprocessor.kernel_name=ds-a2 --ExecutePreprocessor.timeout=600 | The final statistical version completed twice in fresh kernels; 14 sequential code cells and no error outputs. |
| Independent saved-CSV verification and second-run hash comparison | All formulas, counts and decisions passed; exact CSV, PNG, README and notebook-output hashes matched before later documentation edits. Extracted PDF text matched. |
| pdftoppm -r 115 -png report.pdf page | Four rendered pages produced and visually inspected. |
| python -m nbconvert --to html analysis.ipynb | HTML export succeeded. |
| git diff --check | No text whitespace errors after binary attributes were added. |

The initial full reproducibility comparison predates repository-description edits.
The notebook retains its own numerical formula and CSV round-trip checks, so
essential verification can be rerun without a separate checking framework.
ReportLab invariant mode fixes metadata timestamps for reproducible output;
the actual build date is given above.

## Statistical checks actually performed

- Directly calculated initial Welch t, p, degrees of freedom and interval from
  sample variances; matched SciPy at rtol=1e-12.
- Verified theoretical SE=sqrt(15^2/100+15^2/100)=2.1213203436. The simulated
  sampling SD was 2.0818086583, allowing finite Monte Carlo variation.
- Checked the pooled or Welch SE, degrees of freedom and CI endpoints for all
  2,000 counterexample records against SciPy at rtol=1e-11. Independently
  reconstructed their p-values and decisions from saved CSV summaries.
- Reconstructed SE, df, t, p and decisions from the 6,000 saved power records.
- Compared every saved CSV with its in-memory table. Confirmed 200 initial
  scores, 1,000 sampling records, 6,000 power records (12 x 500), and
  2,000 counterexample records (1,000 paired experiments x 2 methods).
- Checked condition/replicate and method/replicate uniqueness, p-value ranges,
  and matching two-sided test/interval rejection-coverage complementarity.
- The zero-effect rejection count was 21/500 (4.2%); the exact 95% binomial
  interval was approximately [0.026, 0.063]. Outcomes were not adjusted.
- Confirmed five embedded notebook figures, five readable PNGs and four PDF
  pages. Reopened PDF text and checked key reported numerical results.
- Confirmed the submission summary contains 220 whitespace-delimited words.

Method reference: [SciPy ttest_ind documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html).
B is passed first, so estimates and intervals have the sign B minus A.

## Visual inspection

All four report pages were rendered and viewed, including all five chart
outputs. Labels, intervals, legends and reference lines were readable without
clipping. Grayscale inspection of the two multiseries figures confirmed that
hatching and text labels distinguish the groups without color alone.

The complete notebook page layout remains pending student review in Jupyter:
the available browser preview blocked local file URLs. Figure content was
visually checked through the report. No student or teacher review is claimed.

## Results

Observed difference: 5.594566605 points; 95% Welch CI
[1.895645432, 9.293487778]; p=0.003226404752; df=188.219458665.
The CI extends below the 5-point threshold; an improvement of at least 5 points
is not established. These simulated results cannot establish teaching effectiveness.

Student/Welch counterexample Type I error: 30.1% / 5.6%; coverage: 69.9% / 94.4%.
These are finite-simulation estimates, not universal guarantees.

## Repository and documentation update

[Zhoujie-SONG/DS-A2-Inference-Uncertainty](https://github.com/Zhoujie-SONG/DS-A2-Inference-Uncertainty)
is the public GitHub repository on branch main. It was initially created and
pushed on 13 October 2026. The GitHub API subsequently confirmed private=false
after its visibility was changed by the repository owner.

The repository URL and current visibility are synchronized in the README and
its notebook-generating text. The execution log is named VERIFICATION_LOG.md.
Documentation edits preserve the numerical experiment code and results.
The documentation version was executed in a fresh kernel successfully. Independent
formula checks passed again, and every CSV and figure PNG matched its pre-edit
SHA-256 hash exactly. The four-page PDF was reopened, its updated text and metadata
were checked, and the changed fourth page was rendered and visually inspected.

## Pending student actions

- Review the full notebook layout and explain its assumptions and results.
- Check course submission requirements and add any actual student validation.
- Submit the public repository URL and reviewed 220-word summary to the course
  platform before 14 October 2026, 16:30 Beijing/Singapore time.

No platform submission or teacher review is claimed.
