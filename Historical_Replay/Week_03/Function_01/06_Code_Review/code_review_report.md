# Code Review Report — Historical_Replay/Week_03/Function_01

**Scope:** Read-only code review of
`01_Notebook/week_03_gp_diagnostics.ipynb`, conducted by the Code Reviewer
Agent. No notebook, dataset, figure, or existing report was modified.
Findings were independently verified — best-observed/descriptive statistics
were recomputed from scratch directly from the raw `.npy` files, checksums
were recomputed independently, and the convergence-aware selection outcome
was cross-checked against the notebook's own raw warning output.

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** PASS — no findings above Informational.

## Findings by Criterion

1. **Data provenance/leakage** — No issue found. Only
   `np.load("../02_Data/cumulative_inputs.npy")` / `cumulative_outputs.npy`.
   No other function, week, or path referenced in any code cell. A benign
   citation to "Function_05" appears only as the origin of the diagnostic
   *methodology* (the convergence-aware rule), not as a data source.
2. **Sanity checks** — No issue found. Real `assert`-backed checks: dtype,
   shape, `n_obs==12`, `d==2`, NaN/Inf==0, duplicates-at-6dp==0, bounds —
   all correct constants for Function_01.
3. **Final-row identification** — No issue found. `historical_idx = n_obs - 1`
   (index 11); markdown explicitly states this. Printed:
   `[0.536429, 0.835362]` → `1.674933466363685e-36`, matching
   `CUMULATIVE_DATA_VALIDATION.md` exactly.
4. **Descriptive statistics / best-observed** — No issue found.
   Independently recomputed argmax/mean/std from raw `.npy` matches the
   notebook's own output exactly (best_idx=2, best_out=`7.710875114502849e-16`).
5. **Standardization** — No issue found. `y_scaled` computed once before
   fitting; every prediction is inverse-transformed before use in any plot
   or print. No double-transform found.
6. **Kernel construction** — No issue found. Exactly
   `ConstantKernel * RBF + WhiteKernel` and the Matérn (nu=2.5) analog.
7. **Seed** — No issue found. `SEED = 42 + 1 = 43`, printed and used
   consistently.
8. **Convergence rule** — No issue found. All 6 restarts instrumented per
   kernel. Result: RBF 0/6 ABNORMAL, Matérn 0/6 ABNORMAL → RBF selected
   (higher LML: -16.143 vs -16.431). Matches the claimed outcome table
   exactly.
9. **Warnings visibility** — No issue found. Zero occurrences of
   `filterwarnings`/`simplefilter("ignore")`. Real `ConvergenceWarning`
   stderr output present (length-scale-near-bound warnings, not the
   ABNORMAL kind, consistent with the 0/6 count).
10. N/A — Function_01 is not the unstable case.
11. **In-sample labeling** — No issue found. Markdown explicitly states
    "In-sample limitation, stated explicitly"; axis label reads "predicted
    output (posterior mean ± 1 std, in-sample)." No claim of held-out
    validation anywhere.
12. **CI/slice correctness** — No issue found. Genuine
    `gpr.predict(..., return_std=True)`. Slice code correctly varies
    exactly one dimension while holding the rest at `best_input`.
13. **Figures genuine** — No issue found. All plotting cells end in
    `fig.savefig(...)` immediately following real computed grid/slice data.
14. **Provenance citation** — No issue found. Cites
    `Historical_Replay/Week_02/Function_01/pair_provenance.md` — correct
    2-digit function number; file confirmed to exist.
15. **`requirements-lock.txt`** — No issue found (project-wide, see
    `CODE_REVIEW_SUMMARY.md`).
16. **No Week 03 data/acquisition/future info** — No issue found.
17. **No dataset/earlier-week modification** — No issue found (project-wide,
    see `CODE_REVIEW_SUMMARY.md`).

**Informational:** Section "## 7. Exploratory input-output figures" jumps
straight to "### 7a." — fine for d=2 (Function_01/02 are the only ones with
a 2D scatter section); cosmetic only.

## Overall Verdict

**PASS.** No Critical, Major, or Minor findings.
