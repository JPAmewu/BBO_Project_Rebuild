# Code Review Report — Historical_Replay/Week_04/Function_05

**Scope:** Read-only code review of
`01_Notebook/week_04_gp_diagnostics.ipynb`, conducted by the Code Reviewer
Agent. No notebook, dataset, figure, or existing report was modified. This
function received specific extra scrutiny given its ongoing convergence
instability (flipped between kernels this week, still not resolved).

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** PASS. The Week 3-vs-Week 4 comparison is genuinely computed
at runtime and described honestly. No Critical/Major/Minor findings.

## Findings by Criterion

1. **Data provenance/leakage** — No issue found.
2. **Sanity checks** — No issue found. `EXPECTED_N=23`, `EXPECTED_D=4`.
3. **Final-row identification** — No issue found. `historical_idx=22`,
   `[0.255842, 0.841692, 0.888985, 0.860266]` → `1035.6647479285914`,
   matching `CUMULATIVE_DATA_VALIDATION.md` exactly.
4. **Descriptive statistics / best-observed** — No issue found.
   Independently recomputed (min=0.11294, max=1088.86, mean=268.939,
   std=380.561; best_idx=15, correctly not the appended row) — matches.
5. **Standardization** — No issue found.
6. **Kernel construction** — No issue found.
7. **Seed** — No issue found. `SEED = 42 + 5 = 47`.
8. **Convergence rule** — No issue found. RBF 5/6 ABNORMAL, Matérn 3/6
   ABNORMAL → **Matérn** selected (fewer ABNORMAL; neither converges
   cleanly). Matches claimed outcome table exactly.
9. **Warnings visibility** — No issue found. Zero suppression anywhere;
   raw stderr contains exactly 5 "ABNORMAL:" messages for RBF and exactly
   3 for Matérn (total 8), matching the printed 5/6 and 3/6 counts exactly.
10. **Function_05-specific dynamic comparison — PASS.** Quoted verbatim
    from the notebook's code cell:
    ```python
    WEEK_03_RBF_ABNORMAL = 3
    WEEK_03_MATERN_ABNORMAL = 5
    ...
    print(_compare(WEEK_03_RBF_ABNORMAL, rbf_n_abnormal, "RBF"))
    print(_compare(WEEK_03_MATERN_ABNORMAL, matern_n_abnormal, "Matern"))
    ```
    This is a genuinely **dynamic** computation: `WEEK_03_RBF_ABNORMAL`/
    `WEEK_03_MATERN_ABNORMAL` are the only hardcoded values (a fixed Week 3
    baseline, correctly set to 3 and 5, matching Week 3's own recorded
    outcome), compared at runtime against the live `rbf_n_abnormal`/
    `matern_n_abnormal` variables computed in this same execution. Printed
    output: *"RBF: WORSENED this week (5/6 vs 3/6 ABNORMAL last week)."* /
    *"Matern: IMPROVED this week (3/6 vs 5/6 ABNORMAL last week)."* /
    *"Overall: a mixed picture relative to Week 3 ... the underlying
    instability for this function has not been cleanly resolved either
    way."* This does **not** overstate the reliability of the Matérn
    selection — a separate caveat cell additionally states: *"the kernel
    with fewer ABNORMAL terminations (MATERN) is treated, per the rule, as
    only the MARGINALLY more convergence-reliable of the two exploratory
    candidates — this is NOT the same as a clean, fully reliable fit."*
11. **In-sample labeling** — No issue found.
12. **CI/slice correctness** — No issue found.
13. **Figures genuine** — No issue found.
14. **Provenance citation** — No issue found. Correct path to
    `Historical_Replay/Week_03/Function_05/pair_provenance.md`.
15. **`requirements-lock.txt`** — No issue found (project-wide).
16. **No Week 04 data/acquisition/future info** — No issue found.
17. **No dataset/earlier-week modification** — No issue found (project-wide).

## Overall Verdict

**PASS.** No Critical, Major, or Minor findings. Function_05's ongoing
instability (kernel preference flipped from Week 3, still not cleanly
resolved) is described honestly and the Matérn selection does not
overstate reliability.
