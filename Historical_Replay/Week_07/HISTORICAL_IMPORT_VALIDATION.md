# Historical Import Validation — Genuine Week_07 Pairs

**Scope:** This validates the import of the eight independently-verified
**genuine historical Week 7** query-output pairs (Function_01–Function_08)
from the original source project (`~/Documents/GitHub/My_Capstone_1_Imperial`,
read-only) into `Historical_Replay/Week_07/Function_0X/` in this rebuild
project. This import is governed by `HISTORICAL_REPLAY_PROTOCOL.md` Rule 5
("Weeks 01–09 may be replayed only from verified query-output pairs") and,
for Function_05 specifically, Rule 9 (the documented Week_04/Week_07
duplicate).

**Important — these are genuine historical observations, not generated
values.** These eight pairs are the real Week 7 queries and their real
returned outputs, as recorded in the original capstone submission history
(the canonical `Results/query_output_ledger.csv`, cross-checked against
each function's own `observations.csv`, `summary.json`, and a third,
independent raw-source copy outside the source git repo). **They were
NOT produced by the Week 7 GP diagnostics notebooks** already built in
this rebuild project — those notebooks explicitly stop before proposing
any query and have no connection to these historical values.

# ⚠️ HEADLINE FINDING — Function_05's Week 7 Pair Is a Documented Duplicate of Week_04

**Function_05's Week 7 record is NOT a new observation.** The source
ledger's Week 7/Function 5 row (`[0,0,0,0] → 163.1225`) carries
`duplicate_of = week:4`, explicitly identifying it as a re-submission of
the same query already recorded in Week_04 (`duplicate_of` empty there —
the confirmed original occurrence). This was anticipated since Week_04's
import (`HISTORICAL_REPLAY_PROTOCOL.md` Rule 9), re-confirmed at every
subsequent week's cumulative-dataset build (Weeks 05, 06, 07), and is now
independently confirmed a final time, in both directions, by direct
inspection of this Week 7 ledger row and the original Week 4 row.

**Function_05's pair is documented in this import (`pair_provenance.md`,
`historical_query.txt`, `historical_output.txt` all exist for
completeness and audit-trail purposes) but will NOT be appended to any
cumulative dataset.** See `Function_05/pair_provenance.md` for full detail
on why, and exactly what happens instead when Week_08's cumulative
dataset is eventually built (Function_05's row count will carry forward
unchanged from Week_07, rather than gaining a new row like every other
function).

**Not yet appended to any cumulative dataset — Functions 01, 02, 03, 04,
06, 07, 08.** These seven pairs exist only as standalone files at this
stage — no `cumulative_inputs.npy`/`cumulative_outputs.npy` has been
updated with them yet, no acquisition-driven query has been generated,
and no `Week_08` has been created. **Function_05's pair will never be
appended, per the above.**

## Per-Function Validation Results

| Function | Ledger vs. per-function agreement | Dimensionality | Bounds [0.000000, 0.999999] | Six-decimal consistency | Finite scalar output | duplicate_of | Overall |
|---|---|---|---|---|---|---|---|
| Function_01 | PASS (exact match) | PASS (2/2) | PASS (min 0.35042, max 0.999543) | PASS (formatting-normalised) | PASS | empty | **PASS** |
| Function_02 | PASS (exact match) | PASS (2/2) | PASS (min 0.009675, max 0.999881) | PASS | PASS | empty | **PASS** |
| Function_03 | PASS (exact match) | PASS (3/3) | PASS (min 0.343678, max 0.66981) | PASS (formatting-normalised) | PASS | empty | **PASS** |
| Function_04 | PASS (exact match) | PASS (4/4) | PASS (min 0.028299, max 0.096005) | PASS | PASS | empty | **PASS** |
| Function_05 | PASS (exact match) | PASS (4/4) | PASS (all zero, inclusive bound) | PASS | PASS | **week:4 — CONFIRMED DUPLICATE** | **PASS (documented, not appended)** |
| Function_06 | PASS (exact match) | PASS (5/5) | PASS (min 0.035248, max 0.777884) | PASS (formatting-normalised) | PASS | empty | **PASS** |
| Function_07 | PASS (exact match) | PASS (6/6) | PASS (min 0.037758, max 0.591179) | PASS | PASS | empty | **PASS** |
| Function_08 | PASS (exact match) | PASS (8/8) | PASS (min 0.006802, max 0.887722) | PASS (formatting-normalised) | PASS | empty | **PASS** |

**8 of 8 functions: PASS** on data-integrity grounds. Seven functions are
genuine new observations awaiting a future cumulative-dataset append;
Function_05 is a genuine, ledger-confirmed duplicate that must never be
appended as if it were new.

## What Was Verified, Per Function

For every function, the following was independently confirmed:

1. The canonical ledger (`Results/query_output_ledger.csv`, SHA-256
   `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d`) Week 7
   row exactly matches that function's own `04_Results/observations.csv`
   row and `04_Results/summary.json` fields, coordinate-by-coordinate and
   value-for-value.
2. `03_Data/provenance.json` consistently names the ledger as the registry
   of record, and its `evidence_gap` field confirms verified evidence
   through Week 7 for every function.
3. The query vector has exactly the expected number of coordinates for
   that function (2, 2, 3, 4, 4, 5, 6, 8 for Functions 01–08).
4. Every coordinate lies within the official bound `[0.000000, 0.999999]`,
   including Function_05's exact-zero coordinates.
5. Every coordinate is consistent with a genuine 6-decimal-place submission
   (round-trip check); five coordinates (Function_01's 1st,
   Function_03's 2nd, Function_06's 1st and 5th, Function_08's 5th) were
   stored with a dropped trailing zero in the source files and were
   formatting-normalised to full 6dp in the saved query string — a
   text-representation change only, confirmed numerically identical.
6. The returned output is a single finite scalar in every case.
7. **Function_05's `duplicate_of` field is populated (`week:4`) — the
   only non-empty `duplicate_of` value anywhere in the entire 96-row
   ledger** (independently confirmed via a full-ledger scan). All other 7
   functions' `duplicate_of` fields are empty.
8. No genuine "proposal only"/"no confirmed return" marker was found
   anywhere for Week 7, for any function; all 8 rows carry
   `evidence_status: verified_cumulative_archive_pair`.

## Documentation Note — Minor Wording Inconsistency Flagged (Non-Blocking)

While verifying, the reviewer noted that `Week_07/01_Queries/README.md`
(and, identically, `Week_05/01_Queries/README.md`) in the source project
contains boilerplate phrasing ("the unavailable-return evidence gap...")
superficially similar in tone to the genuine Week 12/13 dispute language.
This was checked and found to be **inconsistent leftover copy-paste
boilerplate, not a genuine dispute** — contradicted by every
independently hash-verified source for Week 7 (ledger `evidence_status`,
`observations.csv`, `summary.json`, and the raw source files). Recorded
here for awareness only; does not affect this import's PASS verdict.

Full per-function detail, including exact source file paths, row numbers,
SHA-256 checksums, and any formatting-only normalisation applied, is in
each function's own `pair_provenance.md`.

## Agreement Between VERIFIED_PAIRS.csv and Per-Function Files

Every row in `VERIFIED_PAIRS.csv` was written directly from the same values
recorded in that function's own `historical_query.txt`/`historical_output.txt`
— confirmed to match exactly for all 8 functions. Function_05's row is
explicitly marked `VERIFIED_DUPLICATE_NOT_APPENDED` with `duplicate_of:
week:4`, distinguishing it from the other seven functions' plain
`VERIFIED` status.

## Separation from the Week 7 GP Diagnostics Notebooks

These eight pairs are labelled explicitly, in every `pair_provenance.md`
file, as **genuine historical Week 7 observations from the original source
project** — not outputs of, or in any way associated with, the Week 7 GP
diagnostics notebooks already built and independently reviewed in this
rebuild project. Those notebooks stop before proposing any query and have
no computational connection to these historical values.

## Files Created by This Import

- `Historical_Replay/Week_07/Function_0X/historical_query.txt` (X = 1–8)
- `Historical_Replay/Week_07/Function_0X/historical_output.txt` (X = 1–8)
- `Historical_Replay/Week_07/Function_0X/pair_provenance.md` (X = 1–8)
- `Historical_Replay/Week_07/VERIFIED_PAIRS.csv`
- `Historical_Replay/Week_07/HISTORICAL_IMPORT_VALIDATION.md` (this file)

**No cumulative dataset was appended to for any function, including
Function_05** — that remains a separate, later step, and Function_05 will
be permanently excluded from it for this week's pair. No acquisition-driven
query was generated. No `Week_08` was created. No notebook, existing
dataset, `Week_01`–`Week_06` file, or file in
`~/Documents/GitHub/My_Capstone_1_Imperial` was modified.

## Final Verdict

# **PASS — 8/8 genuine historical Week_07 pairs imported and independently verified; Function_05 correctly identified and documented as a confirmed duplicate of Week_04, to be excluded from all future cumulative-dataset appends for this pair**
