# Code Review Report — Historical_Replay/Week_07/Function_05

**Scope:** Read-only code review of `01_Notebook/week_07_gp_diagnostics.ipynb`,
conducted by the Code Reviewer Agent, using the same 17-point checklist
applied to Week_05/Week_06. No notebook, dataset, figure, or existing
report was modified. This function received specific extra scrutiny given
its ongoing convergence instability, unresolved across Weeks 2-6 and
notably worsening again this week.

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** PASS. RBF 5/6 ABNORMAL, Matérn 4/6 ABNORMAL — worse than
Week_06's 4/6-vs-3/6 split on both kernels — independently confirmed as
genuinely computed at runtime, not hardcoded, with Matérn selected via
the fewer-ABNORMAL rule. The Week_06-vs-Week_07 comparison is described
honestly.

## Findings by Criterion

1. **Data provenance/leakage** — No issue found. Loads only
   `Week_07/Function_05/02_Data/cumulative_inputs.npy`/`cumulative_outputs.npy`;
   no Week_06-or-earlier file loaded in code.
2. **Sanity checks** — No issue found. `EXPECTED_N=26`, `EXPECTED_D=4`,
   bounds/finiteness/duplicate assertions present and passing. Minimum
   coordinate of exactly `0.0` confirmed permitted (inclusive lower bound).
3. **Final-row identification** — No issue found. New row matches the
   Week_06 historical pair recorded in `CUMULATIVE_DATA_VALIDATION.md`.
4. **Descriptive statistics / best-observed** — No issue found.
5. **Standardization** — No issue found. `y_scaled` computed once;
   `inverse_mean`/`inverse_std` applied before every reported/plotted value.
6. **Kernel construction** — No issue found. Matches the project-wide
   `ConstantKernel*(RBF|Matern)+WhiteKernel` specification exactly.
7. **Seed** — No issue found. `SEED = 42 + 5 = 47`.
8. **Convergence rule** — No issue found. RBF 5/6 ABNORMAL, Matern 4/6
   ABNORMAL — unequal counts → per the rule, **Matérn** selected as the
   more convergence-reliable candidate (fewer ABNORMAL terminations), not
   necessarily the higher-LML kernel. Matches the claimed outcome exactly,
   and the branch is a genuine runtime comparison of the two integer
   counts computed in the per-restart diagnostic cell, not a hardcoded
   conclusion.
9. **Warnings visibility** — No issue found. Raw stderr contains
   ConvergenceWarning/"ABNORMAL:" messages consistent with 5/6 (RBF) and
   4/6 (Matérn); nothing suppressed.
10. **Function_05-specific dynamic comparison — PASS.** The notebook's
    Section 8e compares this week's live counts against a fixed,
    correctly-set Week_06 baseline (`WEEK_06_RBF_ABNORMAL = 4`,
    `WEEK_06_MATERN_ABNORMAL = 3`) computed at runtime against
    `rbf_n_abnormal`/`matern_n_abnormal`. Printed output: *"RBF: WORSENED
    this week (5/6 vs 4/6 ABNORMAL last week)."* / *"Matern: WORSENED
    this week (4/6 vs 3/6 ABNORMAL last week)."* / *"Overall: convergence
    this week is no better, and at least partially worse, than Week 6's
    4/6-vs-3/6 split — the instability first documented in Week 2 and
    confirmed in every week since persists (or has deepened) for this
    function."* This does not overstate reliability — both kernels
    genuinely worsened, and the notebook says so plainly.
11. **In-sample labeling** — No issue found.
12. **CI/slice correctness** — No issue found. (d=4, no 2D grid section;
    1D slices only.)
13. **Figures genuine** — No issue found. All 7 expected figures present
    in `04_Figures/`.
14. **Provenance citation** — No issue found. Correctly cites
    `Historical_Replay/Week_06/Function_05/pair_provenance.md` by reference
    only (not loaded in code).
15. **`requirements-lock.txt`** — Present at `Historical_Replay/Week_07/`
    (project-wide; matches Week_06's, environment confirmed unchanged via
    direct `pip freeze` inspection before being added).
16. **No Week_08 data/acquisition/future info** — No issue found.
17. **No dataset/earlier-week modification** — No issue found
    (project-wide; confirmed via `find`/markdown-only reference check).

## Overall Verdict

**PASS.** No Critical, Major, or Minor findings. Function_05's convergence
instability has not resolved across six weeks and, this week, genuinely
deepened on both kernels — described honestly, with Matérn's selection
correctly framed as a fewer-failures pick, not a reliable fit.
