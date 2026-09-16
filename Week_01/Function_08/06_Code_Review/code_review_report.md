# Code Review Report — Function_08 (Week_01)

**Scope:** Read-only code review of
`01_Notebook/week_01_random_baseline.ipynb`, conducted by the Code Reviewer
Agent. No notebook, query file, dataset, `CLAUDE.md`, or result folder was
modified. Findings were independently verified where possible.

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** PASS with minor issues.

## Findings by Criterion

1. **Loading correctness** — No issue found (cell 3).
2. **Input/output validation** — **[Minor]** dtype printed, not asserted.
   Cell 5: `assert d_in == 8` correct, matches actual (40,8) shape (largest
   function, correctly handled).
3. **Seed reproducibility** — No issue found. Cell 14: `seed = 42 + 8` = 50,
   correct (highest seed in the set, matches spec exactly).
4. **Dimensionality and bounds** — No issue found. `d = 8`, correct.
5. **Duplicate-query prevention** — No issue found; independently
   reproduced query
   `0.787423-0.833669-0.547904-0.973449-0.236834-0.646923-0.065026-0.555578`
   matches saved file exactly.
6. **Six-decimal / hyphen formatting** — No issue found (cell 16); all 8
   coordinates correctly hyphen-joined with 6 decimals.
7. **Leakage** — No issue found.
8. **Plot correctness/appropriateness** — No issue found. Cell 10: 8
   per-feature subplots, `ncols=4, nrows=2` (exactly 8 slots, none hidden —
   correct, no off-by-one), correct indexing/labels.
9. **Clarity, comments, academic reproducibility** — **[Major]** Same
   missing file-write code as prior notebooks (cell 17 markdown claims the
   save; no code performs it).
10. **Hidden errors / fragile assumptions / unnecessary dependencies** —
    **[Minor]** Unused `pandas` import.
    **[Informational]** Dead `<0.0` branch. This function has the highest
    dimensionality (8) and largest n (40); no scaling issues observed in
    validation or query-generation logic.

## Overall Verdict

**PASS with minor issues.** No Critical findings. One recurring Major finding
(missing file-write code) plus two Minor and one Informational note.


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
- Seed unchanged: `seed = 42 + 8 = 50`.
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
  | `03_Queries/week_01_query.txt` (before) | `d0672129e6d17c10e5e752052bcec73dd01e8ecd0bef19481a80695bd7f2555e` |
  | `03_Queries/week_01_query.txt` (after)  | `d0672129e6d17c10e5e752052bcec73dd01e8ecd0bef19481a80695bd7f2555e` |
  | `02_Data/initial_inputs.npy` (before)   | `fb5ebbb93c990cbdecc44d648362502233b4b5daec335319aca300f9e9b69206` |
  | `02_Data/initial_inputs.npy` (after)    | `fb5ebbb93c990cbdecc44d648362502233b4b5daec335319aca300f9e9b69206` |
  | `02_Data/initial_outputs.npy` (before)  | `693ebca2bb2b738162f701f2587bad0d3f2ac1b96d47d32fa6357f2b8e257798` |
  | `02_Data/initial_outputs.npy` (after)   | `693ebca2bb2b738162f701f2587bad0d3f2ac1b96d47d32fa6357f2b8e257798` |

  All three checksums are identical before and after remediation, confirming
  the new file-write cell reproduces (not changes) the existing query file,
  and that no dataset file was modified.

**Notebook re-execution:** re-run top-to-bottom via
`jupyter nbconvert --to notebook --execute --inplace`; completed with zero
errors, and the new write cell printed
`Query written to ../03_Queries/week_01_query.txt`.
