# Historical Import Validation — Genuine Week_02 Pairs

**Scope:** This validates the import of the eight independently-verified
**genuine historical Week 2** query-output pairs (Function_01–Function_08)
from the original source project (`~/Documents/GitHub/My_Capstone_1_Imperial`,
read-only) into `Historical_Replay/Week_02/Function_0X/` in this rebuild
project. This import is governed by `HISTORICAL_REPLAY_PROTOCOL.md` Rule 5
("Weeks 01–09 may be replayed only from verified query-output pairs").

**Important — these are genuine historical observations, not generated
values.** These eight pairs are the real Week 2 queries and their real
returned outputs, as recorded in the original capstone submission history
(the canonical `Results/query_output_ledger.csv`, cross-checked against
each function's own `observations.csv` and `summary.json`). **They were
NOT produced by the Week 2 GP surrogate notebooks** already built in this
rebuild project
(`Historical_Replay/Week_02/Function_0X/01_Notebook/week_02_eda_gp_surrogate.ipynb`)
— those notebooks explicitly stop before proposing any query and have no
connection to these historical values.

**Verification method:** performed by the Data Quality Agent, independently
re-deriving each pair from the source project's own primary records rather
than trusting the prior `HISTORICAL_REPLAY_INVENTORY.md` summary alone —
mirroring the same rigorous cross-check already applied to the Week 1
import.

**Not yet appended to any cumulative dataset.** These pairs exist only as
standalone files at this stage — no `cumulative_inputs.npy`/
`cumulative_outputs.npy` has been updated with them, no acquisition-driven
query has been generated, and no `Week_03` has been created.

## Per-Function Validation Results

| Function | Ledger vs. per-function agreement | Dimensionality | Bounds [0.000000, 0.999999] | Six-decimal consistency | Finite scalar output | Overall |
|---|---|---|---|---|---|---|
| Function_01 | PASS (exact match) | PASS (2/2) | PASS (min 0.536429, max 0.835362) | PASS | PASS | **PASS** |
| Function_02 | PASS (exact match) | PASS (2/2) | PASS (min 0.932731, max 0.978752) | PASS | PASS | **PASS** |
| Function_03 | PASS (exact match) | PASS (3/3) | PASS (min 0.657452, max 0.998464) | PASS | PASS | **PASS** |
| Function_04 | PASS (exact match) | PASS (4/4) | PASS (min 0.006280, max 0.960202) | PASS | PASS | **PASS** |
| Function_05 | PASS (exact match) | PASS (4/4) | PASS (min 0.255841, max 0.888984) | PASS | PASS | **PASS** |
| Function_06 | PASS (exact match) | PASS (5/5) | PASS (min 0.282753, max 0.972905) | PASS | PASS | **PASS** |
| Function_07 | PASS (exact match) | PASS (6/6) | PASS (min 0.193630, max 0.900100) | PASS | PASS | **PASS** |
| Function_08 | PASS (exact match) | PASS (8/8) | PASS (min 0.083802, max 0.999322) | PASS | PASS | **PASS** |

**8 of 8 functions: PASS. No function required stopping — every pair was
independently verifiable across three cross-referenced source files
(canonical ledger, `observations.csv`, `summary.json`), with no
discrepancy found for any function.**

## What Was Verified, Per Function

For every function, the following was independently confirmed:

1. The canonical ledger (`Results/query_output_ledger.csv`, v1.2, SHA-256
   `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d`) Week 2
   row exactly matches that function's own `04_Results/observations.csv`
   row and `04_Results/summary.json` fields, coordinate-by-coordinate and
   value-for-value.
2. `03_Data/provenance.json` consistently names the ledger as the registry
   of record (does not restate the pair independently, but is not
   contradictory).
3. The query vector has exactly the expected number of coordinates for
   that function (2, 2, 3, 4, 4, 5, 6, 8 for Functions 01–08).
4. Every coordinate lies within the official bound `[0.000000, 0.999999]`.
5. Every coordinate is consistent with a genuine 6-decimal-place submission
   (verified via `round(x * 1e6) / 1e6` round-trip, exact for all
   coordinates in all 8 functions — several values display with fewer than
   6 visible digits because trailing zeros were dropped, e.g. `0.00628` =
   `0.006280`; this is expected and correctly padded in the saved query
   files, with no change to the underlying numeric value).
6. The returned output is a single finite scalar (not an array, not NaN,
   not Inf) in every case.

Full per-function detail, including exact source file paths, row numbers,
SHA-256 checksums, and any formatting-only normalisation applied, is in
each function's own `pair_provenance.md`.

## Agreement Between VERIFIED_PAIRS.csv and Per-Function Files

Every row in `VERIFIED_PAIRS.csv` was written directly from the same values
recorded in that function's own `historical_query.txt`/`historical_output.txt`
— confirmed to match exactly (byte-for-byte on the query string, exact
numeric string on the output) for all 8 functions.

## Separation from the Week 2 GP Surrogate Notebooks

These eight pairs are labelled explicitly, in every `pair_provenance.md`
file, as **genuine historical Week 2 observations from the original source
project** — not outputs of, or in any way associated with, the Week 2 EDA/
GP surrogate notebooks already built in this rebuild project. Those
notebooks were built and independently verified in a separate, earlier step
(`Historical_Replay/Week_02/FINAL_EVALUATION.md`) using only the cumulative
Week 1 data, and explicitly stop before proposing any query — they have no
computational connection to these historical values.

## Files Created by This Import

- `Historical_Replay/Week_02/Function_0X/historical_query.txt` (X = 1–8)
- `Historical_Replay/Week_02/Function_0X/historical_output.txt` (X = 1–8)
- `Historical_Replay/Week_02/Function_0X/pair_provenance.md` (X = 1–8)
- `Historical_Replay/Week_02/VERIFIED_PAIRS.csv`
- `Historical_Replay/Week_02/HISTORICAL_IMPORT_VALIDATION.md` (this file)

No cumulative dataset was appended to. No acquisition-driven query was
generated. No `Week_03` was created. No notebook, existing dataset,
`Week_01` file, or file in `~/Documents/GitHub/My_Capstone_1_Imperial` was
modified.

## Final Verdict

# **PASS — 8/8 genuine historical Week_02 pairs imported and independently verified**
