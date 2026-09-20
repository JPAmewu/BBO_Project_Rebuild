# Code Review Report — Week_04 Retrospective Acquisition Analysis

**Scope:** Read-only, independent code review of the 8 new
`Function_0X/08_Retrospective_Acquisition/week_04_retrospective_acquisition.ipynb`
notebooks and this folder's `RETROSPECTIVE_ACQUISITION_ANALYSIS.md`,
conducted by the Code Reviewer Agent. This analysis computes what EI,
UCB, and PI would have recommended using only Week_04's own cumulative
data — retrospective, counterfactual, never queried, never submitted. No
existing file was modified by this analysis or by this review.

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** **PASS.** No Critical or Major findings across all 8
functions. All 13 checklist items passed, including byte-for-byte
formula verification across all 16 notebooks (Week_04 + Week_05
reviewed together), exemplary seed hygiene, and strict data-boundary
discipline enforced by runtime assertions.

## Findings by Criterion

1. **Scope/leakage** — No issue found. Row counts independently confirmed:
   13, 13, 18, 33, 23, 23, 33, 43 for Functions 01–08. Mentions of other
   weeks appear only in disclaimer markdown text, never in code.
2. **No existing file modified** — No issue found. `git status --short`
   and `git diff --stat` confirm zero tracked-file changes; only new,
   untracked paths appear.
3. **Kernel-selection accuracy** — No issue found. Cross-checked against
   `CODE_REVIEW_SUMMARY.md`: RBF for 01/02/03, Matérn for 04/06/08,
   Matérn-nominal-but-neither-clean for 05 (5/6 vs 3/6 ABNORMAL), Matérn
   for 07 (ordinary LML tie-break, flipped from Week_03's RBF, both clean).
4. **Formula correctness** — No issue found. `acquisitions_raw()`
   implementation verified byte-for-byte identical across all 16
   notebooks (Week_04 and Week_05), matching the specified EI/UCB/PI
   formulas exactly, operating on inverse-transformed raw-scale values,
   correctly oriented for maximisation.
5. **Seed correctness** — No issue found. GP seed `42 + function_number`;
   acquisition seed `4000 + 42 + function_number`, distinct from Week_02
   (2000), Week_03 (3000), and Week_05 (5000).
6. **Bounds** — No issue found. All recommended points confirmed within
   `[0.000000, 0.999999]`.
7. **Labeling** — No issue found. "RETROSPECTIVE, COUNTERFACTUAL, AND
   UNEVALUATED" present at top and bottom of every notebook.
8. **No false association with real data** — No issue found.
9. **Doubly-hypothetical caveat accuracy** — No issue found. Function_05's
   caveat accurately states Matérn is "only the marginally more
   convergence-reliable of the two," with both kernels fit and reported.
10. **Execution integrity** — No issue found. Zero error cells; sequential
    execution counts across all 8 notebooks.
11. **Figures genuine** — No issue found. All figures exist, distinct MD5
    hashes.
12. **Summary document accuracy** — No issue found. Spot-checked
    Functions 02, 07 — exact match to 6 decimal places.
13. **Folder isolation** — No issue found. `01_Notebook/` contents
    unchanged; no `03_Queries` interaction.

## Informational Notes (Non-Blocking)

- **Function_05 visualization asymmetry vs. Week_05**: this week's
  Function_05 notebook plots acquisition slices for the primary kernel
  (RBF) only — one figure — while both kernels' numeric EI/UCB/PI values
  are still printed in text output for both. Week_05's equivalent
  notebook improves on this by producing a figure for each kernel. Not a
  correctness issue (the "primary kernel" convention is explicitly
  disclosed), but worth harmonizing if this analysis is extended further
  — e.g. backfilling a Matérn slice figure here for visual symmetry.
- **Multistart optimizer keeps best point regardless of `res.success`**:
  `optimise_multistart` tracks the best acquisition value across all
  starts without filtering out non-converged L-BFGS-B runs. Defensible,
  common practice, and had no practical effect here (49-50/50 starts
  converged in every case per the printed diagnostics), but noted since a
  genuinely failed run could in principle contribute an unfiltered point.
- **Folder-numbering note**: `CLAUDE.md` documents subfolders 1-7 as the
  standard structure; the new `08_Retrospective_Acquisition/` folder is a
  reasonable, non-colliding extension not yet reflected there — a
  documentation-currency note only, not a defect.

## Overall Verdict

**PASS.** No Critical, Major, or Minor findings. This retrospective
analysis is correctly scoped, formulaically correct, properly labeled,
and independently verifiable against the real historical record.
