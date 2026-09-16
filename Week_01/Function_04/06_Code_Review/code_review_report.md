# Code Review Report — Function_04 (Week_01)

**Scope:** Read-only code review of
`01_Notebook/week_01_random_baseline.ipynb`, conducted by the Code Reviewer
Agent. No notebook, query file, dataset, `CLAUDE.md`, or result folder was
modified. Findings were independently verified where possible.

**Severity labels:** Critical / Major / Minor / Informational.

**Summary:** PASS with minor issues.

## Findings by Criterion

1. **Loading correctness** — No issue found (cell 3).
2. **Input/output validation** — **[Minor]** dtype printed, not asserted
   (cell 3). Cell 5: `assert d_in == 4` correct; matches actual (30,4)
   shape.
3. **Seed reproducibility** — No issue found. Cell 14: `seed = 42 + 4` = 46,
   correct.
4. **Dimensionality and bounds** — No issue found. `d = 4`, correct.
5. **Duplicate-query prevention** — No issue found; independently
   reproduced query `0.905604-0.077227-0.272570-0.621850` matches saved
   file exactly.
6. **Six-decimal / hyphen formatting** — No issue found (cell 16).
7. **Leakage** — No issue found.
8. **Plot correctness/appropriateness** — No issue found. Cell 10: 4
   per-feature subplots, `ncols=4, nrows=1`, all populated correctly, no
   plotting bugs.
9. **Clarity, comments, academic reproducibility** — **[Major]** Same
   missing file-write code as prior notebooks.
10. **Hidden errors / fragile assumptions / unnecessary dependencies** —
    **[Minor]** Unused `pandas` import.
    **[Informational]** Dead `<0.0` branch.
    **[Informational — positive observation]** This function's historical
    inputs are NOT pre-rounded to 6dp (confirmed min=0.006250400244917853,
    i.e. full float64 precision), so the duplicate-check's explicit
    `np.round(inputs, 6)` before comparison (cell 5/14) is important and
    correctly present.

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
- Seed unchanged: `seed = 42 + 4 = 46`.
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
  | `03_Queries/week_01_query.txt` (before) | `a2ba59f21fdd6c1724825b7f23ceb514bbf41716fb7cebd81c1a20573676769d` |
  | `03_Queries/week_01_query.txt` (after)  | `a2ba59f21fdd6c1724825b7f23ceb514bbf41716fb7cebd81c1a20573676769d` |
  | `02_Data/initial_inputs.npy` (before)   | `e431a04b3c4678191bf38f398cd79fdc826aea47a3e7c6b6d72e234ec79339e0` |
  | `02_Data/initial_inputs.npy` (after)    | `e431a04b3c4678191bf38f398cd79fdc826aea47a3e7c6b6d72e234ec79339e0` |
  | `02_Data/initial_outputs.npy` (before)  | `dd594bb7929e5a57d852cbdaf7eb0bb184a3dced78b73c7c03f40ccf9046ec3f` |
  | `02_Data/initial_outputs.npy` (after)   | `dd594bb7929e5a57d852cbdaf7eb0bb184a3dced78b73c7c03f40ccf9046ec3f` |

  All three checksums are identical before and after remediation, confirming
  the new file-write cell reproduces (not changes) the existing query file,
  and that no dataset file was modified.

**Notebook re-execution:** re-run top-to-bottom via
`jupyter nbconvert --to notebook --execute --inplace`; completed with zero
errors, and the new write cell printed
`Query written to ../03_Queries/week_01_query.txt`.
