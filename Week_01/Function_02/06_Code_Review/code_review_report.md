# Code Review Report — Function_02 (Week_01)

**Scope:** Read-only code review of
`01_Notebook/week_01_random_baseline.ipynb`, conducted by the Code Reviewer
Agent. No notebook, query file, dataset, `CLAUDE.md`, or result folder was
modified. Findings were independently verified where possible.

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** PASS with minor issues (identical pattern to Function_01).

## Findings by Criterion

1. **Loading correctness** — No issue found (cell 3, correct paths/variables,
   no swap).
2. **Input/output validation** — **[Minor]** dtype printed (cell 3) but not
   asserted, same as Function_01. Other checks correctly implemented (cell 5).
3. **Seed reproducibility** — No issue found. Cell 16: `seed = 42 + 2` = 44,
   correct.
4. **Dimensionality and bounds** — No issue found. `d = 2`, bounds check
   correct.
5. **Duplicate-query prevention** — No issue found; same-generator reuse
   verified; independently reproduced query `0.122566-0.258113` matches
   saved file; 1 draw needed (retry path unexercised).
6. **Six-decimal / hyphen formatting** — No issue found. Cell 18 uses
   `.6f` formatting and hyphen join; saved file matches.
7. **Leakage** — No issue found; only benign "does not write to 04_Results"
   disclaimer present.
8. **Plot correctness/appropriateness** — No issue found. Cell 10 2D scatter
   uses correct columns (`inputs[:,0]`, `inputs[:,1]`), titled "Function_02:
   input space colored by output" (correctly parametrized, not copy-pasted
   from Function_01). Per-feature plots correct.
9. **Clarity, comments, academic reproducibility** — **[Major]** Same
   missing file-write code as Function_01 (cell 19 markdown claims a save
   that no code cell performs).
10. **Hidden errors / fragile assumptions / unnecessary dependencies** —
    **[Minor]** Unused `pandas` import (cell 1).
    **[Informational]** Dead `rounded < 0.0` branch.
    **[Informational]** Same bounds-check design note as Function_01.

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
- Seed unchanged: `seed = 42 + 2 = 44`.
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
  | `03_Queries/week_01_query.txt` (before) | `20f1aab14204d5d08d99be5eeb09d4a5a6003026a3031f99c4965fbf4b311ff9` |
  | `03_Queries/week_01_query.txt` (after)  | `20f1aab14204d5d08d99be5eeb09d4a5a6003026a3031f99c4965fbf4b311ff9` |
  | `02_Data/initial_inputs.npy` (before)   | `9824ca17efa0fcc70e58040b3e6d6cea7c2d4b8be76023e4df0dfb1788667505` |
  | `02_Data/initial_inputs.npy` (after)    | `9824ca17efa0fcc70e58040b3e6d6cea7c2d4b8be76023e4df0dfb1788667505` |
  | `02_Data/initial_outputs.npy` (before)  | `b4f71bdfd3a50452a2864cffc00b4548ea5d1fb746982ab79b439cee84568a2d` |
  | `02_Data/initial_outputs.npy` (after)   | `b4f71bdfd3a50452a2864cffc00b4548ea5d1fb746982ab79b439cee84568a2d` |

  All three checksums are identical before and after remediation, confirming
  the new file-write cell reproduces (not changes) the existing query file,
  and that no dataset file was modified.

**Notebook re-execution:** re-run top-to-bottom via
`jupyter nbconvert --to notebook --execute --inplace`; completed with zero
errors, and the new write cell printed
`Query written to ../03_Queries/week_01_query.txt`.
