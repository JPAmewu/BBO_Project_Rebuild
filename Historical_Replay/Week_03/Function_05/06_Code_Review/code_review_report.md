# Code Review Report — Historical_Replay/Week_03/Function_05

**Scope:** Read-only code review of
`01_Notebook/week_03_gp_diagnostics.ipynb`, conducted by the Code Reviewer
Agent. No notebook, dataset, figure, or existing report was modified. This
function received specific extra scrutiny given its known convergence
instability (worse than Week 2 — both kernels now show ABNORMAL
terminations).

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** PASS, with an Informational note on unusual-but-honest
handling. No Critical/Major/Minor findings.

## Findings by Criterion

1. **Data provenance/leakage** — No issue found.
2. **Sanity checks** — No issue found. `EXPECTED_N=22`, `EXPECTED_D=4`.
3. **Final-row identification** — No issue found. `historical_idx=21`,
   `[0.255841, 0.841692, 0.888984, 0.860260]` → `1035.6341457754475`,
   matching `CUMULATIVE_DATA_VALIDATION.md` exactly.
4. **Descriptive statistics / best-observed** — No issue found. Independently
   recomputed `best_idx=15`, `best_out=1088.8596181962705` — matches.
5. **Standardization** — No issue found.
6. **Kernel construction** — No issue found.
7. **Seed** — No issue found. `SEED = 42 + 5 = 47`.
8. **Convergence rule** — No issue found. All 6 restarts instrumented per
   kernel. Raw stderr genuinely shows multiple "ABNORMAL:" blocks in both
   the `build_and_fit` cell and the diagnostic cell, consistent with the
   reported 3/6 (RBF) and 5/6 (Matérn) counts. Matches claimed outcome
   table exactly.
9. **Warnings visibility** — No issue found. Zero suppression anywhere;
   real ABNORMAL/ConvergenceWarning stderr text visible and matches the
   printed counts.
10. **Function_05-specific honesty check — PASS.** A dedicated markdown
    cell, titled "Function_05-specific note (read this before trusting the
    diagnostics in Section 9 below)," states verbatim:
    > "Unlike the other 7 functions in this Week 3 batch, Function_05's GP
    > hyperparameter search shows genuine L-BFGS-B optimizer instability
    > across restarts for **both** kernels ... neither kernel converges
    > cleanly across all 6 restart attempts. Per the rule applied
    > programmatically in the cell above, the kernel with the *fewer*
    > ABNORMAL terminations is still treated as the (marginally) more
    > convergence-reliable of the two exploratory candidates — this is
    > **not** the same as a clean, fully reliable fit, and it is **not**
    > necessarily the kernel with the higher raw log-marginal-likelihood."

    This is honest and non-overstated: it does not claim RBF is "the
    model" with any reliability guarantee, and explicitly states "Results
    for this function below should be read with additional caution." The
    decision cell correctly outputs `DECISION: RBF` with reasoning "even
    though this may not be the kernel with the higher raw
    log-marginal-likelihood" — true here (Matérn's raw LML 3.655 > RBF's
    3.466, yet RBF is selected per the fewer-ABNORMAL rule, applied
    programmatically, not hand-picked).
11. **In-sample labeling** — No issue found.
12. **CI/slice correctness** — No issue found.
13. **Figures genuine** — No issue found.
14. **Provenance citation** — No issue found. Correct path to
    `Historical_Replay/Week_02/Function_05/pair_provenance.md`.
15. **`requirements-lock.txt`** — No issue found (project-wide).
16. **No Week 03 data/acquisition/future info** — No issue found.
17. **No dataset/earlier-week modification** — No issue found (project-wide).

**Informational:** Section 9's intro boilerplate is identical generic
language to the other 7 notebooks ("the surrogate tracks the training data
reasonably well in-sample"). For Function_05 this could have been
strengthened with an explicit back-reference to the instability caveat
immediately above it, but that dedicated caveat cell already carries the
necessary weight — this is a stylistic observation, not a defect.

## Overall Verdict

**PASS.** No Critical, Major, or Minor findings. Function_05's instability
is described honestly and the RBF selection does not overstate reliability.
