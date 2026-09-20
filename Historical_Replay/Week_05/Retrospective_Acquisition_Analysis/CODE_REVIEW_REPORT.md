# Code Review Report — Week_05 Retrospective Acquisition Analysis

**Scope:** Read-only, independent code review of the 8 new
`Function_0X/08_Retrospective_Acquisition/week_05_retrospective_acquisition.ipynb`
notebooks and this folder's `RETROSPECTIVE_ACQUISITION_ANALYSIS.md`,
conducted by the Code Reviewer Agent. This analysis computes what EI,
UCB, and PI would have recommended using only Week_05's own cumulative
data — retrospective, counterfactual, never queried, never submitted. No
existing file was modified by this analysis or by this review.

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** **PASS.** No Critical or Major findings across all 8
functions. This function set received extra scrutiny for Function_05,
whose notebook had an unusual build history (an earlier interrupted
session left it unexecuted with stale figures; it was later verified
correct and re-executed fresh, rather than rebuilt from scratch) — this
was independently confirmed to be genuinely correct and freshly executed,
not stale.

## Findings by Criterion

1. **Scope/leakage** — No issue found. Row counts independently confirmed:
   14, 14, 19, 34, 24, 24, 34, 44 for Functions 01–08.
2. **No existing file modified** — No issue found. `git diff --stat` empty
   for this week; only new, untracked paths appear.
3. **Kernel-selection accuracy** — No issue found. Cross-checked against
   `CODE_REVIEW_SUMMARY.md`: RBF for 01/02/03, Matérn for 04/06/07/08,
   **neither selected** for 05 (RBF 4/6 vs Matérn 4/6 ABNORMAL — exact
   tie, the strictest doubly-hypothetical case in the project).
4. **Formula correctness** — No issue found. Verified byte-for-byte
   identical to Week_04's implementation (reviewed together), matching
   the specified EI/UCB/PI formulas exactly.
5. **Seed correctness** — No issue found. GP seed `42 + function_number`;
   acquisition seed `5000 + 42 + function_number`, distinct from all
   other weeks' offsets.
6. **Bounds** — No issue found.
7. **Labeling** — No issue found. Present at top and bottom of every
   notebook.
8. **No false association with real data** — No issue found.
9. **Doubly-hypothetical caveat accuracy** — No issue found, and
   genuinely differentiated from Week_04's case: the notebook's own
   caveat explicitly contrasts itself against Week_04's unequal 5/6-vs-3/6
   split, stating Week_05's exact 4/6-vs-4/6 tie means "neither kernel is
   preferred even marginally" — bespoke language, not a template copy.
10. **Execution integrity** — No issue found, **including Function_05
    specifically**: 8 code cells, sequential execution counts 1-8, two
    genuine embedded figure outputs (one per kernel) matching its two
    saved figure files on disk.
11. **Figures genuine** — No issue found. **Function_05's figures
    independently confirmed fresh**: both figures and the notebook share
    an identical mtime, clustered in the same execution batch as
    Functions 06-08 (the later corrective session) — clearly distinct
    from Functions 01-04's earlier batch, confirming these are genuinely
    regenerated, not leftover stale artifacts from the interrupted
    session.
12. **Summary document accuracy** — No issue found. Spot-checked
    Functions 03, 08 — exact match to 6 decimal places.
13. **Folder isolation** — No issue found.

## Informational Notes (Non-Blocking)

- **Function_05's PI recommends an identical point for both RBF and
  Matérn** (`0.662031-0.651578-0.899750-0.972402`, value `1.000000` for
  both). This is very likely a floating-point saturation artifact (both
  kernels' `z` value is large enough that `norm.cdf(z)` rounds to exactly
  1.0 over a broad plateau, combined with identical seeded multi-start
  arrays for both kernel loops converging to the same locally-flat
  region) rather than two independent optimizations genuinely agreeing.
  The notebook already discloses the saturation itself but doesn't
  explicitly flag that the identical point is likely an optimizer
  artifact — recommend adding one sentence to the Function_05 caveat
  clarifying this, so a future reader doesn't over-interpret it as
  substantive agreement between kernels.
- **Multistart optimizer keeps best point regardless of `res.success`** —
  same non-blocking note as Week_04; had no practical effect here (49-50/50
  starts converged per the printed diagnostics).
- **Folder-numbering note**: same as Week_04 — `08_Retrospective_Acquisition/`
  is a reasonable extension beyond `CLAUDE.md`'s documented 1-7 subfolder
  convention, not yet reflected there.

## Overall Verdict

**PASS.** No Critical, Major, or Minor findings. Function_05's unusual
build history was independently verified as producing genuinely correct,
freshly-executed output — not a shortcut that compromised correctness.
