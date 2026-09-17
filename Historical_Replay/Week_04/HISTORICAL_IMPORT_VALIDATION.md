# Historical Import Validation — Genuine Week_04 Pairs

**Scope:** This validates the import of the eight independently-verified
**genuine historical Week 4** query-output pairs (Function_01–Function_08)
from the original source project (`~/Documents/GitHub/My_Capstone_1_Imperial`,
read-only) into `Historical_Replay/Week_04/Function_0X/` in this rebuild
project. This import is governed by `HISTORICAL_REPLAY_PROTOCOL.md` Rule 5
("Weeks 01–09 may be replayed only from verified query-output pairs").

**Important — these are genuine historical observations, not generated
values.** These eight pairs are the real Week 4 queries and their real
returned outputs, as recorded in the original capstone submission history
(the canonical `Results/query_output_ledger.csv`, cross-checked against
each function's own `observations.csv` and `summary.json`). **They were
NOT produced by the Week 4 GP diagnostics notebooks** already built in
this rebuild project
(`Historical_Replay/Week_04/Function_0X/01_Notebook/week_04_gp_diagnostics.ipynb`)
— those notebooks explicitly stop before proposing any query and have no
connection to these historical values.

**Verification method:** performed by the Data Quality Agent, independently
re-deriving each pair from the source project's own primary records rather
than trusting the prior `HISTORICAL_REPLAY_INVENTORY.md` summary alone —
mirroring the same rigorous cross-check already applied to the Week 1–3
imports.

**Not yet appended to any cumulative dataset.** These pairs exist only as
standalone files at this stage — no `cumulative_inputs.npy`/
`cumulative_outputs.npy` has been updated with them, no acquisition-driven
query has been generated, and no `Week_05` has been created.

## ⚠️ Important Note — Function_05 Is the Original Occurrence of a Later Duplicate

Function_05's Week 4 query, `[0.0, 0.0, 0.0, 0.0]` → `163.1225`, is
**independently duplicated by a later week (Week 7)** in the source
project's own records. This was already identified in
`HISTORICAL_REPLAY_INVENTORY.md` and codified as
`HISTORICAL_REPLAY_PROTOCOL.md` Rule 9: *"Record the Week_07 Function_05
repetition of the Week_04 query as a historical duplicate and do not
append it twice as a new observation."*

This import step confirms, via direct inspection of the source ledger,
that **this Week 4 row is the original, authoritative occurrence** — its
own `duplicate_of` field is empty, while the later Week 7 row for
Function 5 is the one flagged as `duplicate_of: week:4` (pointing back to
this row). No action is needed now; this note exists so that when Week 7
is eventually processed, this pair is not appended a second time as an
independent new observation. See `Historical_Replay/Week_04/Function_05/pair_provenance.md`
for full detail.

## Per-Function Validation Results

| Function | Ledger vs. per-function agreement | Dimensionality | Bounds [0.000000, 0.999999] | Six-decimal consistency | Finite scalar output | Overall |
|---|---|---|---|---|---|---|
| Function_01 | PASS (exact match) | PASS (2/2) | PASS (min 0.321598, max 0.872626) | PASS | PASS | **PASS** |
| Function_02 | PASS (exact match) | PASS (2/2) | PASS (min 0.197553, max 0.808683) | PASS | PASS | **PASS** |
| Function_03 | PASS (exact match) | PASS (3/3) | PASS (min 0.110542, max 0.650132) | PASS | PASS | **PASS** |
| Function_04 | PASS (exact match) | PASS (4/4) | PASS (min 0.275065, max 0.715715) | PASS | PASS | **PASS** |
| Function_05 | PASS (exact match) | PASS (4/4) | PASS (min 0.0, max 0.0 — exact zero, original occurrence confirmed) | PASS | PASS | **PASS** |
| Function_06 | PASS (exact match) | PASS (5/5) | PASS (min 0.022807, max 0.974790) | PASS | PASS | **PASS** |
| Function_07 | PASS (exact match) | PASS (6/6) | PASS (min 0.016167, max 0.773003) | PASS | PASS | **PASS** |
| Function_08 | PASS (exact match) | PASS (8/8) | PASS (min 0.106477, max 0.904740) | PASS | PASS | **PASS** |

**8 of 8 functions: PASS. No function required stopping — every pair was
independently verifiable across three cross-referenced source files
(canonical ledger, `observations.csv`, `summary.json`), with no
discrepancy found for any function.**

## What Was Verified, Per Function

For every function, the following was independently confirmed:

1. The canonical ledger (`Results/query_output_ledger.csv`, v1.2, SHA-256
   `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d`) Week 4
   row exactly matches that function's own `04_Results/observations.csv`
   row and `04_Results/summary.json` fields, coordinate-by-coordinate and
   value-for-value.
2. `03_Data/provenance.json` consistently names the ledger as the registry
   of record.
3. The query vector has exactly the expected number of coordinates for
   that function (2, 2, 3, 4, 4, 5, 6, 8 for Functions 01–08).
4. Every coordinate lies within the official bound `[0.000000, 0.999999]`
   — including Function_05's exact-zero coordinates, confirmed to satisfy
   the inclusive lower bound.
5. Every coordinate is consistent with a genuine 6-decimal-place submission
   (verified via `round(x * 1e6) / 1e6` round-trip, exact for all
   coordinates in all 8 functions).
6. The returned output is a single finite scalar in every case.
7. For Function_05 specifically: the ledger's `duplicate_of` field for
   this Week 4 row was directly inspected and confirmed empty, establishing
   this as the original occurrence rather than itself a duplicate.

Full per-function detail, including exact source file paths, row numbers,
SHA-256 checksums, and any formatting-only normalisation applied, is in
each function's own `pair_provenance.md`.

## Agreement Between VERIFIED_PAIRS.csv and Per-Function Files

Every row in `VERIFIED_PAIRS.csv` was written directly from the same values
recorded in that function's own `historical_query.txt`/`historical_output.txt`
— confirmed to match exactly for all 8 functions.

## Separation from the Week 4 GP Diagnostics Notebooks

These eight pairs are labelled explicitly, in every `pair_provenance.md`
file, as **genuine historical Week 4 observations from the original source
project** — not outputs of, or in any way associated with, the Week 4 GP
diagnostics notebooks already built and independently reviewed in this
rebuild project. Those notebooks stop before proposing any query and have
no computational connection to these historical values.

## Files Created by This Import

- `Historical_Replay/Week_04/Function_0X/historical_query.txt` (X = 1–8)
- `Historical_Replay/Week_04/Function_0X/historical_output.txt` (X = 1–8)
- `Historical_Replay/Week_04/Function_0X/pair_provenance.md` (X = 1–8)
- `Historical_Replay/Week_04/VERIFIED_PAIRS.csv`
- `Historical_Replay/Week_04/HISTORICAL_IMPORT_VALIDATION.md` (this file)

No cumulative dataset was appended to. No acquisition-driven query was
generated. No `Week_05` was created. No notebook, existing dataset,
`Week_01`/`Week_02`/`Week_03` file, or file in
`~/Documents/GitHub/My_Capstone_1_Imperial` was modified.

## Final Verdict

# **PASS — 8/8 genuine historical Week_04 pairs imported and independently verified**
