# Code Review Report — Week_03 Retrospective Acquisition Analysis

**Scope:** Read-only, independent code review of the 8 new
`Function_0X/08_Retrospective_Acquisition/week_03_retrospective_acquisition.ipynb`
notebooks and this folder's `RETROSPECTIVE_ACQUISITION_ANALYSIS.md`,
conducted by the Code Reviewer Agent. This analysis computes what EI,
UCB, and PI would have recommended using only Week_03's own cumulative
data — retrospective, counterfactual, never queried, never submitted. No
existing file was modified by this analysis or by this review.

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** **PASS.** No Critical or Major findings across all 8
functions. All 13 checklist items passed, including independent
cross-verification of every claimed kernel selection against the real
historical record, formula correctness, seed hygiene, bounds, labeling,
and figure genuineness.

## Findings by Criterion

1. **Scope/leakage** — No issue found. Every notebook's only `np.load`
   calls target its own `../02_Data/cumulative_inputs.npy`/
   `cumulative_outputs.npy`. Row counts independently confirmed: 12, 12,
   17, 32, 22, 22, 32, 42 for Functions 01–08.
2. **No existing file modified** — No issue found. `git status
   --porcelain` shows only untracked (`??`) new files; zero modified/deleted
   entries anywhere in the repo.
3. **Kernel-selection accuracy** — No issue found. All 8 functions'
   claimed selections independently cross-checked against
   `CODE_REVIEW_SUMMARY.md` — exact match in every case (RBF: 01, 02, 03,
   05, 07; Matérn: 04, 06, 08; Function_05 with RBF 3/6 vs Matérn 5/6
   ABNORMAL, neither converged cleanly).
4. **Formula correctness** — No issue found. EI/UCB/PI implemented
   exactly per specification (`xi=0.01`, `kappa=2.0`), on inverse-transformed
   raw-scale posterior values, correctly oriented for maximisation.
5. **Seed correctness** — No issue found. GP seed `42 + function_number`;
   acquisition multi-start seed `3000 + 42 + function_number`, uniquely
   offset and non-colliding with Week_02's (2000).
6. **Bounds** — No issue found. All recommended points confirmed within
   `[0.000000, 0.999999]`.
7. **Labeling** — No issue found. "RETROSPECTIVE, COUNTERFACTUAL, AND
   UNEVALUATED" present as an actual markdown cell in every notebook.
8. **No false association with real data** — No issue found.
9. **Doubly-hypothetical caveat accuracy** — No issue found. Function_05
   correctly identified as the doubly-hypothetical case (neither kernel
   converged cleanly) — both RBF and Matérn fit and reported side by
   side, with an explicit caveat, matching its real Week_03 record
   exactly and correctly distinguished from Week_02's milder,
   single-selection case.
10. **Execution integrity** — No issue found. Zero error cells across all
    8 notebooks; sequential execution counts.
11. **Figures genuine** — No issue found. All figure files exist,
    distinct SHA-1 hashes, sensible size variation (e.g. Function_08's
    8-D slice plot larger than Function_01's 2-D surface plot).
12. **Summary document accuracy** — No issue found. Spot-checked
    Functions 01, 03, 05 — recommended points/values, including the
    two-row RBF/Matérn treatment for Function_05, match the executed
    notebook output exactly.
13. **Folder isolation** — No issue found. `08_Retrospective_Acquisition/`
    correctly sits as a sibling of `01_Notebook/`; nothing placed inside
    any existing folder; no `03_Queries` interaction.

## Informational Notes (Non-Blocking)

- **Verification-rigor asymmetry vs. Week_02**: Week_02's notebooks
  independently refit *both* kernels for every function and assert
  numerical equality against the historically-recorded log-marginal-likelihood
  before selecting the winner. Week_03's notebooks (except Function_05,
  which correctly fits both) only refit the single kernel already known
  to have won, trusting `CODE_REVIEW_SUMMARY.md` rather than re-deriving
  the comparison from scratch. This was independently confirmed as
  factually correct against the real record for all 8 functions, so there
  is no leakage or fabrication risk — it is a lower level of self-verification
  than Week_02's approach, not an error. No fix required; noted for
  awareness of the inconsistency in rigor between weeks.
- Fixed `xi=0.01`/`kappa=2.0` constants behave very differently across
  functions with wildly different raw output scales — already
  transparently flagged in this week's own summary document (Section 7)
  as an intentional consequence of the specified formulas, not an error.

## Overall Verdict

**PASS.** No Critical, Major, or Minor findings. This retrospective
analysis is correctly scoped, formulaically correct, properly labeled,
and independently verifiable against the real historical record.
Function_05's doubly-hypothetical treatment is handled with genuine
nuance, distinct from Week_02's milder case.
