# Import Validation — Historical Replay, Week_01

**Scope:** This validates the import of the eight independently-verified
historical Week_01 query-output pairs (Function_01–Function_08) from the
original source project (`~/Documents/GitHub/My_Capstone_1_Imperial`, read-only)
into `Historical_Replay/Week_01/` in this rebuild project. This import is
governed by `HISTORICAL_REPLAY_PROTOCOL.md` (Rules 1, 2, 5, 10, 11 in
particular). Verification was performed by the Data Quality Agent, re-deriving
each pair independently from the source project's own primary records rather
than trusting the prior `HISTORICAL_REPLAY_INVENTORY.md` summary alone.

**No file in the source project was modified.** No file in the existing
`Week_01` random-baseline rebuild work was modified. **No `Week_02` was
created.** No cumulative dataset (`02_Data/initial_inputs.npy`/
`initial_outputs.npy` in the rebuild's `Week_01`) was appended to — these
imported pairs live entirely in the separate `Historical_Replay/` area and
have not been merged into any cumulative dataset.

## Per-Function Validation Results

| Function | Ledger vs. per-function agreement | Dimensionality | Bounds [0.000000, 0.999999] | Six-decimal consistency | Finite scalar output | Overall |
|---|---|---|---|---|---|---|
| Function_01 | PASS (exact match) | PASS (2/2) | PASS (min 0.374540, max 0.950714) | PASS | PASS | **PASS** |
| Function_02 | PASS (exact match) | PASS (2/2) | PASS (min 0.374540, max 0.950714) | PASS | PASS | **PASS** |
| Function_03 | PASS (exact match) | PASS (3/3) | PASS (min 0.333333, max 0.666666) | PASS | PASS | **PASS** |
| Function_04 | PASS (exact match) | PASS (4/4) | PASS (min 0.111111, max 0.555555) | PASS | PASS | **PASS** |
| Function_05 | PASS (exact match) | PASS (4/4) | PASS (min 0.224189, max 0.879484) | PASS | PASS | **PASS** |
| Function_06 | PASS (exact match) | PASS (5/5) | PASS (min 0.154693, max 0.732552) | PASS | PASS | **PASS** |
| Function_07 | PASS (exact match) | PASS (6/6) | PASS (min 0.045091, max 0.641164) | PASS | PASS | **PASS** |
| Function_08 | PASS (exact match) | PASS (8/8) | PASS (min 0.073937, max 0.862321) | PASS | PASS | **PASS** |

**8 of 8 functions: PASS. No function required stopping — every pair was
independently verifiable, so no guessed or interpolated value was needed for
any function.**

## What Was Verified, Per Function

For every function, the following was independently confirmed (not merely
copied from the prior inventory):

1. The canonical ledger (`Results/query_output_ledger.csv`, v1.2, SHA-256
   `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d`) row for
   Week 1 exactly matches that function's own
   `04_Results/observations.csv` row and `04_Results/summary.json` fields,
   coordinate-by-coordinate and value-for-value.
2. The query vector has exactly the expected number of coordinates for that
   function (2, 2, 3, 4, 4, 5, 6, 8 for Functions 01–08 respectively).
3. Every coordinate lies within the official bound `[0.000000, 0.999999]`.
4. Every coordinate is consistent with a genuine 6-decimal-place submission
   (verified via `round(x * 1e6) / 1e6` round-trip, exact for all
   coordinates in all 8 functions — some values display with fewer than 6
   visible digits, e.g. `0.37454`, because a trailing zero was dropped when
   stored; this is expected and does not indicate a formatting problem).
5. The returned output is a single finite scalar (not an array, not NaN, not
   Inf) in every case.
6. The ledger file's own integrity: its SHA-256 matches both
   `Results/query_output_ledger.sha256` and the canonical v1.2 entry recorded
   in `Results/query_output_ledger_versions.json`.

Full per-function detail, including exact source file paths, row numbers, and
SHA-256 checksums of every file read, is in each function's own
`pair_provenance.md`.

## Not Independently Verified (Applies to All 8, Does Not Affect PASS Status)

- The ledger's `source_input`/`source_output` columns reference an external,
  non-repository archive (`Capstone_Folder/initial_data/week_01_inputs.txt`/
  `week_01_outputs.txt`) as the deepest upstream provenance layer. This
  archive was not located during this import and its hashes were not
  independently re-verified here. This does **not** affect the PASS
  verdicts above, which rest on the directly-confirmed agreement between the
  ledger and each function's own primary records (`observations.csv`/
  `summary.json`), which was checked directly for all 8 functions.
- The notebook `Week_01/02_Notebook/Week_1_Capstone.ipynb` referenced by the
  ledger was not opened or executed.

## Separation from the Rebuild's Own Week_01 Random-Baseline Work

Per `HISTORICAL_REPLAY_PROTOCOL.md` Rule 4, these eight historical pairs are
**not** associated with, and have **not** been paired with, the rebuild's own
Week_01 random-search-baseline queries
(`Week_01/Function_0X/03_Queries/week_01_query.txt`). Those remain separate,
unevaluated counterfactual recommendations. No file under `Week_01/` (the
rebuild's own work) was read, modified, or referenced as a data source during
this import — only the separate `Historical_Replay/Week_01/` area was
written to.

## Files Created by This Import

- `Historical_Replay/Week_01/Function_0X/historical_query.txt` (X = 1–8)
- `Historical_Replay/Week_01/Function_0X/historical_output.txt` (X = 1–8)
- `Historical_Replay/Week_01/Function_0X/pair_provenance.md` (X = 1–8)
- `Historical_Replay/Week_01/VERIFIED_PAIRS.csv`
- `Historical_Replay/Week_01/IMPORT_VALIDATION.md` (this file)

No cumulative dataset was appended to. No `Week_02` was created. No file in
`~/Documents/GitHub/My_Capstone_1_Imperial` or in the rebuild's existing
`Week_01/` work was modified.

## Final Verdict

# **PASS — 8/8 historical Week_01 pairs imported and independently verified**
