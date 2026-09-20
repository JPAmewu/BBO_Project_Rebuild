# Code Review Report — Week_02 Retrospective Acquisition Analysis

**Scope:** Read-only, independent code review of the 8 new
`Function_0X/08_Retrospective_Acquisition/week_02_retrospective_acquisition.ipynb`
notebooks and this folder's `RETROSPECTIVE_ACQUISITION_ANALYSIS.md`,
conducted by the Code Reviewer Agent. This analysis computes what EI,
UCB, and PI would have recommended using only Week_02's own cumulative
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
   `cumulative_outputs.npy`. Row counts independently confirmed: 11, 11,
   16, 31, 21, 21, 31, 41 for Functions 01–08.
2. **No existing file modified** — No issue found. `git status
   --porcelain` shows only untracked (`??`) new files; zero modified/deleted
   entries anywhere in the repo.
3. **Kernel-selection accuracy** — No issue found. All 8 functions'
   claimed selections independently cross-checked against
   `week_02_eda_gp_surrogate.ipynb` Section 6a and `REMEDIATION_REPORT.md`
   — exact match in every case (RBF: 01, 02, 03, 05, 06, 07; Matérn: 04,
   08; Function_05 via convergence override, RBF 1/6 vs Matérn 3/6
   ABNORMAL).
4. **Formula correctness** — No issue found. EI/UCB/PI implemented
   exactly per specification (`xi=0.01`, `kappa=2.0`), on inverse-transformed
   raw-scale posterior values, correctly oriented for maximisation.
5. **Seed correctness** — No issue found. GP seed `42 + function_number`;
   acquisition multi-start seed `2000 + 42 + function_number`, uniquely
   offset and non-colliding.
6. **Bounds** — No issue found. All recommended points confirmed within
   `[0.000000, 0.999999]`.
7. **Labeling** — No issue found. "RETROSPECTIVE, COUNTERFACTUAL, AND
   UNEVALUATED" present as an actual markdown cell in every notebook.
8. **No false association with real data** — No issue found.
9. **Doubly-hypothetical caveat accuracy** — No issue found. Function_05
   correctly treated as a genuine single selection with a milder
   instability caveat (not the "neither/both exploratory" case), matching
   its real Week_02 record exactly.
10. **Execution integrity** — No issue found. Zero error cells across all
    8 notebooks; sequential execution counts.
11. **Figures genuine** — No issue found. All figure files exist,
    distinct SHA-1 hashes, sensible size variation.
12. **Summary document accuracy** — No issue found. Spot-checked
    Functions 01, 02, 05, 08 — recommended points/values match the
    executed notebook output exactly.
13. **Folder isolation** — No issue found. `08_Retrospective_Acquisition/`
    correctly sits as a sibling of `01_Notebook/`; nothing placed inside
    any existing folder; no `03_Queries` interaction.

## Informational Notes (Non-Blocking)

- Fixed `xi=0.01`/`kappa=2.0` constants behave very differently across
  functions with wildly different raw output scales (e.g. Function_01
  ~1e-16 vs. Function_05 in the hundreds) — this is a transparent,
  documented consequence of following the specified formulas exactly, not
  an error, and is already flagged in the summary document.
- Minor stylistic inconsistency in divide-by-zero handling between weeks
  (masked assignment vs. `np.where` with a dummy substitute) — both
  mathematically equivalent, cosmetic only.

## Overall Verdict

**PASS.** No Critical, Major, or Minor findings. This retrospective
analysis is correctly scoped, formulaically correct, properly labeled,
and independently verifiable against the real historical record.
