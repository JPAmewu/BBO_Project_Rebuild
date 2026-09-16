# Week_01 Code Review Summary — Functions 01–08

**Scope:** Consolidated, read-only code review of all 8
`week_01_random_baseline.ipynb` notebooks, conducted by the Code Reviewer
Agent. No notebook, query file, dataset, `CLAUDE.md`, or result folder was
modified. Findings were independently verified: the Code Reviewer Agent
re-executed each notebook's seeding/rounding/duplicate-check algorithm from
scratch, in a separate read-only Python check, directly against the real
`.npy` data, and confirmed every one of the 8 recomputed query strings
matched the corresponding saved `week_01_query.txt` exactly.

Full per-function detail is in each function's
`06_Code_Review/code_review_report.md`. This file summarizes findings across
notebooks.

**Severity labels:** Critical / Major / Minor / Informational.

## Method Note

All 8 notebooks share one identical template (only the seed, expected
dimensionality `d`, and a couple of markdown numbers/plot cells differ).
Function_01 and Function_02 have 20 cells (0–19, include an extra 2D-scatter
cell since d=2); Function_03–Function_08 have 18 cells (0–17, per-feature
plots only).

## Cross-Function Totals

| Severity | Count | Notes |
|---|---|---|
| Critical | 0 | No rule violations, no wrong seeds, no out-of-bounds or duplicate queries saved, no leakage found in any notebook. |
| Major | 8 | One per notebook — identical recurring issue (see below). |
| Minor | 16 | Two per notebook — identical recurring issues (see below). |
| Informational | ~24 | Two to three per notebook — mostly identical recurring notes, some function-specific. |

## Recurring Findings (Identical Pattern Across All 8 Notebooks)

### 1. [Major] Missing file-write code
Every notebook's final markdown cell states the query string "is saved
verbatim ... to `../03_Queries/week_01_query.txt`," but grepping every
notebook's JSON for `open(`, `.write(`, `to_csv`, `np.savetxt` returns zero
matches in all 8. The Code Reviewer Agent independently verified that each
`03_Queries/week_01_query.txt` file's content exactly matches (byte-for-byte,
modulo trailing newline) the `query_string` printed by the notebook's own
final code cell's output — so **no transcription error actually occurred**
— but the save step itself is not part of the reproducible/rerunnable code
path.

**This is a process/reproducibility gap, not a correctness-of-value bug.**
Recommended fix (not applied — this review made no changes): add an explicit
final code cell to each notebook:
```python
with open("../03_Queries/week_01_query.txt", "w") as f:
    f.write(query_string)
```

### 2. [Minor] Unused `import pandas as pd`
Present in cell 1 of every notebook — 0 occurrences of `pd.` anywhere in any
notebook. Either remove the import or put pandas to actual use (e.g. a
summary-statistics DataFrame).

### 3. [Minor] dtype checked by print only, not by assert
Present in cell 3 of every notebook — inconsistent with the hard `assert`
used for shape/dimensionality checks elsewhere in the same notebook.

### 4. [Informational] Dead `rounded < 0.0` branch
The out-of-bounds check in every notebook includes a `rounded < 0.0` branch
that is unreachable dead code, since `rng.random()` only ever draws values
in `[0, 1)`.

### 5. [Informational] Redraw-loop retry branch never exercised
All 8 functions reached a valid, unique, in-bounds query on the first draw
in this run, so the retry/redraw branch of the duplicate-and-bounds-checking
loop exists and is logically correct, but was not actually exercised by any
of the 8 runs.

## Function-Specific Informational Notes

- **Function_01, Function_02:** Bounds check on historical data uses
  continuous `< 1.0` rather than discrete `<= 0.999999`; defensible since
  these functions' stored inputs are already rounded to 6dp — worth a
  one-line comment distinguishing this from the query's discrete bound
  check.
- **Function_03, Function_04, Function_06:** These functions' historical
  inputs are NOT pre-rounded to 6dp (confirmed full float64 precision), so
  the explicit `np.round(inputs, 6)` before duplicate comparison is
  important and was confirmed correctly present — flagged as a positive
  observation, not a defect.
- **Function_05:** Given prior audits (`PRE_MODELLING_VERIFICATION.md`)
  flagged this function's output-magnitude outlier for extra scrutiny, its
  seed (47) was specifically double-checked for a possible copy-paste error
  from another function's notebook. **No such error was found** — the seed,
  dimensionality, and query generation are all correct and independent of
  any other function's notebook.
- **Function_08:** Highest dimensionality (8) and largest observation count
  (40) in the dataset; no scaling issues observed in validation or
  query-generation logic.

## Independent Correctness Verification

The Code Reviewer Agent independently recomputed, from scratch, the seed
(`42 + function_number`), dimensionality `d`, redraw-on-duplicate/
out-of-bounds loop, and six-decimal formatting for all 8 functions directly
against the real `initial_inputs.npy` files. Every one of the 8
independently-recomputed query strings matched the corresponding saved
`week_01_query.txt` exactly. All 8 seeds (43–50) are correctly and
consistently mapped to their function numbers with **no copy-paste
cross-function contamination** anywhere, including Function_05. All 8 query
files are within the official `[0.000000, 0.999999]` bound, use exactly 6
decimal places, hyphen-separated, no brackets/commas, and match the expected
per-function dimensionality (2, 2, 3, 4, 4, 5, 6, 8). No references to
`Week_02`, `04_Results` (other than benign "we do not write here"
disclaimers), `My_Capstone_1_Imperial`, or any path outside `../02_Data/` and
`../03_Queries/` were found in any of the 8 notebooks. Exploratory plots are
dimensionality-appropriate (2D scatter only for Function_01/02; per-feature
subplots elsewhere) with correct column indexing, matched array lengths, and
axis labels/titles throughout. No swallowed exceptions were found (zero
try/except blocks in any notebook — failures are loud via `assert`).

## Overall Verdict

**The Week_01 random-search baseline codebase is sound in substance.** All
seed, bounds, dimensionality, duplicate-prevention, and formatting logic is
correct and independently reproducible, and no Critical findings (rule
violations, wrong values, leakage) were found in any of the 8 notebooks.

The main actionable gap is procedural/reproducibility-related: the
notebooks are not fully self-contained, because the final artifact (the
saved query `.txt` file) is not written by code within the notebook itself —
full "rerun-and-get-the-same-files" reproducibility currently depends on an
undocumented manual step outside the reviewed code. Fixing this one
recurring Major gap (plus the two recurring Minor issues) would bring all 8
notebooks to a clean bill of health. No changes were made as part of this
review; these are recommendations only, pending explicit instruction to
implement them.


---

## Remediation Summary Addendum (2026-09-16)

Following explicit user instruction, the recurring Major and Minor findings
identified above have been remediated across all 8 `week_01_random_baseline.ipynb`
notebooks (Function_01–Function_08):

1. **[Major] Missing file-write code** — fixed in all 8 notebooks. Each
   notebook now contains an explicit code cell (added after the
   `query_string` cell) that writes `query_string` to
   `../03_Queries/week_01_query.txt`, reproducing the exact trailing-newline
   byte format that was already present on disk in every case (verified
   individually per file before editing).
2. **[Minor] Unused `import pandas as pd`** — removed from the imports cell
   in all 8 notebooks (no other imports were touched).
3. **[Minor] dtype checked by print only** — fixed in all 8 notebooks by
   adding `assert inputs.dtype == np.float64` / `assert outputs.dtype ==
   np.float64` alongside the existing print statements, matching the style
   of pre-existing shape/dimensionality `assert` checks. All 8 functions'
   real data was independently confirmed to be `float64` before adding
   these assertions.
4. **[Informational] Dead `rounded < 0.0` branch** — addressed in all 8
   notebooks by keeping the branch and adding a one-line comment clarifying
   it is a defensive, currently-unreachable check given `rng.random()`'s
   `[0, 1)` range, rather than removing it; the reachable `>= 1.0` bound and
   the `[0.000000, 0.999999]` guarantee on the final saved query are
   unchanged.

**No other changes were made.** Seeds (43–50), generated query values,
plots, and methodological narrative are unchanged from the original review.

**Verification:** every one of the 8 notebooks was re-executed end-to-end
(`jupyter nbconvert --to notebook --execute --inplace`) with zero errors.
After execution, SHA-256 checksums of all 8 `03_Queries/week_01_query.txt`
files and all 16 `02_Data/*.npy` files were confirmed identical to their
pre-remediation checksums (see each function's `06_Code_Review/
code_review_report.md` Remediation Addendum for the full per-file
before/after SHA-256 values). This confirms the new file-write cells
reproduce, rather than change, the already-correct saved query artifacts,
and that no dataset file was altered.
