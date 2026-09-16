# Independent Evaluation — Week_01 Random-Search Baseline (Functions 01–08)

**Scope:** Independent, read-only re-verification of the Week_01 random-search
baseline (notebooks, query files, and empty `04_Results` folders) conducted by
the Evaluation Agent. No notebook, query file, or dataset was modified,
regenerated, or overwritten. Each query was independently re-derived from
scratch in a separate Python process — the agent did not trust the figures it
was given, and recomputed everything itself.

**Method:** Each notebook was executed via `jupyter nbconvert --to notebook
--execute` against a scratch copy outside the project (with the correct
relative data path preserved), and the original `.ipynb` files were verified
byte-identical before and after via `diff`. Each query was independently
reproduced using `np.random.default_rng(42 + function_number)`, the same
draw/round/redraw procedure specified in the original implementation, and
compared coordinate-by-coordinate against the actual bytes on disk in each
`03_Queries/week_01_query.txt`.

---

## Per-Function Results

### Function_01 (seed 43, expected d=2, expected shape (10,2))

| # | Check | Result |
|---|---|---|
| 1 | Notebook executes without errors | **PASS** |
| 2 | Uses only this function's own `initial_inputs.npy`/`initial_outputs.npy` | **PASS** |
| 3 | Independently reproduced query matches file | **PASS** — reproduced `0.652299-0.043775`; file matches exactly |
| 4 | Correct dimensionality (d=2) | **PASS** |
| 5 | All coordinates in [0.000000, 0.999999] | **PASS** |
| 6 | Exactly six decimal places per coordinate | **PASS** |
| 7 | Hyphen-separated format, no brackets/commas | **PASS** — raw bytes `0.652299-0.043775\n` |
| 8 | No duplicate of existing input row | **PASS** |
| 9 | No fabricated output; `04_Results/` empty | **PASS** |
| 10 | No future-week/source-repo leakage | **PASS** |

**Draws needed:** 1. **OVERALL: PASS (10/10)**

### Function_02 (seed 44, expected d=2, expected shape (10,2))

| # | Check | Result |
|---|---|---|
| 1 | Notebook executes without errors | **PASS** |
| 2 | Uses only own data | **PASS** |
| 3 | Reproduction matches file | **PASS** — `0.122566-0.258113` |
| 4 | Dimensionality (d=2) | **PASS** |
| 5 | Bounds | **PASS** |
| 6 | Six decimal places | **PASS** |
| 7 | File format | **PASS** — raw bytes `0.122566-0.258113\n` |
| 8 | No duplicate | **PASS** |
| 9 | No fabricated output; `04_Results/` empty | **PASS** |
| 10 | No leakage | **PASS** |

**Draws needed:** 1. **OVERALL: PASS (10/10)**

### Function_03 (seed 45, expected d=3, expected shape (15,3))

| # | Check | Result |
|---|---|---|
| 1 | Notebook executes without errors | **PASS** |
| 2 | Uses only own data | **PASS** |
| 3 | Reproduction matches file | **PASS** — `0.573131-0.528491-0.763650` |
| 4 | Dimensionality (d=3) | **PASS** |
| 5 | Bounds | **PASS** |
| 6 | Six decimal places | **PASS** |
| 7 | File format | **PASS** — raw bytes `0.573131-0.528491-0.763650\n` |
| 8 | No duplicate | **PASS** |
| 9 | No fabricated output; `04_Results/` empty | **PASS** |
| 10 | No leakage | **PASS** |

**Draws needed:** 1. **OVERALL: PASS (10/10)**

### Function_04 (seed 46, expected d=4, expected shape (30,4))

| # | Check | Result |
|---|---|---|
| 1 | Notebook executes without errors | **PASS** |
| 2 | Uses only own data | **PASS** |
| 3 | Reproduction matches file | **PASS** — `0.905604-0.077227-0.272570-0.621850` |
| 4 | Dimensionality (d=4) | **PASS** |
| 5 | Bounds | **PASS** |
| 6 | Six decimal places | **PASS** |
| 7 | File format | **PASS** — raw bytes `0.905604-0.077227-0.272570-0.621850\n` |
| 8 | No duplicate | **PASS** |
| 9 | No fabricated output; `04_Results/` empty | **PASS** |
| 10 | No leakage | **PASS** |

**Draws needed:** 1. **OVERALL: PASS (10/10)**

### Function_05 (seed 47, expected d=4, expected shape (20,4))

| # | Check | Result |
|---|---|---|
| 1 | Notebook executes without errors | **PASS** |
| 2 | Uses only own data | **PASS** |
| 3 | Reproduction matches file | **PASS** — `0.741802-0.753669-0.465181-0.103725` |
| 4 | Dimensionality (d=4) | **PASS** |
| 5 | Bounds | **PASS** |
| 6 | Six decimal places | **PASS** |
| 7 | File format | **PASS** — raw bytes `0.741802-0.753669-0.465181-0.103725\n` |
| 8 | No duplicate | **PASS** |
| 9 | No fabricated output; `04_Results/` empty | **PASS** |
| 10 | No leakage | **PASS** |

**Draws needed:** 1. **OVERALL: PASS (10/10)**

### Function_06 (seed 48, expected d=5, expected shape (20,5))

| # | Check | Result |
|---|---|---|
| 1 | Notebook executes without errors | **PASS** |
| 2 | Uses only own data | **PASS** |
| 3 | Reproduction matches file | **PASS** — `0.387700-0.595773-0.513801-0.694492-0.652497` |
| 4 | Dimensionality (d=5) | **PASS** |
| 5 | Bounds | **PASS** |
| 6 | Six decimal places | **PASS** — note: the notebook's interactive terminal display printed the raw float as `0.3877` (Python's default float repr drops trailing zeros), but the actual six-decimal-place formatting was verified from the raw bytes of the `.txt` file itself, which correctly reads `0.387700` |
| 7 | File format | **PASS** — raw bytes `0.387700-0.595773-0.513801-0.694492-0.652497\n` |
| 8 | No duplicate | **PASS** |
| 9 | No fabricated output; `04_Results/` empty | **PASS** |
| 10 | No leakage | **PASS** |

**Draws needed:** 1. **OVERALL: PASS (10/10)**

### Function_07 (seed 49, expected d=6, expected shape (30,6))

| # | Check | Result |
|---|---|---|
| 1 | Notebook executes without errors | **PASS** |
| 2 | Uses only own data | **PASS** |
| 3 | Reproduction matches file | **PASS** — `0.362854-0.593217-0.391950-0.623699-0.655815-0.013586` |
| 4 | Dimensionality (d=6) | **PASS** |
| 5 | Bounds | **PASS** |
| 6 | Six decimal places | **PASS** |
| 7 | File format | **PASS** — raw bytes `0.362854-0.593217-0.391950-0.623699-0.655815-0.013586\n` |
| 8 | No duplicate | **PASS** |
| 9 | No fabricated output; `04_Results/` empty | **PASS** |
| 10 | No leakage | **PASS** |

**Draws needed:** 1. **OVERALL: PASS (10/10)**

### Function_08 (seed 50, expected d=8, expected shape (40,8))

| # | Check | Result |
|---|---|---|
| 1 | Notebook executes without errors | **PASS** |
| 2 | Uses only own data | **PASS** |
| 3 | Reproduction matches file | **PASS** — `0.787423-0.833669-0.547904-0.973449-0.236834-0.646923-0.065026-0.555578` |
| 4 | Dimensionality (d=8) | **PASS** |
| 5 | Bounds | **PASS** |
| 6 | Six decimal places | **PASS** |
| 7 | File format | **PASS** — raw bytes `0.787423-0.833669-0.547904-0.973449-0.236834-0.646923-0.065026-0.555578\n` |
| 8 | No duplicate | **PASS** |
| 9 | No fabricated output; `04_Results/` empty | **PASS** |
| 10 | No leakage | **PASS** |

**Draws needed:** 1. **OVERALL: PASS (10/10)**

---

## Cross-Function Summary

**8/8 functions passed all 10 checks — 80/80 individual checks PASS, 0 FAIL.**

- All 8 data shapes matched the expected values exactly: (10,2), (10,2), (15,3),
  (30,4), (20,4), (20,5), (30,6), (40,8).
- All 8 notebooks used the correct seed (`42 + function_number` = 43–50) and
  correct dimensionality; in every case the first draw from the generator was
  already valid — 0 redraws were needed for any function. This matches both the
  notebooks' own executed output ("Number of draws needed: 1") and the
  evaluator's independent reproduction script.
- All 8 independently-reproduced queries matched the actual bytes on disk in
  `03_Queries/week_01_query.txt` exactly, coordinate-for-coordinate.
- All 8 `04_Results/` directories are confirmed completely empty — no
  fabricated output values anywhere.
- No references to `Week_02` or later, `04_Results`, or
  `~/Documents/GitHub/My_Capstone_1_Imperial` were found in any notebook's
  code cells; the only textual hits were markdown prose explicitly disclaiming
  such references, not actual code paths.
- All 8 query files are single-line, hyphen-separated, six-decimal-place,
  and free of brackets, commas, or extraneous whitespace.
- All 8 original `.ipynb` files were confirmed byte-identical before and after
  the evaluation's own execution pass (verified via `diff` against scratch
  copies) — the evaluation did not alter them.

## Scope Notes (What Was Not Checked)

- The evaluator did not exhaustively check every markdown/prose cell of each
  notebook or the narrative accuracy of `WEEK_01_METHOD.md` — only code-level
  data provenance and leakage were in scope, and those were fully checked.
- Notebook execution was run once per function on the evaluator's available
  Python/Jupyter environment (Python 3.14.3, numpy 2.5.2); given the fixed
  seeds, results are deterministic and expected to reproduce identically in
  the original environment.

## Conclusion

No corrections are required. This is a clean, independently-verified pass
across all 8 functions and all 10 required checks. The Week_01 random-search
baseline (notebooks, queries, and untouched results/datasets) is confirmed
correct, reproducible, and free of data leakage.

---

## Post-Remediation Regression Check (2026-09-16)

**Scope:** Following the Code Debugger Agent's remediation of the Code
Reviewer's recurring Major finding (missing file-write code) and two Minor
findings (unused `pandas` import; dtype checked by print only) across all 8
notebooks, the Evaluation Agent conducted an independent, read-only
regression check to confirm the fixes are genuine, nothing regressed, and no
other rule was violated. All checks below were independently computed or
directly read by the evaluator — none were taken on trust from the
remediation's own addenda. No notebook, query, dataset, or report (other
than this section) was modified during this check.

### Summary — 8/8 Functions PASS on All 8 Required Checks

| Function | 1. Executes clean | 2. Writes own query file | 2b. pandas removed / dtype assert | 3. Query matches approved table | 4. Independent reproduction matches | 5. Datasets unchanged | 6. 04_Results empty | 7. No forbidden refs | 8. Major/Minor findings resolved | Overall |
|---|---|---|---|---|---|---|---|---|---|---|
| Function_01 | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_02 | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_03 | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_04 | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_05 | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_06 | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_07 | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_08 | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |

### Key Evidence

1. **Clean execution.** All 8 notebooks were re-executed end-to-end against a
   scratch copy (never `--inplace` on the originals). `nbconvert` exit status
   0 for all 8; zero error-type cells found in any executed notebook.
2. **Independent file-write confirmed genuine.** All 8 notebooks contain an
   actual executable code cell (not just markdown) performing
   `open("../03_Queries/week_01_query.txt", "w").write(query_string + "\n")`,
   immediately after the `query_string` cell. The executed cell output
   prints `Query written to ../03_Queries/week_01_query.txt` in all 8,
   confirming it actually ran. Re-running each notebook from scratch in the
   scratch copy regenerated a `03_Queries/week_01_query.txt` that was
   `diff`-identical to the original project's file in every case.
   **[Minor fixes]** Zero `import pandas` matches in any of the 8 notebooks;
   all 8 contain genuine `assert inputs.dtype == np.float64` /
   `assert outputs.dtype == np.float64` statements (not print-only).
3. **Query files unchanged from the approved table.** Raw bytes of all 8
   `week_01_query.txt` files were read and matched exactly against the
   previously-approved queries, each with a single trailing `\n`, six
   decimal places, hyphen-separated, no brackets/commas/spaces.
4. **Independent from-scratch reproduction matches.** A standalone
   verification script (not the notebooks) reproduced all 8 queries from the
   raw seed (`42 + function_number`) and the real `initial_inputs.npy`
   files, applying the same redraw-on-duplicate/out-of-bounds rule. All 8
   matched the approved table and the on-disk files exactly, byte-for-byte;
   all needed exactly 1 draw; all coordinates confirmed within
   `[0.000000, 0.999999]`; all dimensionalities/shapes confirmed correct
   ((10,2), (10,2), (15,3), (30,4), (20,4), (20,5), (30,6), (40,8)).
5. **All 16 datasets unchanged.** SHA-256 of all 16 `.npy` files recomputed
   and matched exactly against the pre-remediation reference checksums.
6. **All `04_Results` folders remain empty** — confirmed for all 8.
7. **No forbidden references.** Grepped all 8 notebooks' raw JSON for
   `Week_02`+, `My_Capstone_1_Imperial`, and `04_Results` as an actual code
   path: none found (only benign markdown disclaimers). The only paths
   referenced by any code cell in any notebook are
   `../02_Data/initial_inputs.npy`, `../02_Data/initial_outputs.npy`, and
   `../03_Queries/week_01_query.txt`.
8. **Code Reviewer's Major/Minor findings genuinely resolved.** Cross-checked
   each function's `06_Code_Review/code_review_report.md` remediation
   addendum against the actual notebook source (not merely the addendum's
   own claims): the file-write cell, pandas-import removal, and dtype
   asserts are genuinely present in all 8; the explanatory comment on the
   previously-unreachable `< 0.0` branch is present verbatim in all 8, with
   the reachable `>= 1.0` check and the guard clause both retained (bounds
   guarantee preserved, not weakened). Function_01's addendum-cited SHA-256
   for its query file was independently recomputed and matched exactly.

### Assumptions/Inference Note

The evaluator did not have a pre-remediation snapshot of the notebooks to
diff directly, so the "before remediation" state is taken from each
`code_review_report.md`'s original (pre-addendum) section rather than
independently re-derived. This does not affect any verdict above, since
every PASS is based on directly inspected current-state evidence (source
code, executed outputs, checksums, and independent RNG reproduction), not on
trusting the addenda's own narrative.

### Final Verdict for Week_01

# **PASS**

All 8 functions independently verified PASS on all 8 required
post-remediation checks. No Critical, Major, or Minor issues remain open.
No dataset was altered, no notebook writes outside its authorized
`02_Data`/`03_Queries` paths, no `04_Results` folder was populated, no
future-week or original-source-project data is referenced, and every
notebook's query output is independently reproducible byte-for-byte from the
raw seed and data with zero discrepancy against the previously-approved
table.
