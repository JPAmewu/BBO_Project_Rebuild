# Remediation Report — Week 2 EDA/GP Surrogate Notebooks

**Scope:** Implements the 3 remediations recommended by
`INDEPENDENT_NOTEBOOK_REVIEW.md` across the 8
`Historical_Replay/Week_02/Function_0X/01_Notebook/week_02_eda_gp_surrogate.ipynb`
notebooks. No other change was made to any notebook, dataset, or Week 1
file. All 8 notebooks were re-executed end-to-end after the edits.

## 1. Provenance citation fix (all 8 notebooks)

**Finding confirmed as described in the review.** In every notebook, the
markdown cell titled "## 4. Summary statistics and best observation"
(index 7 in each notebook, 0-based) cited the historical-row provenance
file using a broken three-digit folder name, e.g.
`Historical_Replay/Week_01/Function_001/pair_provenance.md` for
Function_01, while the notebook's own first cell (index 0) already used
the correct two-digit form for the same file. This was purely a
text/citation bug — never programmatically opened — but incorrect.

**Fix applied:** cell 7's citation in every notebook was changed from
`Function_00X` to the correct `Function_0X` (matching that notebook's own
function number), so both citations in each notebook now agree.

**Verification — all 8 corrected paths confirmed to exist on disk:**

| Function | Cited path (after fix) | Exists |
|---|---|---|
| 01 | `Historical_Replay/Week_01/Function_01/pair_provenance.md` | Yes |
| 02 | `Historical_Replay/Week_01/Function_02/pair_provenance.md` | Yes |
| 03 | `Historical_Replay/Week_01/Function_03/pair_provenance.md` | Yes |
| 04 | `Historical_Replay/Week_01/Function_04/pair_provenance.md` | Yes |
| 05 | `Historical_Replay/Week_01/Function_05/pair_provenance.md` | Yes |
| 06 | `Historical_Replay/Week_01/Function_06/pair_provenance.md` | Yes |
| 07 | `Historical_Replay/Week_01/Function_07/pair_provenance.md` | Yes |
| 08 | `Historical_Replay/Week_01/Function_08/pair_provenance.md` | Yes |

Each notebook was also re-scanned after the fix to confirm zero remaining
occurrences of the broken `Function_00\d` pattern, and exactly 2 correct
occurrences (cell 0 and cell 7) of the two-digit path per notebook.

## 2. Function_05 — RBF/Matérn convergence diagnosis

### 2a. Diagnosis method and findings

The independent review's Major finding quoted 4 `ABNORMAL` L-BFGS-B
termination warnings ("21/22/26/31 iteration(s)") printed by Function_05's
Section 6 fitting cell, and attributed all 4 to the RBF fit specifically.

To characterize this rather than take the review's attribution at face
value, a diagnostic (added as new, clearly-labeled cells in the notebook,
titled "6a-diagnostic") replays the **exact same** kernel construction,
`random_state = 47` (`42 + 5`), and `n_restarts_optimizer = 5` search used
by the notebook's `build_and_fit()` for both kernels, but instruments each
of the 6 individual L-BFGS-B optimizer calls per kernel (1 from the
kernel's default initial `theta`, plus 5 random restarts) to record
per-call convergence status and log-marginal-likelihood (LML). This uses a
separate, throwaway `GaussianProcessRegressor` instance — it does not
modify `gpr_rbf`, `gpr_matern`, `n_restarts_optimizer`, the seed, `alpha`,
or any value reported/plotted elsewhere in the notebook, and it does not
suppress any warning (a wrapper forwards every warning to the default
handler in addition to recording it for structured reporting).

**Actual per-restart breakdown found (re-executed, reproducible):**

RBF (6 optimizer attempts):
| Attempt | LML | Status |
|---|---|---|
| initial-theta | −1.334039 | converged |
| restart 1 | −12.496651 | converged |
| restart 2 | −1.334039 | **ABNORMAL** (21 iterations) |
| restart 3 | −1.334039 | converged |
| restart 4 | −14.261805 | converged |
| restart 5 | −1.334040 | converged |

→ **1 of 6** attempts ABNORMAL. Selected (best-LML) run: initial-theta,
converged cleanly.

Matérn (nu=2.5) (6 optimizer attempts):
| Attempt | LML | Status |
|---|---|---|
| initial-theta | −0.909568 | converged |
| restart 1 | −9.441652 | converged |
| restart 2 | −0.909570 | **ABNORMAL** (22 iterations) |
| restart 3 | −0.909568 | **ABNORMAL** (26 iterations) |
| restart 4 | −13.203305 | **ABNORMAL** (31 iterations) |
| restart 5 | −6.323987 | converged |

→ **3 of 6** attempts ABNORMAL (one of which, restart 4, landed at a
clearly worse local optimum, LML = −13.2 vs the true optimum ≈ −0.91).
Selected (best-LML) run: initial-theta, converged cleanly.

**Important correction to the independent review's attribution:** the 4
`ABNORMAL` warnings visible in the un-instrumented Section 6 fitting cell
belong to **both** kernel fits combined (they print as one interleaved
stderr block because `gpr_rbf` and `gpr_matern` are fit sequentially
within a single cell, with the `print("Fitting complete...")` line
flushing to stdout before the accumulated stderr warnings are shown) — not
solely to RBF as the review stated. The per-restart instrumentation
confirms the 21-iteration warning is RBF's only `ABNORMAL` occurrence, and
the 22/26/31-iteration warnings all belong to Matérn. This was cross-
checked against the two additional "close to bound" `ConvergenceWarning`s
in that same block: the one for `noise_level` (lower bound) matches RBF's
final fitted `noise_level` (≈1e-10); the ones for `length_scale` (upper
bound, 100.0) and `noise_level` (lower bound) both match Matérn's final
fitted values (`length_scale[1] = 100.0`, `noise_level ≈ 1e-10`) — a
second, independent line of evidence for the same attribution.

**Likely root cause (grounded in the above, not asserted beyond it):** this
function has `n=21` observations against 6 GP hyperparameters being fit (4
ARD length-scales for `d=4` + 1 output-scale constant + 1 noise level — a
sample-to-parameter ratio of ≈3.5, similar in kind to the Function_03
"Informational" note in the original review but more acute here). Both
kernels' final selected hyperparameters sit at or touch a search bound
(`noise_level` for both; `length_scale` dim 1 for Matérn also at its upper
bound), consistent with a boundary-hugging, ill-conditioned
log-marginal-likelihood surface for this small, higher-dimensional
dataset — a regime known to be numerically difficult for L-BFGS-B.
Whether a different `random_state` would show a different restart-failure
split was **not** tested, since changing the seed is out of scope/forbidden
by this remediation's hard constraints; this diagnosis is grounded in the
single seed (47) actually used and specified for this function, not
generalized beyond it.

### 2b. Warnings not suppressed

Confirmed by direct inspection of the re-executed notebook: the original
Section 6 fitting cell's output still contains **exactly 4** occurrences of
the string `"ABNORMAL"`, unchanged from before this remediation. The new
diagnostic cell's own re-run of the search also prints its warnings
un-suppressed (in addition to the structured per-restart summary).

### 2c/2d/2e. Selection rule and outcome

The notebook's Interpretation markdown (Section 6a, immediately after the
diagnostic) documents and applies this convergence-aware rule verbatim:

> "Prefer the kernel with fewer/no `ABNORMAL` optimizer terminations across
> restarts. If both kernels converge cleanly, prefer the higher
> log-marginal-likelihood. If neither kernel converges reliably across
> restarts, do not select either as 'the model' for this function — report
> both as exploratory only."

**Applied outcome:** Matérn has the higher raw log-marginal-likelihood
(−0.91 vs −1.33), which is the criterion used by default in all other 7
notebooks. For Function_05, that default is explicitly overridden: because
RBF has fewer `ABNORMAL` terminations across its 6 restart attempts (1)
than Matérn (3), **RBF is selected as the more convergence-reliable
candidate**, per the documented rule — not Matérn, despite Matérn's higher
LML. This reversal, and the reasoning behind it, is stated explicitly and
prominently in both the Section 6a interpretation cell and the Section 8
"Notes, scope, and limitations" cell (a new bullet was added at the top of
that section specifically for this).

### 2f. "Neither reliable" scenario — did not apply, but was checked honestly

This is **not** a "neither fit is reliable" situation: in both kernels, the
actual *selected* result (the one whose hyperparameters/LML are reported in
Section 6a and whose posterior is plotted in Section 7) came from the
initial-theta run, which converged cleanly with zero warnings in both
cases. The instability is real (visible in the restart search and in the
boundary-hugging final hyperparameters) and is documented prominently, but
it does not rise to "neither model can be trusted at all" — the notebook
says so explicitly rather than either hiding the instability or
overstating it into a full "reject both" verdict it doesn't support. The
notebook's own wording (Section 8, new bullet) is:

> "**[Function_05-specific, read this before trusting Sections 6/7 above]
> Optimizer convergence instability.** ... Per this notebook's documented
> convergence-aware selection rule ..., RBF — not Matérn, despite its lower
> raw log-marginal-likelihood — is treated as the (marginally) more
> reliable of the two exploratory fits for this function. Both fits are
> retained above for exploratory visualisation only; **neither should be
> treated as a validated surrogate with the same confidence as the other 7
> functions in this project**, and this should be revisited once more
> observations become available for Function_05."

The other 7 notebooks' generic hedging language (Section 6a interpretation
and Section 8) was left completely untouched, as required — only
Function_05's markdown was edited.

## 3. Reproducibility lock file

`Historical_Replay/Week_02/requirements-lock.txt` was created, listing
versions queried directly from the environment used to execute all 8
notebooks (`python3 --version`, `pip3 show <package>`):

```
python==3.14.3
numpy==2.5.2
scipy==1.18.1
scikit-learn==1.9.0
matplotlib==3.11.2
seaborn==0.13.2
jupyter==1.1.1
jupyter_core==5.9.1
nbformat==5.11.1
nbclient==0.11.0
nbconvert==7.17.1
ipykernel==7.3.0
```

`nbconvert` and `ipykernel` were included in addition to the explicitly
requested list because both are actually installed and are the tools that
performed the `--execute --inplace` re-run of all 8 notebooks; no version
was fabricated or guessed for any package not actually present.

## 4. Dataset integrity — before/after SHA-256 (all 16 files, byte-identical)

| File | SHA-256 |
|---|---|
| Function_01/cumulative_inputs.npy | `b78536d083aa66e42f21c99cf71dd5e9ed1a71b0f2d6a039095bbf2674e013c9` |
| Function_01/cumulative_outputs.npy | `d31f1155689179426de763a750f701283b6d4d09216b7840b6c6b70f947916b3` |
| Function_02/cumulative_inputs.npy | `2e953eaaa0946216b8eb079b4affafbd2eded497976917443915127fb742daf7` |
| Function_02/cumulative_outputs.npy | `756099c0a6250ff407ef0c6ea69a78a42ab7aad545bb15875a6c2e3f53b20a87` |
| Function_03/cumulative_inputs.npy | `3d2f4d29ffe34ec15629fdfe8c579506d16a0c6377506abf2d7f3aa5c7235e1d` |
| Function_03/cumulative_outputs.npy | `db06a53358bb2c76e7fb2cbf6200138abfee13b095886ab64cf1110e4cbcfb81` |
| Function_04/cumulative_inputs.npy | `396b567c1b58c21c8f926a71615a24ebb9d1597604cba8de618f14092293b1b6` |
| Function_04/cumulative_outputs.npy | `ac3d0aff8947f5a71af10481defb1a66c83b3aa2de6d411f55d28a4cd2d00a03` |
| Function_05/cumulative_inputs.npy | `c0664ced172d7d8642ccbc902308177d2b04dbf847912d6d20fd116b340ecbb9` |
| Function_05/cumulative_outputs.npy | `7751a3bb3d95d75d67aa87e58c244b26059c58bf93fb27cc3f74d81804bdc110` |
| Function_06/cumulative_inputs.npy | `a02bebc15427f119e5beccef317ea78bde0e71243dc1435ef29f2caa08ac0753` |
| Function_06/cumulative_outputs.npy | `beca42746dcbd38b7151442654519872fb27f57fe51cc35aa29c818ded48b2e0` |
| Function_07/cumulative_inputs.npy | `462dafc9baeb7551b145ada8fd988d556e2c0311237b07ffd6b6b0a1e7e96e85` |
| Function_07/cumulative_outputs.npy | `f7e12fe5d14292a998353cdfc3c22831113d1efeed139fab1ac1684ee6712765` |
| Function_08/cumulative_inputs.npy | `3c3e6adddc6ed389b1b1b2e8b67771b5424d402f249288ac8c85707cb5f7f479` |
| Function_08/cumulative_outputs.npy | `72867d027b9bb995bb93ad1235563552e00ae0643b93552fe801d5a91ae23cb1` |

Computed both before any edit and after all 8 notebooks were re-executed;
identical in every case. No dataset file was modified.

## 5. Re-execution and constraint verification (all 8 notebooks)

- All 8 notebooks were re-executed via
  `jupyter nbconvert --to notebook --execute --inplace`. **Zero execution
  errors** in any of the 8 (`output_type == "error"` count = 0 across all
  code cells in all 8 notebooks). `ConvergenceWarning`s remain present and
  visible where they occurred before (4 `ABNORMAL` occurrences unchanged
  in Function_05's Section 6 cell; other functions' pre-existing
  boundary-value `ConvergenceWarning`s unaffected).
- **Seeds:** confirmed unchanged in all 8 —
  `FUNCTION_NUMBER` = 1..8, `SEED = 42 + FUNCTION_NUMBER` (43–50)
  literally present and un-edited in every notebook; Function_05's new
  diagnostic cells reuse the same `SEED` variable, they do not introduce a
  different seed.
- **Data source:** confirmed unchanged — every `np.load(...)` call in all
  8 notebooks still loads only `../02_Data/cumulative_inputs.npy` /
  `../02_Data/cumulative_outputs.npy`.
- **Standardization:** the `y_mean`/`y_std`/`inverse_mean`/`inverse_std`
  logic was not touched in any of the 8 notebooks, including Function_05
  (the new diagnostic cells reuse the existing `y_scaled` computed by the
  original standardization code; they do not re-derive or alter it).
- **No new query:** confirmed no `03_Queries` folder, acquisition-function
  code, or query file was created anywhere under `Historical_Replay/Week_02`
  (directory search for `03_Queries`/`*query*` returned nothing new).
- **No Week_01 or source-project file touched:** confirmed via file
  modification-time checks — no file under `Historical_Replay/Week_01/`
  or `~/Documents/GitHub/My_Capstone_1_Imperial` is newer than the
  pre-existing `INDEPENDENT_NOTEBOOK_REVIEW.md`, i.e. none was modified
  during this remediation.

## 6. Anomalies encountered

- One anomaly worth flagging honestly: the independent review's Major
  finding for Function_05 mischaracterized which kernel's fit produced the
  4 `ABNORMAL` warnings (it attributed all 4 to RBF; the true split, per
  direct instrumentation, is 1 RBF / 3 Matérn). This remediation corrects
  that attribution in the notebook itself (Section 6a interpretation),
  with the evidence and reasoning shown inline, rather than silently
  reproducing the review's original (incorrect) claim. This does not
  change the review's overall verdict that Function_05 has a real
  convergence issue worth documenting — it only corrects which kernel it
  primarily affects, and, as a result, which kernel the notebook's
  convergence-aware rule actually selects (RBF, not neither and not
  Matérn).
- No other anomalies. No dataset modification, no unexpected execution
  errors, no missing provenance files, no other unauthorized file writes.
