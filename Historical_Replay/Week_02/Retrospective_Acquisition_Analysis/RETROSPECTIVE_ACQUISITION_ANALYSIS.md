# Retrospective Acquisition-Function "What-If" Analysis — Historical_Replay / Week_02

> **RETROSPECTIVE, COUNTERFACTUAL, AND UNEVALUATED.** Everything in this
> document describes what Expected Improvement (EI), Upper Confidence Bound
> (UCB), and Probability of Improvement (PI) would each have recommended as
> the next query point for each of the 8 functions, computed **after the
> fact**, using **only** the data chronologically available at the start of
> Week_02. None of the recommended points below were ever actually queried.
> None has a real associated output. None was ever submitted anywhere. This
> document is **not** connected to, and must never be confused with, any
> genuine historical query/output pair, any real Week_02/Week_03/Week_04/
> Week_05 result, or any of this project's actual submitted queries. It is a
> purely additive, illustrative analysis layer over a week
> (`Historical_Replay/Week_02`) that was already independently verified
> (`FINAL_EVALUATION.md`, Check 12) to have deliberately stopped short of
> applying any acquisition function.

## Scope and provenance

- **New files only.** This document, plus one new notebook and figure(s) per
  function under each `Function_0X/08_Retrospective_Acquisition/` folder,
  are the only artifacts created by this analysis. No existing file anywhere
  in the project (dataset, notebook, report, query, or result) was modified,
  renamed, or deleted.
- **No lookahead.** Each function's notebook loads only its own
  `Historical_Replay/Week_02/Function_0X/02_Data/cumulative_inputs.npy` /
  `cumulative_outputs.npy` — the identical data already used by the
  existing, independently-verified
  `Function_0X/01_Notebook/week_02_eda_gp_surrogate.ipynb`. Row counts: 11,
  11, 16, 31, 21, 21, 31, 41 for Functions 01–08 (confirmed by direct load).
  No Week_03, Week_04, or Week_05 file was read or created.
- **Kernel selection is Week_02's own, not re-decided here.** For each
  function, the kernel (RBF or Matern) used below is the one Week_02's own
  `01_Notebook/week_02_eda_gp_surrogate.ipynb` actually selected, determined
  by directly reading that notebook's own Section 6a code-cell output (see
  table below). This notebook's own fresh, independent refit (same kernel
  construction, seed, and restart count) reproduced each function's original
  log-marginal-likelihood (LML) values exactly, confirming determinism
  before any acquisition value was computed.

## Methodology (identical across all 8 functions)

**GP construction:** `ConstantKernel(1.0, (1e-2, 1e2)) * (RBF | Matern(nu=2.5)) + WhiteKernel(1e-2, (1e-10, 1e1))`,
`alpha=1e-10`, `n_restarts_optimizer=5`, `random_state = 42 + function_number`
(same convention as every other notebook in this project).

**Standardization:** `y_scaled = (y - mean(y)) / std(y)` before fitting;
`mu, sigma` inverse-transformed back to raw output units
(`mu*std+mean`, `sigma*std`) before any acquisition value is computed,
since acquisition functions must operate on the real output scale.

**`f_best`:** the maximum observed output in that function's own Week_02
cumulative dataset (maximisation convention, CLAUDE.md rule 17).

**Acquisition functions (raw output units):**
- **EI** (`xi = 0.01`): `improvement = mu - f_best - xi`; `z = improvement/sigma` (`0` if `sigma == 0`);
  `EI = improvement*Phi(z) + sigma*phi(z)` if `sigma > 0`, else `EI = 0`.
- **UCB** (`kappa = 2.0`): `UCB = mu + kappa*sigma`.
- **PI**: `PI = Phi(z)`, same `z`/`xi` as EI.

**Maximisation over `[0.000000, 0.999999]^d`:**
- d = 2 (Functions 01, 02): fine grid, 400 x 400 = 160,000 points.
- d >= 3 (Functions 03–08): multi-start `scipy.optimize.minimize`
  (`L-BFGS-B`, bounds enforced), 25 seeded random starts
  (`np.random.default_rng(2000 + 42 + function_number)`), best result kept.

**Agreement rule:** EI/UCB/PI recommended points are called "agree" if their
pairwise Euclidean distance in `[0,1)^d` is `< 0.1`; otherwise "diverge".
This is an interpretive convenience defined for this analysis only, not a
formal statistical test.

## Step 1 — Week_02's own kernel-selection outcome, and Step 2 results

**Every row below is RETROSPECTIVE, COUNTERFACTUAL, AND UNEVALUATED. No
recommended point has ever been queried or has a real output.**

| Fn | d | Kernel selected by Week_02's own process | Selection basis | EI recommends | UCB recommends | PI recommends | Agree/diverge |
|----|---|---|---|---|---|---|---|
| 01 | 2 | RBF (LML −14.808 vs Matern −15.008) | default: higher LML, both converged cleanly | `0.040100-0.401002` (val 0.000000) | `0.704260-0.000000` (val 0.001868) | `0.040100-0.403508` (val 0.000000) | EI & PI agree (dist 0.0025); UCB diverges from both |
| 02 | 2 | RBF (LML −11.228 vs Matern −11.407) | default: higher LML, both converged cleanly | `0.696741-0.000000` (val 0.014861) | `0.949874-0.814536` (val 0.724579) | `0.696741-0.481203` (val 0.381336) | all three diverge from each other |
| 03 | 3 | RBF (LML −12.532 vs Matern −14.008) | default: higher LML, both converged cleanly | `0.000000-0.000000-0.000000` (val 0.058918) | `0.000000-0.000000-0.000000` (val 0.204995) | `0.752617-0.570528-0.375473` (val 0.993205) | EI & UCB agree (identical point); PI diverges from both |
| 04 | 4 | Matern (nu=2.5) (LML −17.814 vs RBF −18.652) | default: higher LML, both converged cleanly | `0.013883-0.022180-0.839636-0.026989` (val 0.000000) | `0.408941-0.409307-0.368998-0.425895` (val −0.190382) | `0.013869-0.022177-0.839638-0.026979` (val 0.000000) | EI & PI agree (dist ~0.00002); UCB diverges from both |
| 05 | 4 | RBF (LML −1.334 vs Matern −0.910) | **convergence-aware override**: RBF 1/6 ABNORMAL restarts vs Matern 3/6 — RBF selected despite lower raw LML. **Doubly-caveated (see below)** | `0.000000-0.999999-0.999999-0.999999` (val 604.087934) | `0.000000-0.000000-0.999999-0.999999` (val 1966.588393) | `0.001224-0.942404-0.999999-0.977385` (val 1.000000) | EI & PI agree (dist ~0.062); UCB diverges from both |
| 06 | 5 | RBF (LML −22.903 vs Matern −22.977) | default: higher LML, both converged cleanly | `0.408693-0.137917-0.790443-0.999999-0.000000` (val 0.639910) | `0.333623-0.000000-0.999999-0.999999-0.000000` (val 0.642299) | `0.512144-0.281948-0.652309-0.949011-0.000000` (val 0.993407) | all three diverge from each other |
| 07 | 6 | RBF (LML −33.096 vs Matern −33.420) | default: higher LML, both converged cleanly | `0.000000-0.072405-0.756269-0.228341-0.379182-0.756878` (val 0.024556) | `0.000000-0.064421-0.874331-0.175099-0.359981-0.781076` (val 1.552535) | `0.000000-0.385038-0.295710-0.235438-0.393705-0.747853` (val 0.400563) | all three diverge from each other |
| 08 | 8 | Matern (nu=2.5) (LML 1.529 vs RBF −4.904) | default: higher LML, both converged cleanly | `0.029702-0.020792-0.189964-0.000000-0.999999-0.532228-0.127797-0.000000` (val 0.306177) | `0.000000-0.000000-0.247380-0.000000-0.999999-0.585749-0.000000-0.000000` (val 10.489135) | `0.239318-0.245702-0.076233-0.039700-0.999999-0.270212-0.134839-0.331245` (val 1.000000) | all three diverge from each other |

*Caption: all recommended points and acquisition values in this table are
RETROSPECTIVE, COUNTERFACTUAL, AND UNEVALUATED — computed after the fact
from Week_02's own already-cumulative data only, never queried, and with no
real associated output.*

### Function_05 — doubly-hypothetical caveat

Function_05 is the only one of the 8 functions where Week_02's own notebook
found genuine GP hyperparameter-optimizer instability (L-BFGS-B `ABNORMAL`
terminations: 1/6 restarts for RBF, 3/6 for Matern). RBF was selected via
Week_02's own documented convergence-aware override rule (prefer fewer
`ABNORMAL` terminations), not because it had the higher raw
log-marginal-likelihood — it does not. Because Week_02's own notebook is
explicit that **neither kernel for Function_05 should be trusted with the
same confidence as the other 7 functions**, this function's EI/UCB/PI
recommendations above are **hypothetical relative to an already
convergence-caveated surrogate** — a strictly weaker basis than the other 7
functions' recommendations, which rest on cleanly-converged fits. None of
the other 7 functions showed any `ABNORMAL` optimizer termination in
Week_02's own notebooks, so none of them falls into this doubly-hypothetical
case.

## Overall observations (descriptive only)

- **Kernel selection by Week_02's own process:** RBF for Functions 01, 02,
  03, 05 (via override), 06, 07; Matern (nu=2.5) for Functions 04 and 08.
- **Fresh-refit determinism check:** for all 8 functions, this notebook's
  independent refit reproduced Week_02's own notebook's RBF and Matern
  log-marginal-likelihood values exactly (`np.isclose` — True in all 16
  comparisons), confirming the fixed-seed pipeline is genuinely
  deterministic before any acquisition value was computed on top of it.
- **EI/PI often (not always) land in the same region; UCB diverges from
  both in every function.** Across all 8 functions, EI and PI agreed
  (distance < 0.1) in 3 of 8 cases (Functions 01, 04, 05) — expected in
  those cases, since both are driven by the same `z`-score under
  `f_best`/`xi`. In Function_03, it was instead EI and UCB that
  coincidentally agreed (both landed exactly at `0.000000-0.000000-0.000000`),
  with PI diverging from both. UCB never landed within 0.1 distance of PI in
  any of the 8 functions, and only coincided with EI in Function_03. This is
  a descriptive pattern observed in this specific computation, not a general
  claim about EI/UCB/PI behaviour.
- **Higher-dimensional functions (06, 07, 08) show three-way divergence.**
  For d = 5, 6, and 8, all three acquisition functions recommended points
  more than 0.1 apart from one another in every pairwise comparison —
  consistent with a much larger, sparser search space relative to the
  number of observations, though this is a descriptive observation from
  this specific run, not a proven general property.

## Reminder

**RETROSPECTIVE, COUNTERFACTUAL, AND UNEVALUATED.** No point in this
document was ever queried. No point in this document has a real output. No
point in this document was ever submitted. This document does not modify,
and must not be read as connected to, any genuine historical result
anywhere in this project.
