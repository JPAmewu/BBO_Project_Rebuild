# Week 3 Method — Historical Replay: GP Diagnostics

**Track:** Historical Replay (retrospective learning). This document
describes the method used to build the 8
`Historical_Replay/Week_03/Function_0X/01_Notebook/week_03_gp_diagnostics.ipynb`
notebooks (X = 01..08). **This entire track is a retrospective replay of
decisions using already-completed, independently-verified historical data
— it is not a live or real submission of any kind.** No acquisition
function is run and no new query is proposed, selected, or saved anywhere
in this track.

## 1. Data boundary

The only data used anywhere in these 8 notebooks is, per function X:

- `Historical_Replay/Week_03/Function_0X/02_Data/cumulative_inputs.npy`
- `Historical_Replay/Week_03/Function_0X/02_Data/cumulative_outputs.npy`

These arrays were built (prior to and independently of this notebook track)
by taking each function's Week 2 cumulative dataset and appending exactly
one row: the independently-verified genuine historical Week 2 query-output
pair. That construction and its provenance/verification trail already
exist and are independently PASS-verified in
`Historical_Replay/Week_03/CUMULATIVE_DATA_VALIDATION.md` and
`Historical_Replay/Week_03/INDEPENDENT_CUMULATIVE_REVIEW.md`; each notebook
cites this by reference only and does not re-open those files
programmatically, but every notebook does independently (re-)compute its
own basic sanity checks directly on the loaded arrays (shape, dtype,
dimensionality, finiteness, official-bounds compliance, and duplicate-row
checks after rounding to 6 decimal places) rather than trusting that
provenance chain blindly.

**No data from any later week was used.** In particular, no real Week 3
historical query or output was loaded, revealed, or fabricated anywhere —
none exists yet for this project. No `Week_04` location was created or
referenced. Each notebook contains an explicit "Chronological validity
confirmation" markdown cell stating this.

Row counts and dimensionalities used for each function's sanity checks
(confirmed to match the actual loaded array shapes for all 8 functions):

| Function | n (rows) | d (columns) |
|---|---|---|
| 01 | 12 | 2 |
| 02 | 12 | 2 |
| 03 | 17 | 3 |
| 04 | 32 | 4 |
| 05 | 22 | 4 |
| 06 | 22 | 5 |
| 07 | 32 | 6 |
| 08 | 42 | 8 |

Official input bounds for every function: `[0.000000, 0.999999]`. All 8
functions are **maximisation** problems.

## 2. Standardisation approach and inverse-transform

Output scales vary enormously across the 8 functions, which can badly
condition GP hyperparameter optimisation and make kernel hyperparameters
hard to compare across functions. Following the same approach established
in Week 2, the output `y` is standardised (zero mean, unit variance) before
fitting either GP:

```python
y_mean = outputs.mean()
y_std = outputs.std(ddof=0) or 1.0   # guards against a zero-variance edge case
y_scaled = (outputs - y_mean) / y_std
```

Every posterior value reported or plotted (posterior mean, posterior
standard deviation, predictions used in observed-vs-predicted and residual
diagnostics, 1D slice plots, 2D posterior-mean/std contour plots) is
mapped back to the original output scale before being used, via:

```python
inverse_mean(m) = m * y_std + y_mean
inverse_std(s)  = s * y_std
```

Inputs are **not** rescaled — they are already confined to the official
`[0, 0.999999]` domain by construction.

## 3. Kernel construction (identical for RBF and Matérn)

Both kernel families use the same construction established in Week 2:

```python
ConstantKernel(1.0, (1e-2, 1e2)) * RBF(length_scale=np.ones(d), length_scale_bounds=(1e-2, 1e2))
    + WhiteKernel(noise_level=1e-2, noise_level_bounds=(1e-10, 1e1))

ConstantKernel(1.0, (1e-2, 1e2)) * Matern(length_scale=np.ones(d), length_scale_bounds=(1e-2, 1e2), nu=2.5)
    + WhiteKernel(noise_level=1e-2, noise_level_bounds=(1e-10, 1e1))
```

`GaussianProcessRegressor` fitting details: `alpha=1e-10` (tiny numerical
jitter only — observation noise is modelled by the `WhiteKernel` term, not
`alpha`), `n_restarts_optimizer=5`, `normalize_y=False` (output
standardisation is handled manually, as above), `random_state = 42 +
function_number` (see seed policy below). The `WhiteKernel` term is
retained for all 8 functions as a conservative, consistent modelling
choice, matching the rationale established in Week 2 (evidence of
near-duplicate inputs with non-identical outputs for some functions, which
could not be ruled out as measurement noise).

## 4. Seed policy

`SEED = 42 + FUNCTION_NUMBER`, i.e. 43 for Function_01 through 50 for
Function_08. This seed is passed to every `GaussianProcessRegressor`
(`random_state=SEED`), including the read-only convergence diagnostic
instances described below, and is printed explicitly near the top of every
notebook.

## 5. Convergence-aware model-selection rule

Every notebook instruments the restart search for **both** kernels — this
was applied only to Function_05 as a one-off diagnostic in Week 2's
remediation; in Week 3 it is applied uniformly to **all 8** functions. The
instrumentation replays the exact same kernel construction,
`random_state=SEED`, and `n_restarts_optimizer=5` search used by the
notebook's own `build_and_fit()`, but records, for each of the 6 individual
L-BFGS-B optimizer calls per kernel (1 default initial-`theta` run + 5
random restarts), whether that specific call terminated `ABNORMAL`ly and
what log-marginal-likelihood (LML) it found. This is read-only: it uses a
separate, throwaway `GaussianProcessRegressor` instance and does not alter
the reported `gpr_rbf`/`gpr_matern` models, hyperparameters, or LML values
reported elsewhere in the notebook. No `ConvergenceWarning` is suppressed
anywhere — a wrapper forwards every warning to the default handler in
addition to recording it for structured reporting, so warnings remain
visible in the executed cell output.

The exact rule applied (quoted verbatim from
`Historical_Replay/Week_02/REMEDIATION_REPORT.md`, where it was first
documented and applied to Function_05):

> "Prefer the kernel with fewer/no `ABNORMAL` optimizer terminations across
> restarts. If both kernels converge cleanly, prefer the higher
> log-marginal-likelihood. If neither kernel converges reliably across
> restarts, do not select either as 'the model' for this function — report
> both as exploratory only."

This rule is applied **programmatically** in each notebook (not
hand-picked) against the per-restart `ABNORMAL` counts computed by the
diagnostic described above.

### Outcome per function (Week 3 cumulative datasets)

| Function | RBF ABNORMAL | Matérn ABNORMAL | Selected |
|---|---|---|---|
| 01 | 0/6 | 0/6 | RBF (higher LML; both converged cleanly) |
| 02 | 0/6 | 0/6 | RBF (higher LML; both converged cleanly) |
| 03 | 0/6 | 0/6 | RBF (higher LML; both converged cleanly) |
| 04 | 0/6 | 0/6 | Matérn (higher LML; both converged cleanly) |
| 05 | 3/6 | 5/6 | RBF (fewer ABNORMAL terminations; neither converges cleanly) |
| 06 | 0/6 | 0/6 | Matérn (higher LML; both converged cleanly) |
| 07 | 0/6 | 0/6 | RBF (higher LML; both converged cleanly) |
| 08 | 0/6 | 0/6 | Matérn (higher LML; both converged cleanly) |

Function_05 is the sole exception in this Week 3 batch: neither kernel
converges cleanly across all 6 restarts (mirroring the same kind of
convergence instability first documented for this function in Week 2's
remediation, likely related to its small sample size relative to the
number of GP hyperparameters being fit). Per the rule, RBF — the kernel
with fewer `ABNORMAL` terminations — is still treated as the (marginally)
more convergence-reliable of the two exploratory candidates for
Function_05, even though it is not the kernel with the higher raw
log-marginal-likelihood there. No function in this Week 3 batch produced
the "neither reliable" outcome (equal, non-zero `ABNORMAL` counts on both
kernels).

## 6. Diagnostic limitations

- **In-sample-only observed-vs-predicted and residual diagnostics.** Given
  the very small dataset sizes in this project (12–42 observations
  depending on the function), a leave-one-out or other held-out evaluation
  was judged likely to be unstably estimated and was not used. The
  observed-vs-predicted and residual diagnostics in every notebook are
  computed **in-sample**: the GP predicts on the same points it was fitted
  on. This is stated explicitly in each notebook as a real limitation, not
  concealed — in-sample fit quality for a GP with a `WhiteKernel` noise
  term does not establish generalisation to unseen points.
- **No held-out validation was performed anywhere in this track.**
- **Sample sizes remain small** relative to the number of GP
  hyperparameters being fit (especially for higher-dimensional functions),
  so both the fitted hyperparameters and the LML-based kernel comparison
  should be read as exploratory, not as a statistically validated
  conclusion.
- **The convergence-aware selection rule is not a substitute for a
  statistically validated model comparison.** It only checks whether the
  optimizer search behaved reliably and, secondarily, compares raw
  log-marginal-likelihood; it does not test predictive accuracy on unseen
  data.

## 7. Scope boundary

No acquisition function was calculated, and no new query was selected or
saved, anywhere in this track. No `03_Queries` folder was created under
`Historical_Replay/Week_03`. Each notebook states this explicitly in a
final markdown cell. The next step — an acquisition-function-driven query
proposal over the GP posteriors established here — remains explicitly
deferred to a future, separate step.
