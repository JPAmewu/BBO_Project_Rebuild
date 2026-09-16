# Week 3 Code Review Summary — Historical_Replay/Week_03

**Scope:** Consolidated, read-only code review of all 8
`week_03_gp_diagnostics.ipynb` notebooks, conducted by the Code Reviewer
Agent. No notebook, dataset, figure, or existing report was modified.
Findings were independently verified: best-observed/descriptive statistics
were recomputed from scratch directly from the raw `.npy` files, all
checksums were recomputed independently, and the convergence-aware
selection outcome for every function was cross-checked against each
notebook's own raw optimizer warning output — not taken on trust from the
notebooks' own printed summaries.

Full per-function detail is in each function's
`06_Code_Review/code_review_report.md`. This file summarizes findings
across notebooks.

**Severity labels:** Critical / Major / Minor / Informational.

## Cross-Function Totals

| Severity | Count | Notes |
|---|---|---|
| Critical | 0 | No data leakage, no fabricated results, no query actually generated/saved, no dataset or earlier-week file modified. |
| Major | 0 | No standardization bugs, no kernel-formula errors, no seed copy-paste errors, no convergence-rule misapplication, no overstated reliability for Function_05. |
| Minor | 0 | None found. |
| Informational | 3 | Cosmetic/stylistic observations only — see below. |

**Across all 17 checks × 8 functions (136 check-instances), zero findings
rose above Informational.**

## Checks Confirmed Clean Across All 8 Functions

- **Data provenance**: every notebook loads only its own
  `../02_Data/cumulative_inputs.npy`/`cumulative_outputs.npy` — no
  cross-function, cross-week, `Week_04`+, or source-project references as
  executable code (only expected markdown disclaimers stating their
  absence).
- **Sanity checks are real**: shape/dtype/dimensionality/row-count/
  NaN-Inf/bounds/duplicate checks are genuine `assert`-backed computations
  using correct function-specific constants (row counts 12, 12, 17, 32, 22,
  22, 32, 42; dimensions 2, 2, 3, 4, 4, 5, 6, 8).
- **Final-row identification**: every notebook correctly identifies index
  `n-1` as the newly-appended Week 2 observation, verified against
  `CUMULATIVE_DATA_VALIDATION.md` exactly for all 8 functions — including
  the Function_08 edge case where that same row also happens to be the
  best-observed point overall, handled correctly with no special-case bugs.
- **Descriptive statistics / best-observed**: independently recomputed
  argmax/mean/std from raw data matched each notebook's own printed output
  exactly, for all 8 functions.
- **Standardization**: `y` standardized before fitting and correctly
  inverse-transformed before every reported/plotted value in all 8
  notebooks — no double-transform bug found anywhere.
- **GP kernel construction**: byte-identical and correct in all 8 —
  `ConstantKernel(1.0,(1e-2,1e2)) * (RBF|Matern(nu=2.5, length_scale_bounds=(1e-2,1e2))) + WhiteKernel(noise_level=1e-2, noise_level_bounds=(1e-10,1e1))`,
  `GaussianProcessRegressor(alpha=1e-10, n_restarts_optimizer=5, normalize_y=False, random_state=SEED)`.
- **Seeds**: `SEED = 42 + function_number` correctly computed and printed
  per function (43–50) — no copy-paste errors across any of the 8
  notebooks.
- **Convergence-aware selection rule**: implemented programmatically (not
  hand-picked) in all 8, instrumenting all 6 restart attempts per kernel.
  Outcomes matched the claimed table exactly for every function:

  | Function | RBF ABNORMAL | Matérn ABNORMAL | Selected |
  |---|---|---|---|
  | 01 | 0/6 | 0/6 | RBF (higher LML) |
  | 02 | 0/6 | 0/6 | RBF (higher LML) |
  | 03 | 0/6 | 0/6 | RBF (higher LML) |
  | 04 | 0/6 | 0/6 | Matérn (higher LML) |
  | 05 | 3/6 | 5/6 | RBF (fewer ABNORMAL — **neither converged cleanly**) |
  | 06 | 0/6 | 0/6 | Matérn (higher LML) |
  | 07 | 0/6 | 0/6 | RBF (higher LML) |
  | 08 | 0/6 | 0/6 | Matérn (higher LML) |

- **Warnings never suppressed**: zero occurrences of
  `warnings.filterwarnings`/`simplefilter("ignore")` anywhere across all 8
  notebooks; raw ConvergenceWarning/ABNORMAL stderr text is genuinely
  visible in cell outputs and matches the reported counts exactly.
- **Function_05's instability described honestly**: a dedicated markdown
  cell explicitly states neither kernel converges cleanly this week (worse
  than Week 2), that the RBF selection is only "marginally more
  convergence-reliable," and that it is "not the same as a clean, fully
  reliable fit" — no overstatement of reliability found.
- **In-sample framing explicit and consistent**: every notebook states its
  observed-vs-predicted and residual diagnostics are in-sample only, with
  no claim of held-out or cross-validated predictive accuracy anywhere.
- **Dimensionality-appropriate diagnostics**: 2D scatter and 2D posterior
  contour plots present only for Function_01/02 (d=2); correctly omitted
  for d≥3 functions, confirmed both in notebook structure and by checking
  `04_Figures/` directory contents.
- **CI/slice plots use genuine GP posterior computations**: real
  `gpr.predict(..., return_std=True)` throughout; slice code correctly
  varies exactly one dimension while holding the rest at the
  best-observed point, identical and correct in all 8.
- **Figures are genuine**: all plotting cells pair real computed data with
  `fig.savefig(...)` calls; figure file sizes scale plausibly with
  dimensionality, consistent with genuinely rendered content.
- **Provenance citations correct**: every citation to
  `Historical_Replay/Week_02/Function_0X/pair_provenance.md` uses the
  correct 2-digit function number matching that notebook, and every cited
  file exists on disk — the Week 2 three-digit-citation bug class does not
  recur here.
- **`requirements-lock.txt` accurate**: independently queried actual
  installed package versions match the recorded file exactly (Python
  3.14.3, NumPy 2.5.2, SciPy 1.18.1, scikit-learn 1.9.0, Matplotlib 3.11.2,
  Seaborn 0.13.2, Jupyter 1.1.1, jupyter_core 5.9.1, nbformat 5.11.1,
  nbclient 0.11.0, nbconvert 7.17.1, ipykernel 7.3.0).
- **No dataset or earlier-week modification**: all 16 Week 3 cumulative
  files and all 16 Week 2 "prior" cumulative files independently
  re-verified via SHA-256 as unmodified; `Week_01`, `Historical_Replay/Week_01`,
  and `Historical_Replay/Week_02` mtimes confirm no post-hoc changes.

## Informational-Only Notes (Not Defects)

1. Section "## 7. Exploratory input-output figures" markdown numbering
   skips "7a" for functions with d>2 (Functions 03–08), jumping straight to
   "7b" — purely cosmetic, does not affect correctness, content, or any
   figure.
2. `requirements-lock.txt` and the actual environment both report
   forward-dated package versions (e.g. Python 3.14.3) — independently
   confirmed to match between claim and actual install, so this is not a
   discrepancy, just noted for anyone separately auditing environment
   plausibility.
3. Function_05's Section 9 intro boilerplate is identical generic framing
   to the other 7 notebooks; the dedicated Function_05-specific caveat cell
   immediately preceding it already supplies the necessary caution, so this
   is a stylistic opportunity for tighter cross-referencing, not a defect.

## Overall Verdict

**PASS.** All 8 `week_03_gp_diagnostics.ipynb` notebooks independently
satisfy all 17 required checks with no Critical, Major, or Minor findings.
Data provenance is clean; sanity checks, descriptive statistics, and
standardization are all correct; GP kernel construction and seeding are
correct and consistent across all 8; the convergence-aware selection rule
is applied programmatically and its outcomes match the claimed table
exactly, including Function_05's honestly-described instability; no
warning suppression exists anywhere; in-sample diagnostics are clearly
labeled with no overstated validation claims; figures are genuine; and no
dataset or earlier-week file was modified. No remediation is required at
this time; this report is findings-only, per its stated scope.
