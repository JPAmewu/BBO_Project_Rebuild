# Week 3 Retrospective Acquisition-Function Analysis — Summary

> **RETROSPECTIVE, COUNTERFACTUAL, AND UNEVALUATED.** Every recommended
> point in this document is a *what-if* computation over data already
> chronologically available at the start of Historical_Replay Week_03. **No
> point listed here was ever actually queried, has any associated real
> output, or was submitted anywhere.** This document is entirely separate
> from, and does not modify, any genuine historical query/output pair,
> `pair_provenance.md`, `VERIFIED_PAIRS.csv`, or any of the existing
> `week_03_gp_diagnostics.ipynb` notebooks in this track. Nothing in this
> document should be read as, or associated with, a genuine result of this
> project.

**Scope:** Historical_Replay, **Week_03 only**. This analysis does not
touch, use, or reference Week_02, Week_04, or Week_05 in any way, and does
not modify any existing dataset, notebook, or report.

## 1. Why this analysis exists

Every previously-verified `week_03_gp_diagnostics.ipynb` notebook in this
track (one per function, in `Function_0X/01_Notebook/`) explicitly stops at
visualising the GP posterior — no notebook in this project has, until now,
computed an acquisition function or proposed a next query point anywhere.
This analysis answers, purely retrospectively: *given only the data
available at the start of Week_03, what would Expected Improvement (EI),
Upper Confidence Bound (UCB), and Probability of Improvement (PI) each have
recommended as the next query, per function?*

New artifacts created by this analysis (purely additive, nothing else was
touched):

- `Function_0X/08_Retrospective_Acquisition/week_03_retrospective_acquisition.ipynb`
  (X = 01..08) — one notebook per function, each with its own executed
  outputs and figures in a sibling `figures/` folder.
- This document.

## 2. Data boundary (no lookahead)

Each function's notebook loads **only** its own already-existing
`Function_0X/02_Data/cumulative_inputs.npy` / `cumulative_outputs.npy` — the
exact same files already used (read-only) by that function's own
`week_03_gp_diagnostics.ipynb`. No Week_04+ file, no Week_02 file, and no
historical pair beyond what is already inside each function's own Week_03
cumulative dataset was used. Row counts / dimensionalities (independently
re-verified directly from the `.npy` files during this analysis):

| Function | n (rows) | d (dims) |
|---|---|---|
| 01 | 12 | 2 |
| 02 | 12 | 2 |
| 03 | 17 | 3 |
| 04 | 32 | 4 |
| 05 | 22 | 4 |
| 06 | 22 | 5 |
| 07 | 32 | 6 |
| 08 | 42 | 8 |

All inputs fall within the official bounds `[0.000000, 0.999999]`; all 8
functions are maximisation problems (CLAUDE.md rules 16–17).

## 3. Step 1 — Week_03's own kernel-selection outcome (per function)

Determined by reading `../CODE_REVIEW_SUMMARY.md` and `../WEEK_03_METHOD.md`,
and independently confirmed against each function's own executed
`../Function_0X/01_Notebook/week_03_gp_diagnostics.ipynb` (specifically its
"Model(s) used for Section 9 diagnostics" printed output). All 8 matched
exactly:

| Function | RBF ABNORMAL | Matérn ABNORMAL | Week_03 selected kernel | Notes |
|---|---|---|---|---|
| 01 | 0/6 | 0/6 | RBF | Both converged cleanly; RBF wins on higher LML. |
| 02 | 0/6 | 0/6 | RBF | Both converged cleanly; RBF wins on higher LML. |
| 03 | 0/6 | 0/6 | RBF | Both converged cleanly; RBF wins on higher LML. |
| 04 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML. |
| 05 | 3/6 | 5/6 | RBF | **Neither kernel converged cleanly.** RBF selected only as the marginally more convergence-reliable of the two, per the project's own rule. |
| 06 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML. |
| 07 | 0/6 | 0/6 | RBF | Both converged cleanly; RBF wins on higher LML. |
| 08 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML. |

**Function_05 is the sole doubly-hypothetical case in this Week_03 batch**
(matches this project's own README/CODE_REVIEW_SUMMARY.md claim, verified).
Because neither kernel was judged reliably converged this week, Function_05's
retrospective notebook re-fits **both** RBF and Matérn, clearly separated,
and every acquisition-function value computed for Function_05 carries an
explicit "doubly hypothetical" caveat: (1) retrospective/counterfactual/
unevaluated like every other function here, AND (2) computed from a
surrogate that was not itself judged reliable this week. No function in the
Week_03 batch produced the strict "neither reliable, equal ABNORMAL counts"
outcome — Function_05's case is an unequal-count "prefer fewer ABNORMAL"
resolution that still carries the instability caveat because neither count
was zero.

## 4. Methodology, formulas, and parameters

**Fresh, independent GP refit** per function/kernel (does not reuse any
fitted model object from the existing `01_Notebook` diagnostics):

```
kernel = ConstantKernel(1.0, (1e-2, 1e2)) * (RBF | Matern(nu=2.5, length_scale_bounds=(1e-2,1e2)))
       + WhiteKernel(noise_level=1e-2, noise_level_bounds=(1e-10, 1e1))
GaussianProcessRegressor(alpha=1e-10, n_restarts_optimizer=5, normalize_y=False,
                          random_state=42 + FUNCTION_NUMBER)
```

`y` is standardised (zero mean, unit variance, `ddof=0`) before fitting; the
posterior mean/std are inverse-transformed back to raw output units before
any acquisition value is computed, since acquisition functions must operate
on raw output units and `f_best` is itself a raw value.

`f_best` = each function's own best-observed **raw** output within its own
Week_03 cumulative dataset only (no lookahead).

**Acquisition formulas** (`Phi`/`phi` = standard normal CDF/PDF), all
oriented for maximisation:

- **EI**, `xi = 0.01` (raw output units):
  `improvement(x) = mu(x) - f_best - xi`
  `z(x) = improvement(x)/sigma(x)` (0 if `sigma(x) == 0`)
  `EI(x) = improvement(x)*Phi(z(x)) + sigma(x)*phi(z(x))` if `sigma(x) > 0`, else `0`
- **UCB**, `kappa = 2.0`: `UCB(x) = mu(x) + kappa*sigma(x)`
- **PI**: `PI(x) = Phi(z(x))`, same `z`/`xi` as EI

**Optimisation of each acquisition function** over `[0.000000, 0.999999]^d`:

- d = 2 (Functions 01, 02): fine regular grid, `401 x 401` points
  (160,801 evaluations), arg-max taken directly.
- d ≥ 3 (Functions 03–08): multi-start `scipy.optimize.minimize`
  (L-BFGS-B, bounds enforced) on the negative acquisition value, with **50**
  seeded random starts (≥ the required 20) drawn from
  `np.random.default_rng(ACQ_SEED)`, where
  `ACQ_SEED = 3000 + 42 + FUNCTION_NUMBER` (the `3000` offset is
  Week_03-specific, distinct from Week_02's `2000` offset). The best
  (highest raw acquisition value) result across all 50 starts is kept per
  acquisition function per kernel.

Recommended points are reported rounded to exactly six decimal places,
clamped to `[0.000000, 0.999999]`, hyphen-separated (matching the project's
official submission format) — for illustration only, since none was
submitted.

## 5. Results table

> Reminder: every point below is **retrospective, counterfactual, and
> unevaluated** — a hypothetical acquisition-function arg-max computed from
> a fresh GP refit on already-existing Week_03 data, never queried, with no
> associated real output, and never submitted anywhere. Function_05's row is
> additionally **doubly hypothetical** (surrogate itself not judged
> reliable this week).

| Fn | Kernel(s) fit | EI recommended point (value) | UCB recommended point (value) | PI recommended point (value) | EI–UCB dist. | EI–PI dist. | UCB–PI dist. |
|---|---|---|---|---|---|---|---|
| 01 | RBF | 0.287500-0.504999 (0.000000) | 0.395000-0.207500 (0.001800) | 0.342500-0.504999 (0.000000) | 0.316326 | 0.055000 | 0.302097 |
| 02 | RBF | 0.694999-0.000000 (0.015191) | 0.694999-0.000000 (0.715708) | 0.694999-0.617499 (0.393099) | 0.000000 | 0.617499 | 0.617499 |
| 03 | RBF | 0.883904-0.999999-0.431425 (0.041287) | 0.017133-0.000000-0.000000 (0.146880) | 0.941392-0.638818-0.377986 (0.907835) | 1.391912 | 0.369611 | 1.185419 |
| 04 | Matérn | 0.433452-0.412610-0.393214-0.397985 (2.388761) | 0.408951-0.402010-0.369287-0.426258 (-0.210903) | 0.502587-0.428489-0.407987-0.298885 (1.000000) | 0.045656 | 0.122763 | 0.164895 |
| 05 † RBF (primary) | RBF, Matérn | 0.000000-0.999999-0.999999-0.999999 (765.002022) | 0.000000-0.000000-0.999999-0.999999 (2002.534729) | 0.263578-0.484211-0.999999-0.999999 (1.000000) | 0.999999 | 0.579232 | 0.551302 |
| 05 † Matérn (secondary, exploratory) | (see above) | 0.000000-0.745753-0.999999-0.999999 (595.655221) | 0.000000-0.000000-0.999999-0.999999 (1923.974770) | 0.243258-0.336828-0.995632-0.992057 (1.000000) | — | — | — |
| 06 | Matérn | 0.406747-0.000000-0.993538-0.999999-0.000000 (0.400184) | 0.343817-0.000000-0.999999-0.999999-0.000000 (0.353962) | 0.506216-0.217568-0.707886-0.881940-0.000000 (0.943117) | 0.063261 | 0.390851 | 0.415905 |
| 07 | RBF | 0.626129-0.256120-0.238397-0.251963-0.395780-0.983203 (0.033723) | 0.025387-0.526584-0.223431-0.220252-0.359778-0.999999 (1.567995) | 0.029263-0.494025-0.236640-0.241484-0.413072-0.772178 (0.706535) | 0.660946 | 0.676603 | 0.237577 |
| 08 | Matérn | 0.070863-0.071706-0.117734-0.097495-0.999999-0.433124-0.211296-0.999999 (0.061686) | 0.000000-0.000000-0.000000-0.452635-0.999999-0.589129-0.000000-0.999999 (10.241469) | 0.130057-0.138636-0.134688-0.071944-0.999999-0.482207-0.216396-0.846242 (0.894716) | 0.468115 | 0.187083 | 0.530190 |

† Function_05: distances shown are for the primary (RBF) kernel only, consistent with each notebook's own agreement check.

## 6. Agreement vs. divergence, per function (descriptive only)

- **Function_01**: EI and PI agree closely (distance 0.055) — both land in
  a narrow high-uncertainty band; UCB diverges somewhat (~0.30–0.32 from
  either). Note: this function's raw output scale is extremely small
  (values ranging ~1e-124 to ~1e-16, `f_best ≈ 7.7e-16`); the fixed
  `xi = 0.01` (raw units) is roughly 10x this function's output standard
  deviation, so EI/PI collapse to ≈0 almost everywhere except where
  posterior std is largest — a direct, transparent consequence of the
  fixed-raw-unit `xi`, not an error.
- **Function_02**: EI and UCB coincide exactly (distance 0.000000); PI
  diverges to a different point along the same edge. All three sit on the
  boundary `x1 = 0.694999`.
- **Function_03**: All three diverge substantially (largest EI–UCB distance
  in the whole table, 1.39) — EI/PI cluster near one corner region, UCB
  favours a different, near-origin region.
- **Function_04**: All three agree closely (largest pairwise distance only
  0.165) — a tight cluster near the function's existing best-observed
  region.
- **Function_05 (doubly hypothetical)**: EI and UCB diverge substantially
  (distance ≈ 1.0, the largest possible along a single axis at this bound);
  PI sits between them. RBF and Matérn broadly agree on holding the last
  two dimensions at the upper bound but disagree on the first two —
  consistent with this function's underlying convergence instability.
- **Function_06**: EI and UCB agree closely (distance 0.063); PI diverges
  more (~0.39–0.42) toward a less extreme point.
- **Function_07**: EI diverges from both UCB and PI (~0.66–0.68); UCB and PI
  are comparatively closer to each other (0.238).
- **Function_08**: Moderate divergence across all three (0.19–0.53); UCB
  recommends the most boundary-heavy point (three coordinates at exactly
  0 or 0.999999).

These are purely descriptive, retrospective observations about where three
different acquisition functions' arg-maxes fall on a single already-fitted
posterior — they are not a claim about which acquisition function is
"better," and none of them was ever evaluated against a real output.

## 7. Limitations (in addition to the doubly-hypothetical caveat for Function_05)

- In-sample-only GP fits on very small datasets (12–42 points); the same
  limitations already documented in each function's own
  `01_Notebook/week_03_gp_diagnostics.ipynb` and `WEEK_03_METHOD.md` apply
  here too (small-sample hyperparameter instability, no held-out
  validation).
- `xi = 0.01` and `kappa = 2.0` are fixed, project-specified parameters in
  **raw output units**; because raw output scales differ by many orders of
  magnitude across the 8 functions (e.g. Function_01's outputs are ~1e-16
  vs. Function_05's outputs in the hundreds/thousands), the practical
  exploration/exploitation behaviour of EI/PI relative to each function's
  own output range is not uniform across functions. This is a transparent
  consequence of following the specified formulas exactly, not an error.
- Grid search (d=2) and 50-start L-BFGS-B (d≥3) are both approximate global
  optimisers; neither guarantees the true global arg-max of a non-convex
  acquisition surface, though both are standard, reasonable choices at this
  scale.
- None of these recommended points has any bearing on, and must never be
  presented as connected to, any genuine historical query-output pair in
  this project.

## 8. Explicit non-claims

- No point in this document was ever queried, submitted, or evaluated.
- No genuine historical output is claimed or implied for any point in this
  document.
- No existing file (notebook, dataset, or report) anywhere in the project
  was modified to produce this document.
- No Week_02, Week_04, or Week_05 file was used, read, or referenced.
- No `03_Queries` folder was created or referenced under
  `Historical_Replay/Week_03`.
