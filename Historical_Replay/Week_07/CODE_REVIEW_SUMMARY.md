# Code Review Summary — Week_07

**Scope:** Summarises the Code Reviewer Agent's independent, read-only
review of all 8 Week_07 GP diagnostics notebooks
(`Historical_Replay/Week_07/Function_0X/01_Notebook/week_07_gp_diagnostics.ipynb`),
using the same 17-point checklist applied to Week_05 and Week_06. Per-function
detail is in each function's own `06_Code_Review/code_review_report.md`.

## Overall Result

**PASS — all 8 functions.** No Critical or Major findings anywhere. One
Minor finding (missing `requirements-lock.txt` for Week_07, since
fixed — see below).

## Per-Function Results

| Function | n (rows) | d | Seed | RBF ABNORMAL | Matérn ABNORMAL | Kernel selected | Result |
|---|---|---|---|---|---|---|---|
| Function_01 | 16 | 2 | 43 | 0/6 | 0/6 | RBF (LML tie-break) | **PASS** |
| Function_02 | 16 | 2 | 44 | 0/6 | 0/6 | RBF (LML tie-break) | **PASS** |
| Function_03 | 21 | 3 | 45 | 0/6 | 0/6 | RBF (LML tie-break) | **PASS** |
| Function_04 | 36 | 4 | 46 | 0/6 | 0/6 | Matérn (LML tie-break) | **PASS** |
| Function_05 | 26 | 4 | 47 | 5/6 | 4/6 | **Matérn (fewer-ABNORMAL rule — worse on both kernels than Week_06)** | **PASS** |
| Function_06 | 26 | 5 | 48 | 0/6 | 0/6 | Matérn (LML tie-break) | **PASS** |
| Function_07 | 36 | 6 | 49 | 0/6 | 0/6 | RBF (LML tie-break, unchanged from Week_06) | **PASS** |
| Function_08 | 46 | 8 | 50 | 0/6 | 0/6 | Matérn (LML tie-break) | **PASS** |

## Notable Findings

- **Function_05's instability deepened rather than resolving.** Week_06
  showed an unequal 4/6-vs-3/6 ABNORMAL split (Matérn selected). This
  week: RBF worsened to 5/6, Matérn worsened to 4/6 — both kernels are
  genuinely less reliable than last week. The notebook's own dynamic
  comparison states this plainly: "convergence this week is no better,
  and at least partially worse, than Week 6's 4/6-vs-3/6 split — the
  instability first documented in Week 2 and confirmed in every week
  since persists (or has deepened) for this function." This is the sixth
  consecutive week (Weeks 2-7) this function has shown genuine optimizer
  instability, and the direction has now reversed from Week_06's partial
  improvement.
- **Function_07's kernel selection (RBF) held steady** this week,
  unchanged from Week_06. Both kernels converged cleanly in both weeks —
  an ordinary, stable LML outcome, not part of the earlier flip pattern
  (Week_03 RBF → Week_04 Matérn → Week_05 Matérn → Week_06 RBF → Week_07
  RBF).
- **All other 6 functions converged cleanly for both kernels** (0/6
  ABNORMAL each), with kernel selection resolved purely by
  log-marginal-likelihood comparison.

## Minor Finding — Resolved

The review flagged that `Historical_Replay/Week_07/requirements-lock.txt`
was missing, unlike Week_02-06. This has been added, using the exact same
package versions as Week_06's lock file — independently confirmed via
direct `pip freeze` inspection of the Python 3.14.3 environment actually
used to execute these notebooks, showing the environment is byte-identical
to Week_06's (numpy 2.5.2, scipy 1.18.1, scikit-learn 1.9.0, matplotlib
3.11.2, seaborn 0.13.2, and the same Jupyter toolchain versions).

## Scope Confirmation

No notebook loads or references Week_06-or-earlier data files directly
(only that function's own Week_07 cumulative dataset), no notebook runs an
acquisition function or proposes/saves a query, no notebook references or
builds anything resembling the separate `08_Retrospective_Acquisition`
analysis that exists for Weeks 02-05 (confirmed absent from Week_07 on
disk), no `Week_08` directory exists anywhere in the project, and no
Week_01-06 file was modified.
