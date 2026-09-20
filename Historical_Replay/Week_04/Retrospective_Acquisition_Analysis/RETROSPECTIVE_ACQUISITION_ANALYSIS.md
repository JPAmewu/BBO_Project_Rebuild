# Week 4 Retrospective Acquisition-Function Analysis — Summary

> **RETROSPECTIVE, COUNTERFACTUAL, AND UNEVALUATED.** Every recommended
> point in this document is a *what-if* computation over data already
> chronologically available at the start of Historical_Replay Week_04. **No
> point listed here was ever actually queried, has any associated real
> output, or was submitted anywhere.** This document is entirely separate
> from, and does not modify, any genuine historical query/output pair,
> `pair_provenance.md`, `VERIFIED_PAIRS.csv`, or any of the existing
> `week_04_gp_diagnostics.ipynb` notebooks in this track. Nothing in this
> document should be read as, or associated with, a genuine result of this
> project.

**Scope:** Historical_Replay, **Week_04 only**. This analysis does not
touch, use, or reference Week_02, Week_03, or Week_05 in any way, and does
not modify any existing dataset, notebook, or report.

## 1. Why this analysis exists

Every previously-verified `week_04_gp_diagnostics.ipynb` notebook in this
track (one per function, in `Function_0X/01_Notebook/`) explicitly stops at
visualising the GP posterior — no notebook in this project has, until now,
computed an acquisition function or proposed a next query point anywhere.
This analysis answers, purely retrospectively: *given only the data
available at the start of Week_04, what would Expected Improvement (EI),
Upper Confidence Bound (UCB), and Probability of Improvement (PI) each have
recommended as the next query, per function?*

New artifacts created by this analysis (purely additive, nothing else was
touched):

- `Function_0X/08_Retrospective_Acquisition/week_04_retrospective_acquisition.ipynb`
  (X = 01..08) — one notebook per function, each with its own executed
  outputs and figures in a sibling `figures/` folder.
- This document.

## 2. Data boundary (no lookahead)

Each function's notebook loads **only** its own already-existing
`Function_0X/02_Data/cumulative_inputs.npy` / `cumulative_outputs.npy` — the
exact same files already used (read-only) by that function's own
`week_04_gp_diagnostics.ipynb`. No Week_05+ file, no Week_02/Week_03 file,
and no historical pair beyond what is already inside each function's own
Week_04 cumulative dataset was used. Row counts / dimensionalities
(independently re-verified directly from the `.npy` files during this
analysis):

| Function | n (rows) | d (dims) |
|---|---|---|
| 01 | 13 | 2 |
| 02 | 13 | 2 |
| 03 | 18 | 3 |
| 04 | 33 | 4 |
| 05 | 23 | 4 |
| 06 | 23 | 5 |
| 07 | 33 | 6 |
| 08 | 43 | 8 |

All inputs fall within the official bounds `[0.000000, 0.999999]`; all 8
functions are maximisation problems (CLAUDE.md rules 16–17).

## 3. Step 1 — Week_04's own kernel-selection outcome (per function)

Determined by reading `../CODE_REVIEW_SUMMARY.md`, and independently
confirmed against each function's own executed
`../Function_0X/01_Notebook/week_04_gp_diagnostics.ipynb` (specifically its
per-restart diagnostic printout and final "DECISION:" line). All 8 matched
exactly:

| Function | RBF ABNORMAL | Matérn ABNORMAL | Week_04 selected kernel | Notes |
|---|---|---|---|---|
| 01 | 0/6 | 0/6 | RBF | Both converged cleanly; RBF wins on higher LML. |
| 02 | 0/6 | 0/6 | RBF | Both converged cleanly; RBF wins on higher LML. |
| 03 | 0/6 | 0/6 | RBF | Both converged cleanly; RBF wins on higher LML. |
| 04 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML. |
| 05 | 5/6 | 3/6 | Matérn | **Neither kernel converged cleanly.** Matérn selected only as the marginally more convergence-reliable of the two, per the project's own rule. Mixed picture vs. Week_03 (RBF worsened 3/6→5/6, Matérn improved 5/6→3/6) — instability not resolved either way. |
| 06 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML. |
| 07 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML — kernel preference **flipped from RBF in Week_03**, an ordinary LML tie-break outcome for two cleanly-converged kernels this week, **not** an instability case. |
| 08 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML. |

**Function_05 is the sole doubly-hypothetical case in this Week_04 batch**
(matches this project's own README/`CODE_REVIEW_SUMMARY.md` claim, verified
directly against the raw stderr `ABNORMAL:` warning counts in
`../Function_05/01_Notebook/week_04_gp_diagnostics.ipynb`: exactly 5 for
RBF, exactly 3 for Matérn). Because neither kernel was judged reliably
converged this week, Function_05's retrospective notebook re-fits **both**
RBF and Matérn, clearly separated, and every acquisition-function value
computed for Function_05 carries an explicit "doubly hypothetical" caveat:
(1) retrospective/counterfactual/unevaluated like every other function
here, AND (2) computed from a surrogate that was not itself judged reliable
this week.

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
Week_04 cumulative dataset only (no lookahead).

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
  `ACQ_SEED = 4000 + 42 + FUNCTION_NUMBER` (the `4000` offset is
  Week_04-specific, distinct from Week_02's `2000` offset and Week_03's
  `3000` offset). The best (highest raw acquisition value) result across
  all 50 starts is kept per acquisition function per kernel. All (or
  nearly all — see Function_08's UCB row, 49/50) starts converged
  successfully in every case.

Recommended points are reported rounded to exactly six decimal places,
clamped to `[0.000000, 0.999999]`, hyphen-separated (matching the project's
official submission format) — for illustration only, since none was
submitted.

**Function_01 output-scale note:** this function's raw outputs range from
roughly `1e-124` to `1e-16` (`f_best ≈ 7.71e-16`). The fixed `xi = 0.01`
(raw units) therefore dominates the improvement term almost everywhere,
causing EI/PI to collapse to ≈0 across most of the grid — expected and
already observed in the Week_02/Week_03 versions of this analysis, not a
bug.

## 5. Results table

> Reminder: every point below is **retrospective, counterfactual, and
> unevaluated** — a hypothetical acquisition-function arg-max computed from
> a fresh GP refit on already-existing Week_04 data, never queried, with no
> associated real output, and never submitted anywhere. Function_05's row is
> additionally **doubly hypothetical** (surrogate itself not judged
> reliable this week).

| Fn | Kernel(s) fit | EI recommended point (value) | UCB recommended point (value) | PI recommended point (value) | EI–UCB dist. | EI–PI dist. | UCB–PI dist. |
|---|---|---|---|---|---|---|---|
| 01 | RBF | 0.417500-0.504999 (0.000000) | 0.982499-0.205000 (0.001736) | 0.455000-0.504999 (0.000000) | 0.639706 | 0.037500 | 0.606841 |
| 02 | RBF | 0.694999-0.000000 (0.014872) | 0.694999-0.000000 (0.714518) | 0.694999-0.624999 (0.389169) | 0.000000 | 0.624999 | 0.624999 |
| 03 | RBF | 0.000000-0.000000-0.000000 (0.052555) | 0.000000-0.000000-0.000000 (0.188076) | 0.765894-0.574554-0.376302 (0.993002) | 0.000000 | 1.028742 | 1.028742 |
| 04 | Matérn | 0.424257-0.403902-0.371216-0.412738 (0.900025) | 0.400013-0.412650-0.408641-0.418608 (-0.360284) | 0.402605-0.368357-0.269316-0.454031 (0.999997) | 0.045819 | 0.117563 | 0.150450 |
| 05 † RBF (primary) | RBF, Matérn | 0.000000-0.999999-0.999999-0.999999 (768.213259) | 0.000000-0.000000-0.999999-0.999999 (2004.807254) | 0.218736-0.547577-0.805223-0.999999 (1.000000) | 0.999999 | 0.538951 | 0.620986 |
| 05 † Matérn (secondary, exploratory) | (see above) | 0.000000-0.707532-0.999999-0.999999 (601.138792) | 0.000000-0.000000-0.999999-0.999999 (1928.737232) | 0.221470-0.544845-0.993615-0.993373 (1.000000) | — | — | — |
| 06 | Matérn | 0.414963-0.000000-0.879574-0.999999-0.000000 (0.411622) | 0.349834-0.000000-0.999999-0.999999-0.000000 (0.328576) | 0.499438-0.128876-0.679404-0.902240-0.000000 (0.946902) | 0.136909 | 0.270869 | 0.389010 |
| 07 | Matérn | 0.013193-0.251247-0.690436-0.095800-0.375877-0.901239 (0.137562) | 0.000000-0.235842-0.683851-0.005796-0.363282-0.990324 (2.648969) | 0.128556-0.285978-0.610805-0.175750-0.391545-0.830505 (0.964105) | 0.129036 | 0.180269 | 0.282137 |
| 08 | Matérn | 0.019714-0.841400-0.014183-0.581788-0.396057-0.779635-0.039683-0.333913 (0.000000) | 0.000000-0.000000-0.168613-0.000000-0.999999-0.227302-0.000000-0.999999 (10.202569) | 0.139242-0.130845-0.144824-0.062388-0.999999-0.471462-0.215354-0.784562 (0.909144) | 1.478422 | 1.224614 | 0.439741 |

† Function_05: distances shown are for the RBF kernel only (the first-fitted
of the two exploratory kernels in this function's notebook), consistent with
each notebook's own agreement-check convention. This is purely a reporting
choice for the pairwise-distance columns — it does **not** mean RBF is "the"
recommendation. Per Step 1, Week_04's own convergence-aware rule actually
selected **Matérn** as the marginally more convergence-reliable of the two
(5/6 vs. 3/6 ABNORMAL restarts); neither kernel converged cleanly, so both
RBF and Matérn rows are reported in full above without privileging either as
a dependable proposal, per the doubly-hypothetical caveat.

## 6. Agreement vs. divergence, per function (descriptive only)

- **Function_01**: EI and PI agree closely (distance 0.0375) — both land in
  a narrow band with near-zero acquisition value almost everywhere (see the
  output-scale note in Section 4). UCB diverges substantially (~0.61–0.64
  from either), favouring a high-uncertainty corner far from the EI/PI
  region.
- **Function_02**: EI and UCB coincide exactly (distance 0.000000); PI
  diverges to a different point along the same edge (`x1 = 0.694999`
  shared by all three).
- **Function_03**: EI and UCB coincide exactly (both at the origin corner,
  distance 0.000000), while PI diverges substantially (distance ≈1.03) to
  an interior point with near-certain (0.993) probability of improvement.
- **Function_04**: All three agree closely (largest pairwise distance only
  0.150) — a tight cluster near the function's existing best-observed
  region.
- **Function_05 (doubly hypothetical)**: EI and UCB diverge substantially
  (distance ≈1.0, the largest possible along a single axis at this bound);
  PI sits between them. RBF and Matérn broadly agree on holding the last
  two dimensions at the upper bound but disagree on the second dimension —
  consistent with this function's underlying convergence instability.
- **Function_06**: EI and UCB agree fairly closely (distance 0.137); PI
  diverges more (~0.27–0.39) toward a less extreme, more interior point.
- **Function_07**: All three show moderate, roughly comparable divergence
  (0.13–0.28); EI and UCB are the closest pair (0.129).
- **Function_08**: Substantial divergence across all three (0.44–1.48,
  the largest overall spread in this week's table); EI collapses to a
  near-zero-value interior point, UCB favours a boundary-heavy point (four
  coordinates at exactly 0 or 0.999999), and PI sits at an intermediate
  point.

These are purely descriptive, retrospective observations about where three
different acquisition functions' arg-maxes fall on a single already-fitted
posterior — they are not a claim about which acquisition function is
"better," and none of them was ever evaluated against a real output.

## 7. Limitations (in addition to the doubly-hypothetical caveat for Function_05)

- In-sample-only GP fits on very small datasets (13–43 points); the same
  limitations already documented in each function's own
  `01_Notebook/week_04_gp_diagnostics.ipynb` and `WEEK_04_METHOD.md` apply
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
- No Week_02, Week_03, or Week_05 file was used, read, or referenced.
- No `03_Queries` folder was created or referenced under
  `Historical_Replay/Week_04`.
