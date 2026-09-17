# Week 4 Code Review Summary — Historical_Replay/Week_04

**Scope:** Consolidated, read-only code review of all 8
`week_04_gp_diagnostics.ipynb` notebooks, conducted by the Code Reviewer
Agent. No notebook, dataset, figure, or existing report was modified.
Findings were independently verified: descriptive statistics were
recomputed from scratch directly from the raw `.npy` files, all checksums
were recomputed independently, installed package versions were queried
directly, and the convergence-aware selection outcome for every function
(including Function_05's flipped-kernel instability) was cross-checked
against each notebook's own raw optimizer warning output.

Full per-function detail is in each function's
`06_Code_Review/code_review_report.md`. This file summarizes findings
across notebooks.

**Severity labels:** Critical / Major / Minor / Informational.

## Cross-Function Totals

| Severity | Count | Notes |
|---|---|---|
| Critical | 0 | No data leakage, no fabricated results, no acquisition-driven query generated/saved, no dataset or earlier-week file modified. |
| Major | 0 | No standardization bugs, no kernel-formula errors, no seed copy-paste errors, no convergence-rule misapplication, no overstated reliability for Function_05, no warning suppression. |
| Minor | 0 | None found. |
| Informational | 1 recurring pattern (across Functions 03, 04, 05, 06, 07, 08) | Cosmetic section-numbering skip ("7a"/"9c" omitted without renumbering for d≥3 functions) — identical pattern already accepted as non-defect in the Week 3 review. |

**Across all 17 checks × 8 functions (136 check-instances), zero findings
rose above Informational.**

## Checks Confirmed Clean Across All 8 Functions

- **Data provenance**: every notebook loads only its own
  `../02_Data/cumulative_inputs.npy`/`cumulative_outputs.npy` — no
  cross-function, cross-week, `Week_05`+, or source-project references as
  executable code.
- **Sanity checks are real**: shape/dtype/dimensionality/row-count/
  NaN-Inf/bounds/duplicate checks are genuine `assert`-backed computations
  using correct function-specific constants (row counts 13, 13, 18, 33,
  23, 23, 33, 43; dimensions 2, 2, 3, 4, 4, 5, 6, 8).
- **Final-row identification**: every notebook correctly identifies index
  `n-1` as the newly-appended Week 3 observation, verified against
  `CUMULATIVE_DATA_VALIDATION.md` exactly for all 8 functions — including
  two edge cases (Function_04, Function_07) where that same row also
  happens to be the best-observed point overall, handled correctly with
  no special-case bugs.
- **Descriptive statistics / best-observed**: independently recomputed
  argmax/mean/std from raw data matched each notebook's own printed output
  exactly, for all 8 functions.
- **Standardization**: `y` standardized before fitting and correctly
  inverse-transformed before every reported/plotted value in all 8
  notebooks — no double-transform bug found anywhere.
- **GP kernel construction**: byte-identical and correct in all 8, same as
  Week 3.
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
  | 05 | 5/6 | 3/6 | Matérn (fewer ABNORMAL — **neither converged cleanly**) |
  | 06 | 0/6 | 0/6 | Matérn (higher LML) |
  | 07 | 0/6 | 0/6 | Matérn (higher LML — flipped from RBF in Week 3) |
  | 08 | 0/6 | 0/6 | Matérn (higher LML) |

- **Warnings never suppressed**: zero occurrences of
  `warnings.filterwarnings`/`simplefilter("ignore")` anywhere across all 8
  notebooks; raw stderr text for Function_05 contains exactly 5
  "ABNORMAL:" messages for RBF and exactly 3 for Matérn (total 8),
  matching the printed 5/6 and 3/6 counts exactly.
- **Function_05's Week 3-vs-Week 4 comparison is genuinely dynamic, not
  hardcoded**: the only hardcoded values are the fixed Week 3 baseline
  constants (`WEEK_03_RBF_ABNORMAL = 3`, `WEEK_03_MATERN_ABNORMAL = 5`,
  correctly matching Week 3's own recorded outcome), compared at runtime
  against the live Week 4 counts. The printed comparison ("RBF: WORSENED
  ... Matern: IMPROVED ... a mixed picture ... not been cleanly resolved
  either way") is honest and does not overstate the Matérn selection's
  reliability — a separate caveat cell explicitly states it is only
  "marginally more convergence-reliable," not "a clean, fully reliable
  fit."
- **In-sample framing explicit and consistent**: every notebook states its
  observed-vs-predicted and residual diagnostics are in-sample only, with
  no claim of held-out or cross-validated predictive accuracy anywhere.
- **Dimensionality-appropriate diagnostics**: 2D scatter and 2D posterior
  contour plots present only for Function_01/02 (d=2); correctly omitted
  for d≥3 functions.
- **CI/slice plots use genuine GP posterior computations**: real
  `gpr.predict(..., return_std=True)` throughout; slice code correctly
  varies exactly one dimension while holding the rest at the
  best-observed point, identical and correct in all 8.
- **Figures are genuine**: all plotting cells pair real computed data with
  `fig.savefig(...)` calls; figure sizes scale plausibly with
  dimensionality.
- **Provenance citations correct**: every citation to
  `Historical_Replay/Week_03/Function_0X/pair_provenance.md` uses the
  correct 2-digit function number matching that notebook, and every cited
  file exists on disk — the Week 2 three-digit-citation bug does not
  recur here.
- **`requirements-lock.txt` accurate**: independently queried actual
  installed package versions match the recorded file exactly, identical
  to Week 3's environment.
- **No dataset or earlier-week modification**: all 16 Week 4 cumulative
  files independently re-verified via SHA-256 as unmodified;
  `Week_01`, `Historical_Replay/Week_01`, `Historical_Replay/Week_02`, and
  `Historical_Replay/Week_03` mtimes confirm no post-hoc changes.

## Informational-Only Note (Not a Defect)

Section "## 7. Exploratory input-output figures" and Section 9 skip the
"7a"/"9c" subsection labels for functions with d>2 (the d=2-only 2D
scatter/contour subsections are correctly omitted, but the numbering
isn't renumbered) — purely cosmetic, does not affect correctness, content,
or any figure. Identical pattern already accepted as non-defect in the
Week 3 review.

## Overall Verdict

**PASS.** All 8 `week_04_gp_diagnostics.ipynb` notebooks independently
satisfy all 17 required checks with no Critical, Major, or Minor findings.
Data provenance is clean; sanity checks, descriptive statistics, and
standardization are all correct; GP kernel construction and seeding are
correct and consistent across all 8; the convergence-aware selection rule
is applied programmatically and its outcomes match the claimed table
exactly, including Function_05's honestly-described, still-unresolved
instability (kernel preference flipped, neither kernel converges cleanly);
no warning suppression exists anywhere; in-sample diagnostics are clearly
labeled with no overstated validation claims; figures are genuine; and no
dataset or earlier-week file was modified. No remediation is required at
this time; this report is findings-only, per its stated scope.
