# Code Review Report — Function_01 (Week_01)

**Scope:** Read-only code review of
`01_Notebook/week_01_random_baseline.ipynb`, conducted by the Code Reviewer
Agent. No notebook, query file, dataset, `CLAUDE.md`, or result folder was
modified. Findings were independently verified where possible (e.g. the
notebook's algorithm was independently re-executed against the real
`.npy` data in a scratch, out-of-project location).

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** PASS with minor issues. Loading, validation, seed (43),
dimensionality (2), bounds enforcement, duplicate handling, and query
formatting are all correct and independently reproducible; the notebook is
missing the actual file-write code it claims to perform.

## Findings by Criterion

1. **Loading correctness** — No issue found. Cell 3:
   `inputs = np.load("../02_Data/initial_inputs.npy")`,
   `outputs = np.load("../02_Data/initial_outputs.npy")`; correct relative
   path, correct variable assignment, no swap.
2. **Input/output validation** — **[Minor]** Cell 3 prints `inputs.dtype` /
   `outputs.dtype` but never asserts a dtype (unlike shape/dimensionality,
   which use `assert` in cell 5). Shape, dimensionality, NaN/Inf,
   duplicate-row (rounded to 6dp), and bounds checks are otherwise correctly
   implemented as actual computations, not just claimed in markdown.
3. **Seed reproducibility** — No issue found. Cell 16: `seed = 42 + 1` = 43,
   matches function number 1.
4. **Dimensionality and bounds** — No issue found. Cell 16: `d = 2`;
   `out_of_bounds = bool(np.any(rounded >= 1.0)) or bool(np.any(rounded < 0.0))`
   correctly rejects coordinates >= 1.0.
5. **Duplicate-query prevention** — No issue found. Same `rng` instance
   reused in the `while True` loop (cell 16), never reseeded; compares
   rounded candidate vs. rounded existing rows via a set. Independently
   re-executed: 1 draw needed, no duplicate/out-of-bounds encountered in this
   run (retry branch itself was not exercised — see Informational below).
6. **Six-decimal / hyphen formatting** — No issue found. Cell 18:
   `query_string = "-".join(f"{v:.6f}" for v in rounded)`. Saved file
   `0.652299-0.043775\n` matches the printed output exactly.
7. **Leakage** — No issue found. Grepped notebook JSON for `Week_02`,
   `My_Capstone_1_Imperial`, `GitHub`, absolute paths: none found. Only
   `04_Results` mentions are in markdown disclaimers ("does not write
   anything to 04_Results"), not actual reads/references.
8. **Plot correctness/appropriateness** — No issue found. Cell 10: 2D
   scatter `inputs[:,0]` vs `inputs[:,1]`, colored by `outputs`, labeled
   axes/title/colorbar — appropriate since d=2. Cell 12: per-feature
   scatter, correct column indexing, matched array lengths, labeled.
9. **Clarity, comments, academic reproducibility** — **[Major]** Section 7
   markdown (cell 19) states "The query string printed in Section 6 is saved
   verbatim ... to `../03_Queries/week_01_query.txt`," but no code cell in
   the notebook performs this write (grepped for `open(`, `.write(`,
   `to_csv`, `savetxt`: zero matches). The file exists on disk and matches
   the printed value exactly, but the save step happened outside the
   notebook, so rerunning the notebook top-to-bottom would NOT regenerate
   the saved artifact. This breaks single-source reproducibility.
10. **Hidden errors / fragile assumptions / unnecessary dependencies** —
    **[Minor]** `import pandas as pd` (cell 1) is imported but never used (0
    occurrences of `pd.`) — unnecessary dependency.
    **[Informational]** The `rounded < 0.0` branch of the out-of-bounds check
    (cell 16) is unreachable dead code since `rng.random()` only draws
    `[0,1)`.
    **[Informational]** The bounds check on historical data (cell 5) uses
    continuous `< 1.0` rather than discrete `<= 0.999999`; defensible since
    this function's stored inputs are already rounded to 6dp, but worth a
    one-line comment distinguishing it from the query's discrete bound check.

## Overall Verdict

**PASS with minor issues.** No Critical findings. One recurring Major finding
(missing file-write code — see `Week_01/CODE_REVIEW_SUMMARY.md` for the
cross-notebook pattern) plus two Minor and two Informational notes.


---

## Remediation Addendum (2026-09-16)

Following explicit user instruction, the four corrections recommended above
were implemented in `01_Notebook/week_01_random_baseline.ipynb` and the
notebook was re-executed end-to-end. Applicability and outcome per finding:

1. **[Major] Missing file-write code** — Applicable and fixed. A new code
   cell was added immediately after the `query_string` cell:
   ```python
   query_path = "../03_Queries/week_01_query.txt"
   with open(query_path, "w") as f:
       f.write(query_string + "\n")
   print(f"Query written to {query_path}")
   ```
   The existing `week_01_query.txt` was inspected byte-for-byte before this
   change and confirmed to already end with a single trailing `\n`; the new
   cell reproduces that exact byte format. The notebook now regenerates the
   saved artifact itself rather than depending on an out-of-band write.

2. **[Minor] Unused `import pandas as pd`** — Applicable and fixed. Removed
   from the imports cell; `numpy`, `matplotlib.pyplot`, and `seaborn` imports
   were left untouched.

3. **[Minor] dtype checked by print only** — Applicable and fixed. Added
   `assert inputs.dtype == np.float64, "inputs dtype is not float64"` and
   `assert outputs.dtype == np.float64, "outputs dtype is not float64"`
   alongside (not replacing) the existing print statements, in the same
   style as the other `assert`-based checks (shape/dimensionality) already
   present in this notebook. Both dtypes were independently confirmed as
   `float64` for this function before the assertions were added.

4. **[Informational] Dead `rounded < 0.0` branch** — Applicable; addressed
   by keeping both conditions and adding a one-line comment above the check
   noting that `rounded < 0.0` is a defensive, currently-unreachable
   belt-and-braces guard given `rng.random()`'s `[0, 1)` range, rather than
   removing the clause. The reachable `>= 1.0` bound is unchanged, and the
   guarantee that every rounded coordinate in the final saved query lies in
   `[0.000000, 0.999999]` is preserved (verified below).

**Verification performed after remediation:**
- Seed unchanged: `seed = 42 + 1 = 43`.
- Query value unchanged: the notebook's final `query_string` output was
  re-diffed against `../03_Queries/week_01_query.txt` after re-execution and
  matches exactly.
- Plots, markdown narrative, and methodological conclusions were not
  altered beyond what these four fixes required.
- `.npy` data files were not modified (read-only throughout).
- Byte-for-byte SHA-256 checksums, before remediation and after the notebook
  was re-executed with the new write cell:

  | File | SHA-256 |
  |---|---|
  | `03_Queries/week_01_query.txt` (before) | `ebb64e736ea65b326ed1fe590e5796639715e49ce805f30dabe3508404748ee1` |
  | `03_Queries/week_01_query.txt` (after)  | `ebb64e736ea65b326ed1fe590e5796639715e49ce805f30dabe3508404748ee1` |
  | `02_Data/initial_inputs.npy` (before)   | `ad0017b5583aababb3d3573bc9139f013fc328aa70eba5748f54ef4f1505ad0a` |
  | `02_Data/initial_inputs.npy` (after)    | `ad0017b5583aababb3d3573bc9139f013fc328aa70eba5748f54ef4f1505ad0a` |
  | `02_Data/initial_outputs.npy` (before)  | `8335e4febb64aec5e7b62a7761c0e610e5f4611551a393d225b03feec84e37db` |
  | `02_Data/initial_outputs.npy` (after)   | `8335e4febb64aec5e7b62a7761c0e610e5f4611551a393d225b03feec84e37db` |

  All three checksums are identical before and after remediation, confirming
  the new file-write cell reproduces (not changes) the existing query file,
  and that no dataset file was modified.

**Notebook re-execution:** re-run top-to-bottom via
`jupyter nbconvert --to notebook --execute --inplace`; completed with zero
errors, and the new write cell printed
`Query written to ../03_Queries/week_01_query.txt`.
