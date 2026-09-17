# Week 4 Method — Historical Replay: GP Diagnostics

**Track:** Historical Replay (retrospective learning). This document
describes the method used to build the 8
`Historical_Replay/Week_04/Function_0X/01_Notebook/week_04_gp_diagnostics.ipynb`
notebooks (X = 01..08). **This entire track is a retrospective replay of
decisions using already-completed, independently-verified historical data
— it is not a live or real submission of any kind.** No acquisition
function is run and no new query is proposed, selected, or saved anywhere
in this track.

## 1. Data boundary

The only data used anywhere in these 8 notebooks is, per function X:

- `Historical_Replay/Week_04/Function_0X/02_Data/cumulative_inputs.npy`
- `Historical_Replay/Week_04/Function_0X/02_Data/cumulative_outputs.npy`

These arrays were built (prior to and independently of this notebook track)
by taking each function's Week 3 cumulative dataset and appending exactly
one row: the independently-verified genuine historical Week 3 query-output
pair. That construction and its provenance/verification trail already
exist and are independently PASS-verified in
`Historical_Replay/Week_04/CUMULATIVE_DATA_VALIDATION.md` and
`Historical_Replay/Week_04/INDEPENDENT_CUMULATIVE_REVIEW.md`; each notebook
cites this by reference only and does not re-open those files
programmatically, but every notebook does independently (re-)compute its
own basic sanity checks directly on the loaded arrays (shape, dtype,
dimensionality, finiteness, official-bounds compliance, and duplicate-row
checks after rounding to 6 decimal places) rather than trusting that
provenance chain blindly.

**No data from any later week was used.** In particular, no real Week 4
historical query or output was loaded, revealed, or fabricated anywhere —
none exists yet for this project. No `Week_05` location was created or
referenced. Each notebook contains an explicit "Chronological validity
confirmation" markdown cell stating this.

Row counts and dimensionalities used for each function's sanity checks
(confirmed to match the actual loaded array shapes for all 8 functions):

| Function | n (rows) | d (columns) |
|---|---|---|
| 01 | 13 | 2 |
| 02 | 13 | 2 |
| 03 | 18 | 3 |
| 04 | 33 | 4 |
| 05 | 23 | 4 |
| 06 | 23 | 5 |
| 07 | 33 | 6 |
| 08 | 43 | 8 |

Official input bounds for every function: `[0.000000, 0.999999]`. All 8
functions are **maximisation** problems.

## 2. Standardisation approach and inverse-transform

Output scales vary enormously across the 8 functions, which can badly
condition GP hyperparameter optimisation and make kernel hyperparameters
hard to compare across functions. Following the same approach established
in Week 2 and Week 3, the output `y` is standardised (zero mean, unit
variance) before fitting either GP:

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

Both kernel families use the same construction established in Week 2 and
Week 3:

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
remediation, then applied uniformly to all 8 functions in Week 3, and again
uniformly to all 8 functions here in Week 4. The instrumentation replays
the exact same kernel construction, `random_state=SEED`, and
`n_restarts_optimizer=5` search used by the notebook's own
`build_and_fit()`, but records, for each of the 6 individual L-BFGS-B
optimizer calls per kernel (1 default initial-`theta` run + 5 random
restarts), whether that specific call terminated `ABNORMAL`ly and what
log-marginal-likelihood (LML) it found. This is read-only: it uses a
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
diagnostic described above. Additionally, each notebook now includes a
Section 8d that dynamically prints an explicit convergence-stability
caveat computed directly from that function's own per-restart counts (not
hardcoded), so that any instability observed this week — for any function,
not only Function_05 — is stated plainly regardless of which kernel the
rule ultimately selects.

### Outcome per function (Week 4 cumulative datasets, as executed)

| Function | RBF ABNORMAL | Matérn ABNORMAL | Selected |
|---|---|---|---|
| 01 | 0/6 | 0/6 | RBF (higher LML; both converged cleanly) |
| 02 | 0/6 | 0/6 | RBF (higher LML; both converged cleanly) |
| 03 | 0/6 | 0/6 | RBF (higher LML; both converged cleanly) |
| 04 | 0/6 | 0/6 | Matérn (higher LML; both converged cleanly) |
| 05 | 5/6 | 3/6 | Matérn (fewer ABNORMAL terminations; neither converges cleanly) |
| 06 | 0/6 | 0/6 | Matérn (higher LML; both converged cleanly) |
| 07 | 0/6 | 0/6 | Matérn (higher LML; both converged cleanly) |
| 08 | 0/6 | 0/6 | Matérn (higher LML; both converged cleanly) |

No function in this Week 4 batch produced the "neither reliable" outcome
(equal, non-zero `ABNORMAL` counts on both kernels). Function_05 remains
the sole exception with any `ABNORMAL` terminations this week.

**Function_05 vs. Week 2 and Week 3.** Week 3's diagnostic found RBF: 3/6
ABNORMAL, Matérn: 5/6 ABNORMAL (RBF selected as the marginally more
reliable candidate; neither converged cleanly). This week (Week 4, with one
additional cumulative observation), the counts have **flipped**: RBF: 5/6
ABNORMAL (worse than Week 3's 3/6), Matérn: 3/6 ABNORMAL (better than Week
3's 5/6) — so Matérn is now the marginally more convergence-reliable
candidate instead of RBF. Neither kernel converges cleanly across all 6
restarts this week either, so this is **not** an improvement to full
reliability — it is a mixed picture relative to Week 3 (one kernel got
worse, the other got better), consistent with the same underlying
optimizer instability first documented for this function in Week 2's
remediation report and confirmed again in Week 3. This function's
instability has neither been resolved nor uniformly worsened; it persists
in a different (flipped) form. Each notebook's own Section 8b/8c/8e cells
contain the live, independently re-executed evidence for this comparison
(computed at run time from `rbf_n_abnormal`/`matern_n_abnormal`, not
hardcoded from this table).

## 6. Diagnostic limitations

- **In-sample-only observed-vs-predicted and residual diagnostics.** Given
  the very small dataset sizes in this project (13–43 observations
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
`Historical_Replay/Week_04`. Each notebook states this explicitly in a
final markdown cell. The next step — an acquisition-function-driven query
proposal over the GP posteriors established here — remains explicitly
deferred to a future, separate step.
