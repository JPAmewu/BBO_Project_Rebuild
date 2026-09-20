# Week 5 Retrospective Acquisition-Function Analysis — Summary

> **RETROSPECTIVE, COUNTERFACTUAL, AND UNEVALUATED.** Every recommended
> point in this document is a *what-if* computation over data already
> chronologically available at the start of Historical_Replay Week_05. **No
> point listed here was ever actually queried, has any associated real
> output, or was submitted anywhere.** This document is entirely separate
> from, and does not modify, any genuine historical query/output pair or
> any of the existing `week_05_gp_diagnostics.ipynb` notebooks in this
> track. Nothing in this document should be read as, or associated with, a
> genuine result of this project.

**Scope:** Historical_Replay, **Week_05 only**. This analysis does not
touch, use, or reference Week_02, Week_03, or Week_04 in any way, and does
not modify any existing dataset, notebook, or report.

## 1. Why this analysis exists

Every previously-verified `week_05_gp_diagnostics.ipynb` notebook in this
track (one per function, in `Function_0X/01_Notebook/`) explicitly stops at
visualising the GP posterior — no notebook in this project has, until now,
computed an acquisition function or proposed a next query point anywhere.
This analysis answers, purely retrospectively: *given only the data
available at the start of Week_05, what would Expected Improvement (EI),
Upper Confidence Bound (UCB), and Probability of Improvement (PI) each have
recommended as the next query, per function?*

New artifacts created by this analysis (purely additive, nothing else was
touched):

- `Function_0X/08_Retrospective_Acquisition/week_05_retrospective_acquisition.ipynb`
  (X = 01..08) — one notebook per function, each with its own executed
  outputs and figures in a sibling `figures/` folder.
- This document.

## 2. Data boundary (no lookahead)

Each function's notebook loads **only** its own already-existing
`Function_0X/02_Data/cumulative_inputs.npy` / `cumulative_outputs.npy` — the
exact same files already used (read-only) by that function's own
`week_05_gp_diagnostics.ipynb`. No Week_06+ file, no Week_02/Week_03/Week_04
file, and no historical pair beyond what is already inside each function's
own Week_05 cumulative dataset was used. Row counts / dimensionalities
(independently re-verified directly from the `.npy` files during this
analysis):

| Function | n (rows) | d (dims) |
|---|---|---|
| 01 | 14 | 2 |
| 02 | 14 | 2 |
| 03 | 19 | 3 |
| 04 | 34 | 4 |
| 05 | 24 | 4 |
| 06 | 24 | 5 |
| 07 | 34 | 6 |
| 08 | 44 | 8 |

All inputs fall within the official bounds `[0.000000, 0.999999]`; all 8
functions are maximisation problems (CLAUDE.md rules 16–17).

## 3. Step 1 — Week_05's own kernel-selection outcome (per function)

Determined by reading `../CODE_REVIEW_SUMMARY.md`, and independently
confirmed against each function's own executed
`../Function_0X/01_Notebook/week_05_gp_diagnostics.ipynb` and (for
Functions 01–04) their already-complete retrospective notebooks. All 8
matched exactly:

| Function | RBF ABNORMAL | Matérn ABNORMAL | Week_05 selected kernel | Notes |
|---|---|---|---|---|
| 01 | 0/6 | 0/6 | RBF | Both converged cleanly; RBF wins on higher LML. |
| 02 | 0/6 | 0/6 | RBF | Both converged cleanly; RBF wins on higher LML. |
| 03 | 0/6 | 0/6 | RBF | Both converged cleanly; RBF wins on higher LML. |
| 04 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML. |
| 05 | 4/6 | 4/6 | **Neither** | **Strict tie, non-zero on both sides.** Per the project's own convergence-aware rule, neither kernel is selected as "the model" this week; both are reported as exploratory only. Compared to Week_04 (RBF 5/6, Matérn 3/6), this is a genuinely mixed result — RBF improved by one restart, Matérn worsened by one restart; the underlying instability is not resolved either way. |
| 06 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML. |
| 07 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML — kernel preference **held steady from Week_04** (an ordinary LML tie-break outcome on two cleanly-converged fits, not an instability finding). |
| 08 | 0/6 | 0/6 | Matérn | Both converged cleanly; Matérn wins on higher LML. |

**Function_05 is the sole doubly-hypothetical case in this Week_05 batch.**
Unlike Week_04's Function_05 (an *unequal* 5/6 vs. 3/6 split, which still
allowed a "marginally more convergence-reliable" kernel to be named),
Week_05's Function_05 shows an *exactly equal*, non-zero ABNORMAL count on
both kernels (4/6 each) — a strict tie. Per the project's own
convergence-aware selection rule, this means **neither kernel is preferred
even marginally**; both RBF and Matérn are re-fitted and reported side by
side in that function's notebook, with **neither designated "primary"**
(a stricter framing than Week_04's Function_05 case). Every
acquisition-function value computed for Function_05 carries an explicit
"doubly hypothetical" caveat: (1) retrospective/counterfactual/unevaluated
like every other function here, AND (2) computed from a surrogate that was
not itself judged reliable this week.

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
Week_05 cumulative dataset only (no lookahead).

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
  `ACQ_SEED = 5000 + 42 + FUNCTION_NUMBER` (the `5000` offset is
  Week_05-specific, distinct from Week_02's `2000`, Week_03's `3000`, and
  Week_04's `4000` offsets). The best (highest raw acquisition value)
  result across all 50 starts is kept per acquisition function per kernel.
  Convergence was 49/50 or 50/50 in every case (see Section 6 note for
  Function_05).

Recommended points are reported rounded to exactly six decimal places,
clamped to `[0.000000, 0.999999]`, hyphen-separated (matching the project's
official submission format) — for illustration only, since none was
submitted.

**Function_01 output-scale note:** this function's raw outputs are
extremely small (`f_best ≈ 7.71e-16`, achieved at input `[0.731024,
0.733000]`). The fixed `xi = 0.01` (raw units) therefore dominates the
improvement term almost everywhere, causing EI/PI to collapse to ≈0 across
most of the grid — expected and already observed in the Week_02/03/04
versions of this analysis, not a bug.

## 5. Results table

> Reminder: every point below is **retrospective, counterfactual, and
> unevaluated** — a hypothetical acquisition-function arg-max computed from
> a fresh GP refit on already-existing Week_05 data, never queried, with no
> associated real output, and never submitted anywhere. Function_05's rows
> are additionally **doubly hypothetical** (surrogate itself not judged
> reliable this week, and — unlike prior weeks' Function_05 cases — neither
> kernel is even marginally preferred).

| Fn | Kernel(s) fit | EI recommended point (value) | UCB recommended point (value) | PI recommended point (value) | EI–UCB dist. | EI–PI dist. | UCB–PI dist. |
|---|---|---|---|---|---|---|---|
| 01 | RBF | 0.507499-0.495000 (0.000000) | 0.797499-0.519999 (0.001715) | 0.509999-0.495000 (0.000000) | 0.291075 | 0.002500 | 0.288585 |
| 02 | RBF | 0.694999-0.000000 (0.014161) | 0.694999-0.000000 (0.712240) | 0.694999-0.624999 (0.378186) | 0.000000 | 0.624999 | 0.624999 |
| 03 | RBF | 0.000000-0.487504-0.000000 (0.041320) | 0.000000-0.000000-0.000000 (0.126752) | 0.721770-0.548029-0.375319 (0.999716) | 0.487504 | 0.815770 | 0.980894 |
| 04 | Matérn | 0.406153-0.395044-0.385550-0.407777 (1.024086) | 0.388752-0.401322-0.420232-0.418357 (-0.061836) | 0.414015-0.385903-0.339941-0.411815 (0.998260) | 0.040706 | 0.047348 | 0.085822 |
| 05 † RBF | RBF, Matérn (neither preferred) | 0.000000-0.999999-0.999999-0.999999 (804.367024) | 0.000000-0.000000-0.999999-0.999999 (2039.934775) | 0.662031-0.651578-0.899750-0.972402 (1.000000) | 0.999999 | 0.755310 | 0.934692 |
| 05 † Matérn | (see above) | 0.000000-0.725467-0.999999-0.999999 (532.054552) | 0.000000-0.000000-0.999999-0.999999 (1874.736633) | 0.662031-0.651578-0.899750-0.972402 (1.000000) | 0.725467 | 0.674208 | 0.934692 |
| 06 | Matérn | 0.399714-0.000000-0.999999-0.999999-0.000000 (0.454958) | 0.339532-0.000000-0.999999-0.999999-0.000000 (0.364468) | 0.497342-0.188047-0.726626-0.914058-0.000000 (0.967100) | 0.060182 | 0.356387 | 0.377339 |
| 07 | Matérn | 0.000000-0.238778-0.831137-0.084167-0.371077-0.910862 (0.184336) | 0.000000-0.222245-0.766748-0.000000-0.359956-0.999999 (2.730239) | 0.129881-0.283758-0.614101-0.176600-0.390393-0.828390 (0.990473) | 0.139902 | 0.285859 | 0.324834 |
| 08 | Matérn | 0.465880-0.204112-0.061384-0.163995-0.710741-0.914044-0.334119-0.557904 (0.000000) | 0.000000-0.000000-0.142309-0.000000-0.999999-0.271789-0.073096-0.999999 (10.182938) | 0.465880-0.204112-0.061384-0.163995-0.710741-0.914044-0.334119-0.557904 (0.000000) | 1.025618 | 0.000000 | 1.025618 |

† Function_05: unlike Week_04's version of this table (which reported one
kernel as "primary" for the pairwise-distance columns, since Week_04's
5/6-vs-3/6 split allowed a marginally-preferred kernel), Week_05's exact
4/6-vs-4/6 tie means **neither RBF nor Matérn is privileged**. Both rows are
reported in full, each with its own internally-consistent EI–UCB / EI–PI /
UCB–PI distances (computed from that kernel's own three recommended
points), and neither should be read as a dependable proposal.

## 6. Agreement vs. divergence, per function (descriptive only)

- **Function_01**: EI and PI agree closely (distance 0.0025) — both land in
  a narrow, near-zero-value band consistent with the output-scale note in
  Section 4. UCB diverges more (~0.29 from either), favouring a
  higher-uncertainty point.
- **Function_02**: EI and UCB coincide exactly (distance 0.000000); PI
  diverges to a different point along the same edge (`x1 = 0.694999`
  shared by all three).
- **Function_03**: EI and UCB are moderately close (0.4875, both hugging
  the origin corner on two of three dimensions), while PI diverges
  substantially (0.82–0.98) to an interior point with near-certain (0.9997)
  probability of improvement.
- **Function_04**: All three agree closely (largest pairwise distance only
  0.086) — a tight cluster near the function's existing best-observed
  region.
- **Function_05 (doubly hypothetical, neither kernel preferred)**: For
  both RBF and Matérn, EI and UCB diverge substantially from each other
  (0.72–1.0) while PI is identical between the two kernels
  (`0.662031-0.651578-0.899750-0.972402`, value 1.000000) — the GP posterior
  is saturated with near-certain improvement probability over a wide region
  for both kernels at this function's raw output scale (in the hundreds).
  Both kernels agree closely with each other on where EI/UCB fall along the
  last two dimensions (holding at the upper bound), consistent with this
  function's own diagnosed convergence instability rather than a clean
  agreement signal.
- **Function_06**: EI and UCB agree fairly closely (distance 0.060); PI
  diverges more (~0.36–0.38) toward a less extreme, more interior point.
- **Function_07**: EI and UCB are the closest pair (0.140); all three show
  moderate, roughly comparable divergence overall (0.14–0.32).
- **Function_08**: EI and PI coincide exactly (distance 0.000000, both
  reporting acquisition value 0.000000 — the GP posterior favours very
  little predicted improvement over `f_best` at that point at this
  function's raw output scale); UCB diverges substantially from both
  (1.026), favouring a boundary-heavy point (five coordinates at exactly 0
  or 0.999999).

These are purely descriptive, retrospective observations about where three
different acquisition functions' arg-maxes fall on a single already-fitted
posterior — they are not a claim about which acquisition function is
"better," and none of them was ever evaluated against a real output.

## 7. Limitations (in addition to the doubly-hypothetical caveat for Function_05)

- In-sample-only GP fits on very small datasets (14–44 points); the same
  limitations already documented in each function's own
  `01_Notebook/week_05_gp_diagnostics.ipynb` apply here too (small-sample
  hyperparameter instability, no held-out validation).
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
- Function_05's ABNORMAL-restart instability has now persisted across four
  consecutive weeks (Weeks 2–5), per `../CODE_REVIEW_SUMMARY.md`; this
  week's exact 4/6-vs-4/6 tie is the least resolved of the four.
- None of these recommended points has any bearing on, and must never be
  presented as connected to, any genuine historical query-output pair in
  this project.

## 8. Explicit non-claims

- No point in this document was ever queried, submitted, or evaluated.
- No genuine historical output is claimed or implied for any point in this
  document.
- No existing file (notebook, dataset, or report) anywhere in the project
  was modified to produce this document.
- No Week_02, Week_03, or Week_04 file was used, read, or referenced.
- No `03_Queries` folder was created or referenced under
  `Historical_Replay/Week_05`.
