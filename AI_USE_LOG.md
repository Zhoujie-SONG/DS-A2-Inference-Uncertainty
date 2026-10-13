# AI Use and Verification Log

Build date: 13 October 2026 (Asia/Singapore). This records actual assistance and
execution in the Codex desktop workspace. It does not claim student review,
teacher approval, or validation by a separate human or AI reviewer.

## Tool and assistance

OpenAI Codex (this desktop chat) drafted the Python notebook, English explanations,
plots, PDF, README and submission summary; executed the simulations; diagnosed
environment and code failures; and performed programmatic and visual checks.
NumPy, SciPy, Pandas, Matplotlib, Jupyter/nbconvert, ReportLab, pypdf, Poppler and
Git were the supporting software. No subagents were used. The user supplied the
research topic, hypotheses, minimum effect, seeds and experiment grids.

The AI generated synthetic data with `numpy.random.default_rng()`. No real student
records were accessed. All reported results are from actual execution.

## Suggestions adopted and not adopted

Adopted: a single notebook with five sections; Welch as the principal two-sided
test; direct formulas checked against SciPy; separate fixed streams for sampling,
power and counterexample; one simple power function; per-replicate CSV records;
paired Student/Welch counterexample samples; and a four-page ReportLab report.

During review, the AI replaced approximate symmetric Monte Carlo error bars in
the power plot with 95% Wilson binomial intervals. One condition yielded 500/500
rejections; a plug-in normal error bar at that boundary has zero width. The
replacement shows finite simulation uncertainty without changing any data.

No actual external AI suggestions were presented and rejected by the student.
No such rejection is claimed. Features excluded by the task (web apps, Docker,
GPU models and complex frameworks) were not implemented; this is scope compliance,
not an invented suggestion-rejection history.

## Environment and actual issues

1. Both existing Python installations lacked SciPy. The bundled Python also lacked
   Matplotlib and Jupyter execution components. An isolated environment was placed
   under `work/.venv`, outside the delivered project.
2. The initial bundled `python -m venv --system-site-packages work/.venv` failed in
   `ensurepip`. The environment executable still existed. The bundled pip's
   `--python work/.venv/Scripts/python.exe` option installed the required packages
   into that environment; pip itself was subsequently installed there.
3. The first restricted pip attempt could not connect to the configured local proxy
   (WinError 10013) and failed. The approved unrestricted dependency installation
   succeeded. A later pip/JupyterLab installation had transient SSL/time-out retries
   and ultimately succeeded. No certificate verification was disabled.
4. Initial `ipykernel install` failed writing a sandbox temporary directory
   (WinError 5). Setting TEMP/TMP to `work/tmp` fixed registration. The kernel was
   registered at `work/kernel_env/share/jupyter/kernels/ds-a2`; JUPYTER_PATH was set
   to that prefix. The kernelspec points explicitly to `work/.venv/Scripts/python.exe`.
5. Restricted nbconvert commands stalled before notebook execution. A bounded
   faulthandler diagnostic timed out after 15 seconds and showed Windows asyncio
   stuck in `socket.accept` while creating its local socket pair. The stalled
   commands were cancelled. Approved execution outside that restriction enabled
   the fresh Jupyter kernel. This is an environment problem, not a fabricated
   notebook failure.
6. The first actual notebook execution completed the numerical experiments but
   failed in the report cell: an apostrophe in `Student's` produced an unterminated
   string. The generated cell was corrected to use double quotes; all code cells
   were compiled before subsequent executions. An invalid-backslash warning in
   the README-generating string was removed by using a forward-slash activation path.
7. The corrected notebook completed. A visual/statistical review then found the
   boundary-error-bar issue described above. The final revision was executed twice
   from fresh kernels with unchanged parameters; both final executions succeeded.
8. A later HTML refresh using `python -m jupyter nbconvert` without prepending the
   environment's Scripts directory dispatched Anaconda's global jupyter-nbconvert,
   which failed in its Windows certificate store with ASN1 NOT_ENOUGH_DATA.
   Running `python -m nbconvert` directly in the project environment fixed this.
   The README activates the environment before its jupyter command.
9. The first staged diff treated ReportLab's ASCII-encoded PDF as text, reporting
   whitespace and possible Windows newline conversion. Added `.gitattributes`
   marking PDF/PNG files binary and restaged the PDF. This preserves PDF byte
   offsets on checkout; the final staged whitespace check passed.

Nonfatal messages: Windows ZMQ used a selector helper thread; the local kernel
reported TCP transport without encryption; nbconvert warned that the five embedded
images lack alternative-text metadata. Plots have descriptive visible titles,
axes, legends and nearby interpretation. Poppler printed missing Symbol/ArialUnicode
display-font messages, but rendered all four pages successfully; the report uses
Helvetica text and ASCII formulas and the inspected pages had no missing glyphs.

## Actual commands and verification

The build used Python 3.12.14. Tested direct versions are pinned in requirements.txt:
NumPy 2.5.4, SciPy 1.18.1, Pandas 3.0.6, Matplotlib 3.11.2,
nbformat 5.11.1, nbclient 0.11.0, nbconvert 7.17.2, ipykernel 7.4.0,
JupyterLab 4.6.4, ReportLab 5.0.1, pypdf 6.20.1.

In the command records below, `<workspace>` denotes the actual directory
`C:/Users/Icarus/Documents/Codex/2026-10-13/files-pasted-by-the-user-codex`.
This is a compact path notation, not an instruction to type a placeholder literally.
Execution used `<workspace>/work/.venv/Scripts/python.exe`. TEMP/TMP,
IPYTHONDIR, MPLCONFIGDIR, JUPYTER_RUNTIME_DIR and JUPYTER_PATH were directed to
workspace subdirectories; Scripts was prepended to PATH for the jupyter command.

| Actual command (arguments preserved; paths abbreviated) | Observed result |
|---|---|
| `python -m pip check` in the isolated environment | No broken requirements found. |
| `python -m pip install --no-index --dry-run -r outputs/DS-A2-Inference-Uncertainty/requirements.txt` from the workspace | Every pinned direct requirement was already satisfied. |
| `python <workspace>/work/build_notebook.py` | nbformat validation and compilation of all 14 code cells passed; 22 cells created. |
| `python -m jupyter nbconvert --execute --to notebook --inplace analysis.ipynb --ExecutePreprocessor.kernel_name=ds-a2 --ExecutePreprocessor.timeout=600` from the project | The unchanged final notebook succeeded twice, writing 344,453 bytes each time. All 14 code cells have sequential execution counts; no error outputs. |
| `python <workspace>/work/verify_project.py` followed after the rerun by `python <workspace>/work/verify_project.py --compare` | All numeric/structure checks passed; exact CSV, PNG, README and notebook-output content hashes matched. Extracted PDF text matched. Notebook execution timestamps were intentionally excluded from the content comparison. |
| `pdftoppm -r 115 -png <project>/report.pdf <workspace>/work/pdf_preview/page` | Four PNG pages produced. All pages visually inspected, with the revised power page inspected again. |
| `python -m nbconvert --to html <project>/analysis.ipynb --output analysis --output-dir work` | HTML exported successfully. |
| `git init -b main` and `git remote -v` | Standalone repository initialized; no remote configured. Final commit status is given in the delivery message and can be checked with `git log -1`. |

The independent saved-data verification script is a build-time scratch script under
work, not a dependency of the delivered notebook. The notebook preserves its own
formula checks and CSV round-trip checks so the essential verification is rerunnable.
ReportLab's invariant mode fixes PDF metadata timestamps for deterministic output;
the actual build date is recorded at the top of this log.

### Statistical checks actually executed

- Computed the initial Welch t, p, Satterthwaite degrees of freedom and interval
  directly from the sample variances, then compared with SciPy at rtol=1e-12.
- Checked the theoretical SE identity: sqrt(15^2/100+15^2/100) equals
  15*sqrt(2/100), giving 2.1213203436. Sampling SD was 2.0818086583;
  these need not agree exactly after 1,000 repetitions.
- For all 2,000 counterexample test/CI records, independently calculated pooled
  or Welch standard error, degrees of freedom and interval endpoints and checked
  against SciPy (rtol=1e-11). The external verification also recomputed these from
  saved CSV summaries, including p-values and decisions.
- For all 6,000 power records, the external check independently reconstructed
  standard errors, degrees of freedom, t statistics and two-sided p-values from
  the saved SDs and sample sizes and reconciled rejection indicators.
- Read every CSV back and compared all values with the in-memory experiment
  tables. Confirmed 200 initial score records, 1,000 sampling records, 6,000 power
  records (12 conditions x 500), and 2,000 counterexample records (1,000 x 2).
  Confirmed uniqueness of condition/replicate and method/replicate keys.
- Under zero effect, 21/500 tests rejected (4.2%). The exact 95% binomial interval
  was approximately [0.026, 0.063], containing the nominal 0.05. No data were
  adjusted to make this happen. Rejection/coverage complementarity also passed.
- Confirmed five notebook PNG outputs, five readable PNG files and four PDF pages.
  Reopened the PDF with pypdf and verified readable text and key numerical results.
- Checked the README submission summary against executed results and confirmed
  220 whitespace-delimited English words.

Method interpretation was checked against the official
[SciPy ttest_ind documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html):
`equal_var=False` selects Welch, and its result exposes df and confidence_interval.
B is passed first so differences and intervals refer to B minus A.

### Visual review and remaining preview limitation

All four PDF pages were rendered and actually viewed. Their five figures include
all notebook chart content. Titles, units, reference lines, legends, rate labels,
intervals and the final power revision were inspected for readability and clipping.
A grayscale rendering of the two multiseries figures was viewed: hatching and
text labels retained the A/B and Student/Welch distinctions without color.

The HTML notebook export exists as a scratch preview, but the browser tool blocked
the local `file:` URL by policy. No alternate browser route was attempted. A Codex
file-preview request was queued, which is not proof of model-side visual inspection.
The complete notebook page layout therefore remains pending human review in
Jupyter; the notebook's actual figures were visually checked through the PDF.

## Final results and human checks

Initial observed difference: 5.594566605 points; 95% Welch CI
[1.895645432, 9.293487778]; p=0.003226404752; df=188.219458665.
The interval includes the 5-point threshold, so a positive difference is supported
in the model, but an improvement of at least 5 points is not established.

Student/Welch counterexample Type I error: 30.1% / 5.6%; interval coverage:
69.9% / 94.4%. These are finite-simulation estimates, not universal guarantees.

Pending student actions at the initial build (GitHub setup was completed in the follow-up below):

- Personally review the notebook's complete rendered layout and explain the
  methods, assumptions and results in their own words.
- Verify the course's AI-use rules and add any actual student checks to this log.
- Create or select an actual GitHub repository, add its real remote URL, push,
  and inspect the uploaded files. No GitHub URL, remote commit or tag is claimed.
- Submit the actual project/repository URL and reviewed 220-word summary to
  learn.spaiq.ai before 14 October 2026, 16:30 Beijing/Singapore time.

No manual student review, teacher approval or platform submission has occurred
as part of this automated build.


## GitHub follow-up — 13 October 2026

The user explicitly requested creation of a remote repository and a GitHub push.
The GitHub connector was authenticated as szjhuho-maker, while Git Credential
Manager was authenticated as Zhoujie-SONG, matching the local Git author setup.
The latter account was used for this Git operation; this choice was stated to
the user before creation. Credentials were kept in memory and were not printed
or written to project files.

The GitHub REST API confirmed that the requested repository name was available
and created `Zhoujie-SONG/DS-A2-Inference-Uncertainty` as a private, empty repository
(HTTP 201). The returned repository URL was
https://github.com/Zhoujie-SONG/DS-A2-Inference-Uncertainty.

Actual commands:

```bash
git remote add origin https://github.com/Zhoujie-SONG/DS-A2-Inference-Uncertainty.git
git push -u origin main
```

Both succeeded. The initial assignment commit `a9f9877` was uploaded and main now
tracks origin/main. README and its notebook-generating text were updated to use
the actual repository URL and clone instructions; the notebook's numerical code
and saved experiment outputs were preserved. Its format and all code-cell syntax
were checked after this documentation-only update. The resulting documentation
commit is visible in Git history.

GitHub setup is complete. Human review, course-policy verification and platform
submission remain student actions. A course reviewer will need access to this
private repository. No platform submission or teacher review is claimed.
