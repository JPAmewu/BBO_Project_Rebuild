# Code Review Report — Historical_Replay/Week_05/Function_05

**Scope:** Read-only code review of `01_Notebook/week_05_gp_diagnostics.ipynb`,
conducted by the Code Reviewer Agent, using the same 17-point checklist
applied to Week_03/Week_04. No notebook, dataset, figure, or existing
report was modified. This function received specific extra scrutiny given
its ongoing convergence instability, unresolved across Weeks 2-4.

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** PASS. The RBF-4/6-vs-Matérn-4/6 "both exploratory only, no
kernel selected" outcome was independently confirmed as genuinely computed
at runtime, not hardcoded, and the Week_04-vs-Week_05 comparison is
described honestly.

## Findings by Criterion

1. **Data provenance/leakage** — No issue found. Loads only
   `Week_05/Function_05/02_Data/cumulative_inputs.npy`/`cumulative_outputs.npy`;
   no Week_04-or-earlier file loaded in code.
2. **Sanity checks** — No issue found. `EXPECTED_N=24`, `EXPECTED_D=4`,
   bounds/finiteness/duplicate assertions present and passing.
3. **Final-row identification** — No issue found. New row matches the
   Week_04 historical pair (`0.000000-0.000000-0.000000-0.000000` →
   `163.1225`) recorded in `CUMULATIVE_DATA_VALIDATION.md`.
4. **Descriptive statistics / best-observed** — No issue found.
5. **Standardization** — No issue found. `y_scaled` computed once;
   `inverse_mean`/`inverse_std` applied before every reported/plotted value.
6. **Kernel construction** — No issue found. Matches the project-wide
   `ConstantKernel*(RBF|Matern)+WhiteKernel` specification exactly.
7. **Seed** — No issue found. `SEED = 42 + 5 = 47`.
8. **Convergence rule** — No issue found. RBF 4/6 ABNORMAL, Matern 4/6
   ABNORMAL — equal and non-zero → per the rule, **neither** kernel is
   selected; both reported as exploratory only. Matches the claimed
   outcome exactly, and the branch (`if rbf==0 and matern==0 ... elif
   rbf != matern ... else: selected_kernel_name = None`) is a genuine
   runtime comparison of the two integer counts computed in the
   per-restart diagnostic cell, not a hardcoded conclusion.
9. **Warnings visibility** — No issue found. Raw stderr contains
   ConvergenceWarning/"ABNORMAL:" messages consistent with 4/6 for each
   kernel; nothing suppressed.
10. **Function_05-specific dynamic comparison — PASS.** The notebook's
    Section 8e compares this week's live counts against a fixed,
    correctly-set Week_04 baseline (`WEEK_04_RBF_ABNORMAL = 5`,
    `WEEK_04_MATERN_ABNORMAL = 3`) computed at runtime against
    `rbf_n_abnormal`/`matern_n_abnormal`. Printed output: *"RBF: IMPROVED
    this week (4/6 vs 5/6 ABNORMAL last week)."* / *"Matern: WORSENED this
    week (4/6 vs 3/6 ABNORMAL last week)."* / *"Overall: a mixed picture
    relative to Week 4 ... the underlying instability for this function has
    not been cleanly resolved either way."* This does not overstate
    reliability — a separate caveat cell explicitly states that even the
    kernel selected in a tie-break scenario would only be "marginally more
    convergence-reliable," and in this week's case no kernel is selected
    at all.
11. **In-sample labeling** — No issue found.
12. **CI/slice correctness** — No issue found. (d=4, no 2D grid section;
    1D slices only, shown for both kernels since neither was selected.)
13. **Figures genuine** — No issue found. All 7 expected figures present
    in `04_Figures/` (both RBF and Matérn slices shown, consistent with
    the "both exploratory" outcome).
14. **Provenance citation** — No issue found. Correctly cites
    `Historical_Replay/Week_04/Function_05/pair_provenance.md` by reference
    only (not loaded in code).
15. **`requirements-lock.txt`** — Present at `Historical_Replay/Week_05/`
    (project-wide; matches Week_04's, environment unchanged).
16. **No Week_06 data/acquisition/future info** — No issue found.
17. **No dataset/earlier-week modification** — No issue found
    (project-wide; confirmed via `git status`).

## Overall Verdict

**PASS.** No Critical, Major, or Minor findings. Function_05's convergence
instability remains unresolved this week (4/6 ABNORMAL for both kernels,
worse for Matérn and better for RBF relative to Week_04 — a mixed,
inconclusive picture) and is described honestly, with neither kernel
overclaimed as reliable.
