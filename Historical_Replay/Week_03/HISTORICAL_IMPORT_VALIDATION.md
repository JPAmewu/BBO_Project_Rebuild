# Historical Import Validation — Genuine Week_03 Pairs

**Scope:** This validates the import of the eight independently-verified
**genuine historical Week 3** query-output pairs (Function_01–Function_08)
from the original source project (`~/Documents/GitHub/My_Capstone_1_Imperial`,
read-only) into `Historical_Replay/Week_03/Function_0X/` in this rebuild
project. This import is governed by `HISTORICAL_REPLAY_PROTOCOL.md` Rule 5
("Weeks 01–09 may be replayed only from verified query-output pairs").

**Important — these are genuine historical observations, not generated
values.** These eight pairs are the real Week 3 queries and their real
returned outputs, as recorded in the original capstone submission history
(the canonical `Results/query_output_ledger.csv`, cross-checked against
each function's own `observations.csv` and `summary.json`). **They were
NOT produced by the Week 3 GP diagnostics notebooks** already built in
this rebuild project
(`Historical_Replay/Week_03/Function_0X/01_Notebook/week_03_gp_diagnostics.ipynb`)
— those notebooks explicitly stop before proposing any query and have no
connection to these historical values.

**Verification method:** performed by the Data Quality Agent, independently
re-deriving each pair from the source project's own primary records rather
than trusting the prior `HISTORICAL_REPLAY_INVENTORY.md` summary alone —
mirroring the same rigorous cross-check already applied to the Week 1 and
Week 2 imports. Two unusually small coordinate values (`6e-06` for
Function_02, `7e-06` for Function_06) were specifically double-checked via
raw byte-level file inspection and confirmed to be genuine, positive,
valid values — not parsing artifacts or negative numbers.

**Not yet appended to any cumulative dataset.** These pairs exist only as
standalone files at this stage — no `cumulative_inputs.npy`/
`cumulative_outputs.npy` has been updated with them, no acquisition-driven
query has been generated, and no `Week_04` has been created. Appending
these to build Week 4's cumulative starting datasets is the natural next
step, to be done separately.

## Per-Function Validation Results

| Function | Ledger vs. per-function agreement | Dimensionality | Bounds [0.000000, 0.999999] | Six-decimal consistency | Finite scalar output | Overall |
|---|---|---|---|---|---|---|
| Function_01 | PASS (exact match) | PASS (2/2) | PASS (min 0.334143, max 0.614168) | PASS | PASS | **PASS** |
| Function_02 | PASS (exact match) | PASS (2/2) | PASS (min 0.000006, max 0.334143) | PASS | PASS | **PASS** |
| Function_03 | PASS (exact match) | PASS (3/3) | PASS (min 0.057881, max 0.670026) | PASS | PASS | **PASS** |
| Function_04 | PASS (exact match) | PASS (4/4) | PASS (min 0.256803, max 0.461856) | PASS | PASS | **PASS** |
| Function_05 | PASS (exact match) | PASS (4/4) | PASS (min 0.255842, max 0.888985) | PASS | PASS | **PASS** |
| Function_06 | PASS (exact match) | PASS (5/5) | PASS (min 0.000007, max 0.672921) | PASS | PASS | **PASS** |
| Function_07 | PASS (exact match) | PASS (6/6) | PASS (min 0.143585, max 0.815792) | PASS | PASS | **PASS** |
| Function_08 | PASS (exact match) | PASS (8/8) | PASS (min 0.011239, max 0.992538) | PASS | PASS | **PASS** |

**8 of 8 functions: PASS. No function required stopping — every pair was
independently verifiable across three cross-referenced source files
(canonical ledger, `observations.csv`, `summary.json`), with no
discrepancy found for any function.**

## What Was Verified, Per Function

For every function, the following was independently confirmed:

1. The canonical ledger (`Results/query_output_ledger.csv`, v1.2, SHA-256
   `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d`) Week 3
   row exactly matches that function's own `04_Results/observations.csv`
   row and `04_Results/summary.json` fields, coordinate-by-coordinate and
   value-for-value.
2. `03_Data/provenance.json` consistently names the ledger as the registry
   of record.
3. The query vector has exactly the expected number of coordinates for
   that function (2, 2, 3, 4, 4, 5, 6, 8 for Functions 01–08).
4. Every coordinate lies within the official bound `[0.000000, 0.999999]`
   — including the two very small values (Function_02's `6e-06`,
   Function_06's `7e-06`), independently confirmed genuine and positive.
5. Every coordinate is consistent with a genuine 6-decimal-place submission
   (verified via `round(x * 1e6) / 1e6` round-trip, exact for all
   coordinates in all 8 functions).
6. The returned output is a single finite scalar in every case.

Full per-function detail, including exact source file paths, row numbers,
SHA-256 checksums, and any formatting-only normalisation applied, is in
each function's own `pair_provenance.md`.

## Note on Function_05

This function's Week 3 pair (`[0.255842, 0.841692, 0.888985, 0.860266]` →
`1035.6647479285914`) is numerically close to, but distinct from, the
already-imported Week 2 pair (`[0.255841, 0.841692, 0.888984, 0.860260]` →
`1035.6341457754475`). Both were independently confirmed as separate,
correctly-ordered rows in the source project's own `observations.csv`
(Week 2 = row 22, Week 3 = row 23), each with its own distinct output.
This continues the already-documented pattern for this function (see
`HISTORICAL_REPLAY_INVENTORY.md` and
`Historical_Replay/Week_02/INDEPENDENT_CUMULATIVE_REVIEW.md`) — it is not
a duplicate record, and neither value was altered here.

## Agreement Between VERIFIED_PAIRS.csv and Per-Function Files

Every row in `VERIFIED_PAIRS.csv` was written directly from the same values
recorded in that function's own `historical_query.txt`/`historical_output.txt`
— confirmed to match exactly for all 8 functions.

## Separation from the Week 3 GP Diagnostics Notebooks

These eight pairs are labelled explicitly, in every `pair_provenance.md`
file, as **genuine historical Week 3 observations from the original source
project** — not outputs of, or in any way associated with, the Week 3 GP
diagnostics notebooks already built and independently reviewed in this
rebuild project. Those notebooks stop before proposing any query and have
no computational connection to these historical values.

## Files Created by This Import

- `Historical_Replay/Week_03/Function_0X/historical_query.txt` (X = 1–8)
- `Historical_Replay/Week_03/Function_0X/historical_output.txt` (X = 1–8)
- `Historical_Replay/Week_03/Function_0X/pair_provenance.md` (X = 1–8)
- `Historical_Replay/Week_03/VERIFIED_PAIRS.csv`
- `Historical_Replay/Week_03/HISTORICAL_IMPORT_VALIDATION.md` (this file)

No cumulative dataset was appended to. No acquisition-driven query was
generated. No `Week_04` was created. No notebook, existing dataset,
`Week_01`/`Week_02` file, or file in `~/Documents/GitHub/My_Capstone_1_Imperial`
was modified.

## Final Verdict

# **PASS — 8/8 genuine historical Week_03 pairs imported and independently verified**
