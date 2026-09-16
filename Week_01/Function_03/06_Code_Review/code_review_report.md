# Code Review Report — Function_03 (Week_01)

**Scope:** Read-only code review of
`01_Notebook/week_01_random_baseline.ipynb`, conducted by the Code Reviewer
Agent. No notebook, query file, dataset, `CLAUDE.md`, or result folder was
modified. Findings were independently verified where possible.

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** PASS with minor issues. First of the higher-dimensional
notebooks (18 cells; no 2D scatter, per-feature plots only — correctly
omitted since d=3).

## Findings by Criterion

1. **Loading correctness** — No issue found (cell 3).
2. **Input/output validation** — **[Minor]** dtype printed (cell 3), not
   asserted. Cell 5: `assert d_in == 3` correct for Function_03 (expected
   shape (15,3) confirmed against actual data).
3. **Seed reproducibility** — No issue found. Cell 14: `seed = 42 + 3` = 45,
   correct.
4. **Dimensionality and bounds** — No issue found. `d = 3` in cell 14,
   matches actual data dimensionality; bounds check correct.
5. **Duplicate-query prevention** — No issue found; independently
   reproduced query `0.573131-0.528491-0.763650` matches saved file exactly.
6. **Six-decimal / hyphen formatting** — No issue found (cell 16).
7. **Leakage** — No issue found.
8. **Plot correctness/appropriateness** — No issue found. Cell 10 uses
   per-feature subplots only (no attempt at a 3D/2D scatter forcing 3 dims
   onto 2 axes) — appropriate design given d=3. `ncols=3`, `nrows=1`, all 3
   subplots populated, none hidden unnecessarily.
9. **Clarity, comments, academic reproducibility** — **[Major]** Same
   missing file-write code as Function_01/02 (cell 17 markdown notes claim
   the save; no corresponding code cell exists).
10. **Hidden errors / fragile assumptions / unnecessary dependencies** —
    **[Minor]** Unused `pandas` import.
    **[Informational]** Dead `< 0.0` branch.
    **[Informational]** This function's data is NOT pre-rounded to 6dp
    (confirmed continuous float64), making the `<1.0` bounds check here the
    only sensible choice.

## Overall Verdict

**PASS with minor issues.** No Critical findings. One recurring Major finding
(missing file-write code) plus two Minor and two Informational notes.


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
- Seed unchanged: `seed = 42 + 3 = 45`.
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
  | `03_Queries/week_01_query.txt` (before) | `06f355e57b701e5463e6ee8ec0d0590bc2342f72ffbfbebfb1494a1f7fb11c70` |
  | `03_Queries/week_01_query.txt` (after)  | `06f355e57b701e5463e6ee8ec0d0590bc2342f72ffbfbebfb1494a1f7fb11c70` |
  | `02_Data/initial_inputs.npy` (before)   | `e54dc0e1bd94e301d0756faaf94bff74f60497f67d7fbdf7eb167fa10fe0ed71` |
  | `02_Data/initial_inputs.npy` (after)    | `e54dc0e1bd94e301d0756faaf94bff74f60497f67d7fbdf7eb167fa10fe0ed71` |
  | `02_Data/initial_outputs.npy` (before)  | `f7daf48bc753f8314f09c9a2abfad391382010ab789ca8145bc3e028871dc013` |
  | `02_Data/initial_outputs.npy` (after)   | `f7daf48bc753f8314f09c9a2abfad391382010ab789ca8145bc3e028871dc013` |

  All three checksums are identical before and after remediation, confirming
  the new file-write cell reproduces (not changes) the existing query file,
  and that no dataset file was modified.

**Notebook re-execution:** re-run top-to-bottom via
`jupyter nbconvert --to notebook --execute --inplace`; completed with zero
errors, and the new write cell printed
`Query written to ../03_Queries/week_01_query.txt`.
