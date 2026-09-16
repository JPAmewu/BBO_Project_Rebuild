# Independent Methodology Review — Week 2 EDA/GP Surrogate Notebooks

**Scope:** Independent, read-only methodology review of all 8
`Historical_Replay/Week_02/Function_0X/01_Notebook/week_02_eda_gp_surrogate.ipynb`
notebooks, conducted by the ML Reviewer Agent. No file was modified. Findings
were verified by reading actual code cells and stderr output, not by
trusting the notebooks' own markdown claims. Severity labels (Critical /
Major / Minor / Informational) follow this project's established convention.

**Overall verdict: Sound, with minor remediation recommended.** No Critical
findings. Two Major findings, both specific to Function_05 (an optimizer
non-convergence issue not reflected in that notebook's conclusion text). All
other findings are Minor or Informational.

## Checks Confirmed Clean Across All 8 Functions (Independently Verified)

- **Data provenance/no leakage**: every notebook loads only
  `../02_Data/cumulative_inputs.npy`/`cumulative_outputs.npy` for its own
  function. No reference to `Week_01/Function_0X/03_Queries/week_01_query.txt`
  (the separate, unevaluated random-baseline query), no later week, no
  reference to the original source project as an executed code path.
  "03_Queries"/"Week_03" appear only in scope-boundary disclaimer markdown.
- **Sanity checks are real**: shape/dtype/dimensionality/NaN-Inf/bounds/
  duplicate checks are genuine `assert`-backed computations using
  function-specific expected values (11/2, 11/2, 16/3, 31/4, 21/4, 21/5,
  31/6, 41/8) that match both the spec and the notebooks' own printed output.
- **Standardization is correct**: `y` is standardized before fitting and
  correctly inverse-transformed before every plotted/reported value; no
  double-transform, no reporting of standardized values as original-scale.
- **GP model construction is correct**: kernel composition is exactly
  `ConstantKernel * (RBF|Matern(nu=2.5)) + WhiteKernel` for every function;
  `random_state = 42 + function_number` correctly instantiated per function
  (verified 43–50); `n_restarts_optimizer=5` present everywhere.
- **Reproducibility (in-notebook)**: no unseeded stochastic operations found
  anywhere in the GP-fitting or plotting code.
- **Conclusions are hedged**: the "which kernel wins" language is
  consistently caveated with the same verbatim disclaimer in every notebook
  ("...this is not the same as being the definitively 'correct' or
  generalising-best kernel... should be treated as exploratory evidence, not
  a validated model-selection result"), applied consistently regardless of
  which kernel actually won.
- **Visualizations are genuine**: 2D contour plots (Function_01/02) use a
  real `np.meshgrid`/`np.linspace(0,0.999999,60)` grid, not a placeholder;
  1D slice plots (all 8) correctly vary exactly one coordinate
  (`X_slice[:, i] = slice_grid`) while holding the rest at the best-observed
  input, verified from the slicing code directly.
- **No new query generated**: no notebook computes, prints, or saves
  anything resembling a proposed next query; the only file writes are
  `fig.savefig(...)` PNGs to `05_Figures/`.
- **Code quality**: no unused imports, no hardcoded absolute paths, no
  silently-swallowed exceptions, dimensionality read dynamically via
  `inputs.shape[1]` rather than hardcoded per function.

## Per-Function Findings

### Function_01 — PASS (Minor findings only)
- **[Minor]** Cell 7's markdown cites the provenance file as
  `Historical_Replay/Week_01/Function_001/pair_provenance.md` (three-digit),
  which does not exist. The correct path,
  `Historical_Replay/Week_01/Function_01/pair_provenance.md` (two-digit), is
  used correctly elsewhere in the same notebook (cell 0). Text-only — never
  programmatically opened — but a broken citation.
- **[Minor]** No library/environment version pinning anywhere in the
  project (no `requirements.txt`/`environment.yml`, no printed
  `sklearn.__version__` etc.) — exact bit-for-bit reproducibility across
  environments isn't guaranteed even with the seed fixed.
- **[Informational]** `ConvergenceWarning`s for `length_scale` near the
  upper bound (dim 2, both kernels) — not discussed in markdown.
- RBF wins (LML −14.81 vs −15.01), correctly hedged.

### Function_02 — PASS (Minor findings only)
Same pattern as Function_01: broken three-digit citation (`Function_002`),
same version-pinning gap, same unaddressed boundary `ConvergenceWarning`s.
RBF wins (−11.23 vs −11.41), correctly hedged.

### Function_03 — PASS (Minor/Informational findings only)
Same broken-citation and version-pinning issues (`Function_003`). No 2D
contour section (correctly omitted, d=3). **[Informational]** n=16
observations against 5 GP hyperparameters (3 ARD length-scales + constant +
noise) is a fairly tight sample-to-parameter ratio; the notebook's generic
small-data caveat covers this in general terms but doesn't call out this
specific ratio. RBF wins (−12.53 vs −14.01), correctly hedged.

### Function_04 — PASS (Minor/Informational findings only)
Same broken-citation (`Function_004`) and version-pinning issues. Matérn
wins this time (−17.81 vs −18.65) — notably, the same hedging text applies
regardless of which kernel wins, confirming the caveat isn't cherry-picked.
`noise_level` hit its lower bound for both kernels (Informational, not
discussed in markdown).

### Function_05 — PASS overall, with a function-specific Major finding
- **[Major]** The RBF fit's L-BFGS-B optimizer reported explicit `ABNORMAL`
  non-convergence **4 separate times** during the 5-restart search (quoted
  stderr: `"lbfgs failed to converge after 21/22/26/31 iteration(s)
  (status=2): ABNORMAL..."`), plus 2 further near-bound warnings. This
  means a majority of the restarts used to select the reported
  hyperparameters/LML did not converge to a local optimum — sklearn still
  returns the best-found result among attempts, but the underlying fit
  process was visibly less reliable here than for any other function.
- **[Major, tied to the above]** The notebook's conclusion text uses the
  same confidence level and identical generic hedging as the other 7
  functions, without mentioning this specific optimizer instability — an
  instance of the narrative not reflecting what the underlying computation
  actually showed.
- Cell 14's markdown citing this function's own near-duplicate finding from
  `INDEPENDENT_CUMULATIVE_REVIEW.md` (as justification for including a
  `WhiteKernel`) was independently checked and confirmed **accurate** —
  the quoted coordinates, output difference (≈−0.0061), and the "cannot be
  determined without executing the underlying black-box function" line are
  verbatim from that document, not fabricated.
- Matérn wins (−0.91 vs −1.33) — this is the specific result whose
  reliability is undercut by the convergence issue above.
- Same broken-citation issue (`Function_005`).

### Function_06 — PASS (cleanest of the 8)
The only notebook with **zero** `ConvergenceWarning`s of any kind. Same
broken-citation and version-pinning issues (`Function_006`). RBF wins, with
an extremely close LML gap (−22.90 vs −22.98), correctly not overclaimed
given how close the values are.

### Function_07 — PASS (Minor/Informational findings only)
Same broken-citation (`Function_007`) and version-pinning issues.
**[Informational]** Several ARD length-scales saturated at the upper bound
(dim 2 for RBF; dims 1 and 2 for Matérn) — a real signal that those
directions are being modelled as near-linear/very smooth on this data, not
called out in the notebook text. RBF wins (−33.10 vs −33.42).

### Function_08 — PASS (Minor/Informational findings only)
Same broken-citation issue (`Function_008`). Matérn wins with the only
**positive** LML in the set (+1.53 vs −4.90 for RBF) — mathematically
normal for a continuous likelihood, but a large swing versus RBF that the
notebook doesn't comment on the magnitude of (though it doesn't overclaim
either). One genuinely interesting case handled correctly: the newly
appended historical point (index 40) is also the best-observed point
overall; the notebook detects and reports this plainly
(`best_idx == historical_idx` → `True`) without any inappropriate
special-casing.

## Cross-Function Summary

**Recurring identical issues (all 8 notebooks):**
1. **[Minor]** Broken three-digit provenance citation in cell 7
   (`Function_00X` instead of `Function_0X`) — text-only, no computational
   effect, but present identically in all 8 and worth fixing for
   documentation integrity.
2. **[Minor]** No library/environment version pinning anywhere in the
   project — `random_state` is correctly set everywhere, but exact
   cross-environment reproducibility of the L-BFGS-B-based optimizer isn't
   fully guaranteed without recorded package versions.

**Function-specific issue (not recurring):**
- **[Major, Function_05 only]** Optimizer non-convergence during hyperparameter
  search not reflected in the conclusion's confidence level.

**Informational only (7 of 8; Function_06 clean):** unaddressed
`ConvergenceWarning`s for hyperparameters landing at/near their search
bounds — doesn't invalidate results, but a missed opportunity to flag
per-function fit quality.

## Severity Counts (Across All 8 Functions)

| Severity | Count |
|---|---|
| Critical | 0 |
| Major | 2 (both Function_05, two facets of one underlying issue) |
| Minor | 17 (broken citation ×8, version-pinning gap counted project-wide, plus the Function_03 sample-to-parameter-ratio observation) |
| Informational | 8 (one per affected notebook; Function_06 clean) |

## What Could Not Be Verified Statically (Disclosed, Out of Scope for a Read-Only Review)

- Whether re-executing these notebooks today reproduces the exact printed
  hyperparameters/LML values bit-for-bit — the seeding/code logic is
  confirmed deterministic-by-design, but nothing was actually re-executed
  during this review.
- The actual pixel content of the saved PNG figures — confirmed the
  plotting code computes genuine grids/slices and that expected
  figure/axes counts exist in the notebook output, but individual PNGs
  were not visually opened.
- The accuracy of `Historical_Replay/Week_01/Function_0X/pair_provenance.md`
  content itself — only the *path* cited by the Week 2 notebooks was
  checked for existence; deep-auditing Week 1's own provenance chain was
  out of scope (the Week 2 notebooks cite it by reference only, never open
  it programmatically).

## Recommendations (Not Implemented — Findings Only, Per This Review's Scope)

1. Fix the broken three-digit provenance citation (`Function_00X` →
   `Function_0X`) in cell 7 of all 8 notebooks.
2. Add an explicit caveat to Function_05's interpretation text acknowledging
   the observed L-BFGS-B non-convergence for its RBF fit, so the stated
   confidence matches the underlying computation's actual reliability.
3. Optionally, record library versions (`numpy`, `scipy`, `scikit-learn`) in
   each notebook or a project-level `requirements.txt`, for stronger
   cross-environment reproducibility guarantees beyond the already-correct
   seeding.

## Final Verdict

# **Sound — no Critical issues; 2 Major findings (both Function_05-specific, both addressable); remediation recommended but not blocking**
