# Historical Import Validation — Genuine Week_06 Pairs

**Scope:** This validates the import of the eight independently-verified
**genuine historical Week 6** query-output pairs (Function_01–Function_08)
from the original source project (`~/Documents/GitHub/My_Capstone_1_Imperial`,
read-only) into `Historical_Replay/Week_06/Function_0X/` in this rebuild
project. This import is governed by `HISTORICAL_REPLAY_PROTOCOL.md` Rule 5
("Weeks 01–09 may be replayed only from verified query-output pairs").

**Important — these are genuine historical observations, not generated
values.** These eight pairs are the real Week 6 queries and their real
returned outputs, as recorded in the original capstone submission history
(the canonical `Results/query_output_ledger.csv`, cross-checked against
each function's own `observations.csv`, `summary.json`, and a third,
independent raw-source copy outside the source git repo). **They were
NOT produced by the Week 6 GP diagnostics notebooks** already built in
this rebuild project
(`Historical_Replay/Week_06/Function_0X/01_Notebook/week_06_gp_diagnostics.ipynb`)
— those notebooks explicitly stop before proposing any query and have no
connection to these historical values.

**Verification method:** performed by the Data Quality Agent, independently
re-deriving each pair from the source project's own primary records rather
than trusting any prior summary — mirroring the same rigorous cross-check
already applied to the Week 1–5 imports, with an additional third-source
cross-check this week against raw text files outside the source git repo
(`~/Downloads/Capstone_Folder/week_06_data/`), referenced by the ledger's
own checksum fields.

**Not yet appended to any cumulative dataset.** These pairs exist only as
standalone files at this stage — no `cumulative_inputs.npy`/
`cumulative_outputs.npy` has been updated with them, no acquisition-driven
query has been generated, and no `Week_07` has been created.

## ⚠️ Function_05 Note — Distinct From the Documented Week_04/Week_07 Duplicate

Function_05 already has a documented duplicate relationship in this
project: its Week_04 pair (`[0,0,0,0] → 163.1225`) is duplicated by an
original Week_07 entry (per `HISTORICAL_REPLAY_PROTOCOL.md` Rule 9). This
Week_06 pair is a **completely different, distinct query**
(`[0.654981, 0.602894, 0.898891, 0.589622] → 281.9115728306016`, the 6th
recorded row for this function) — it has its own empty `duplicate_of`
field and is unrelated to the Week_04/Week_07 duplicate. Confirmed by
reading all 12 of Function_05's ledger rows directly. See
`Historical_Replay/Week_06/Function_05/pair_provenance.md` for full detail.

## ⚠️ Forward-Looking Note — Function_05 Week 9 Out-of-Bounds Coordinate

While independently verifying Function_05's full ledger history for the
duplicate check above, the verifying agent incidentally discovered that
Function_05's **Week 9** query in the source ledger has a coordinate
exactly equal to `1.0` (`[0.319924, 0.675406, 0.431628, 1.0]`), which
would violate the official bound `[0.000000, 0.999999]` (Rule 16). This
is unrelated to, and does not affect, this Week 6 import — it is recorded
here only so it is not lost before Week 9 is eventually processed, at
which point it will need explicit handling.

## Per-Function Validation Results

| Function | Ledger vs. per-function agreement | Dimensionality | Bounds [0.000000, 0.999999] | Six-decimal consistency | Finite scalar output | duplicate_of | Overall |
|---|---|---|---|---|---|---|---|
| Function_01 | PASS (exact match) | PASS (2/2) | PASS (min 0.222521, max 0.999673) | PASS | PASS | empty | **PASS** |
| Function_02 | PASS (exact match) | PASS (2/2) | PASS (min 0.473151, max 0.950706) | PASS | PASS | empty | **PASS** |
| Function_03 | PASS (exact match) | PASS (3/3) | PASS (min 0.418776, max 0.695421) | PASS | PASS | empty | **PASS** |
| Function_04 | PASS (exact match) | PASS (4/4) | PASS (min 0.441826, max 0.942545) | PASS | PASS | empty | **PASS** |
| Function_05 | PASS (exact match) | PASS (4/4) | PASS (min 0.589622, max 0.898891) | PASS | PASS | empty (distinct from Wk4/Wk7 pair) | **PASS** |
| Function_06 | PASS (exact match) | PASS (5/5) | PASS (min 0.192076, max 0.805757) | PASS (formatting-normalised) | PASS | empty | **PASS** |
| Function_07 | PASS (exact match) | PASS (6/6) | PASS (min 0.042873, max 0.515646) | PASS (formatting-normalised) | PASS | empty | **PASS** |
| Function_08 | PASS (exact match) | PASS (8/8) | PASS (min 0.022809, max 0.965550) | PASS (formatting-normalised) | PASS | empty | **PASS** |

**8 of 8 functions: PASS.** No function required stopping — every pair was
independently verifiable across four cross-referenced sources (canonical
ledger, `observations.csv`, `summary.json`, and a third-party raw-text
copy outside the source git repo), with no data-integrity discrepancy
found for any function.

## What Was Verified, Per Function

For every function, the following was independently confirmed:

1. The canonical ledger (`Results/query_output_ledger.csv`, SHA-256
   `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d`) Week 6
   row exactly matches that function's own `04_Results/observations.csv`
   row and `04_Results/summary.json` fields, coordinate-by-coordinate and
   value-for-value.
2. `03_Data/provenance.json` consistently names the ledger as the registry
   of record, and its `evidence_gap` field confirms verified evidence
   through Week 6 for every function.
3. The query vector has exactly the expected number of coordinates for
   that function (2, 2, 3, 4, 4, 5, 6, 8 for Functions 01–08).
4. Every coordinate lies within the official bound `[0.000000, 0.999999]`.
5. Every coordinate is consistent with a genuine 6-decimal-place submission
   (round-trip check); five coordinates (Function_06's 1st and 5th,
   Function_07's 4th, Function_08's 5th and 6th) were stored with a
   dropped trailing zero in the source files and were formatting-normalised
   to full 6dp in the saved query string — a text-representation change
   only, confirmed numerically identical.
6. The returned output is a single finite scalar in every case.
7. No row's `duplicate_of` field is populated — all 8 pairs are original
   entries, including Function_05's, which is confirmed distinct from its
   already-documented Week_04/Week_07 duplicate relationship.
8. No "proposal only"/"no confirmed return" marker (of the kind flagging
   Weeks 12-13 in `HISTORICAL_REPLAY_INVENTORY.md`) was found anywhere for
   Week 6, for any function; all 8 rows carry the "clean" (non-reconciled)
   `evidence_status: verified_cumulative_archive_pair`.

Full per-function detail, including exact source file paths, row numbers,
SHA-256 checksums, and any formatting-only normalisation applied, is in
each function's own `pair_provenance.md`.

## Agreement Between VERIFIED_PAIRS.csv and Per-Function Files

Every row in `VERIFIED_PAIRS.csv` was written directly from the same values
recorded in that function's own `historical_query.txt`/`historical_output.txt`
— confirmed to match exactly for all 8 functions.

## Separation from the Week 6 GP Diagnostics Notebooks

These eight pairs are labelled explicitly, in every `pair_provenance.md`
file, as **genuine historical Week 6 observations from the original source
project** — not outputs of, or in any way associated with, the Week 6 GP
diagnostics notebooks already built and independently reviewed in this
rebuild project. Those notebooks stop before proposing any query and have
no computational connection to these historical values.

## Files Created by This Import

- `Historical_Replay/Week_06/Function_0X/historical_query.txt` (X = 1–8)
- `Historical_Replay/Week_06/Function_0X/historical_output.txt` (X = 1–8)
- `Historical_Replay/Week_06/Function_0X/pair_provenance.md` (X = 1–8)
- `Historical_Replay/Week_06/VERIFIED_PAIRS.csv`
- `Historical_Replay/Week_06/HISTORICAL_IMPORT_VALIDATION.md` (this file)

No cumulative dataset was appended to. No acquisition-driven query was
generated. No `Week_07` was created. No notebook, existing dataset,
`Week_01`–`Week_05` file, or file in
`~/Documents/GitHub/My_Capstone_1_Imperial` was modified.

## Final Verdict

# **PASS — 8/8 genuine historical Week_06 pairs imported and independently verified**
