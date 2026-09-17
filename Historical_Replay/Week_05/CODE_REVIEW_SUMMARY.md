# Code Review Summary — Week_05

**Scope:** Summarises the Code Reviewer Agent's independent, read-only
review of all 8 Week_05 GP diagnostics notebooks
(`Historical_Replay/Week_05/Function_0X/01_Notebook/week_05_gp_diagnostics.ipynb`),
using the same 17-point checklist applied to Week_03 and Week_04. Per-function
detail is in each function's own `06_Code_Review/code_review_report.md`.

## Overall Result

**PASS — all 8 functions.** No Critical or Major findings anywhere. One
Minor finding (missing `requirements-lock.txt` for Week_05, since fixed —
see below) and two Informational notes (no pre-existing `06_Code_Review`
folder prior to this review being written; Function_05's Section 8e
cross-week comparison correctly appears only in its own notebook).

## Per-Function Results

| Function | n (rows) | d | Seed | RBF ABNORMAL | Matérn ABNORMAL | Kernel selected | Result |
|---|---|---|---|---|---|---|---|
| Function_01 | 14 | 2 | 43 | 0/6 | 0/6 | RBF (LML tie-break) | **PASS** |
| Function_02 | 14 | 2 | 44 | 0/6 | 0/6 | RBF (LML tie-break) | **PASS** |
| Function_03 | 19 | 3 | 45 | 0/6 | 0/6 | RBF (LML tie-break) | **PASS** |
| Function_04 | 34 | 4 | 46 | 0/6 | 0/6 | Matérn (LML tie-break) | **PASS** |
| Function_05 | 24 | 4 | 47 | 4/6 | 4/6 | **Neither — both exploratory only** | **PASS** |
| Function_06 | 24 | 5 | 48 | 0/6 | 0/6 | Matérn (LML tie-break) | **PASS** |
| Function_07 | 34 | 6 | 49 | 0/6 | 0/6 | Matérn (LML tie-break, unchanged from Week_04) | **PASS** |
| Function_08 | 44 | 8 | 50 | 0/6 | 0/6 | Matérn (LML tie-break) | **PASS** |

## Notable Findings

- **Function_05 remains unresolved.** Both kernels showed 4/6 ABNORMAL
  L-BFGS-B terminations this week — equal counts, so per the
  convergence-aware rule neither kernel is selected as "the model"; both
  are reported as exploratory only. Compared to Week_04 (RBF 5/6, Matérn
  3/6), this is a genuinely mixed result: RBF improved by one restart,
  Matérn worsened by one restart. The notebook's own dynamic comparison
  (Section 8e) states this plainly as "a mixed picture ... the underlying
  instability for this function has not been cleanly resolved either way."
  This is the fourth consecutive week (Weeks 2-5) this function has shown
  genuine optimizer instability.
- **Function_07's kernel preference (Matérn, set in Week_04 after
  flipping from RBF in Week_03) held steady this week** — an ordinary
  LML tie-break outcome on two cleanly-converged fits, not an instability
  finding.
- **All other 6 functions converged cleanly for both kernels** (0/6
  ABNORMAL each), with kernel selection resolved purely by
  log-marginal-likelihood comparison.

## Minor Finding — Resolved

The review flagged that `Historical_Replay/Week_05/requirements-lock.txt`
was missing, unlike Week_02/03/04. This has been added, using the exact
same package versions as Week_04's lock file — independently confirmed via
direct `pip freeze` inspection of the Python 3.14.3 environment actually
used to execute these notebooks, showing the environment is byte-identical
to Week_04's (numpy 2.5.2, scipy 1.18.1, scikit-learn 1.9.0, matplotlib
3.11.2, seaborn 0.13.2, and the same Jupyter toolchain versions).

## Scope Confirmation

No notebook loads or references Week_04-or-earlier data files directly
(only that function's own Week_05 cumulative dataset), no notebook runs an
acquisition function or proposes/saves a query, no `Week_06` directory
exists anywhere in the project, and no Week_01/02/03/04 file was modified.
