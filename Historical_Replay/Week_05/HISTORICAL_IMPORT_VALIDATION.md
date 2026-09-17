# Historical Import Validation — Genuine Week_05 Pairs

**Scope:** This validates the import of the eight independently-verified
**genuine historical Week 5** query-output pairs (Function_01–Function_08)
from the original source project (`~/Documents/GitHub/My_Capstone_1_Imperial`,
read-only) into `Historical_Replay/Week_05/Function_0X/` in this rebuild
project. This import is governed by `HISTORICAL_REPLAY_PROTOCOL.md` Rule 5
("Weeks 01–09 may be replayed only from verified query-output pairs").

**Important — these are genuine historical observations, not generated
values.** These eight pairs are the real Week 5 queries and their real
returned outputs, as recorded in the original capstone submission history
(the canonical `Results/query_output_ledger.csv`, cross-checked against
each function's own `observations.csv` and `summary.json`). **They were
NOT produced by the Week 5 GP diagnostics notebooks** already built in
this rebuild project
(`Historical_Replay/Week_05/Function_0X/01_Notebook/week_05_gp_diagnostics.ipynb`)
— those notebooks explicitly stop before proposing any query and have no
connection to these historical values.

**Verification method:** performed by the Data Quality Agent, independently
re-deriving each pair from the source project's own primary records rather
than trusting any prior summary — mirroring the same rigorous cross-check
already applied to the Week 1–4 imports.

**Not yet appended to any cumulative dataset.** These pairs exist only as
standalone files at this stage — no `cumulative_inputs.npy`/
`cumulative_outputs.npy` has been updated with them, no acquisition-driven
query has been generated, and no `Week_06` has been created.

## ⚠️ Function_05 Note — Distinct From the Documented Week_04/Week_07 Duplicate

Function_05 already has a documented duplicate relationship in this
project: its Week_04 pair (`[0,0,0,0] → 163.1225`) is duplicated by an
original Week_07 entry (per `HISTORICAL_REPLAY_PROTOCOL.md` Rule 9). This
Week_05 pair is a **completely different, distinct query**
(`[0.754614, 0.504566, 0.432986, 0.307039] → 0.9401160531598542`,
`observations.csv` row 25, immediately after the Week_04 row) — it has its
own empty `duplicate_of` field and is unrelated to the Week_04/Week_07
duplicate. See `Historical_Replay/Week_05/Function_05/pair_provenance.md`
for full detail.

## Per-Function Validation Results

| Function | Ledger vs. per-function agreement | Dimensionality | Bounds [0.000000, 0.999999] | Six-decimal consistency | Finite scalar output | duplicate_of | Overall |
|---|---|---|---|---|---|---|---|
| Function_01 | PASS (exact match) | PASS (2/2) | PASS (min 0.081699, max 0.897714) | PASS | PASS | empty | **PASS** |
| Function_02 | PASS (exact match) | PASS (2/2) | PASS (min 0.360931, max 0.555332) | PASS | PASS | empty | **PASS** |
| Function_03 | PASS (exact match) | PASS (3/3) | PASS (min 0.090769, max 0.522711) | PASS | PASS | empty | **PASS** |
| Function_04 | PASS (exact match) | PASS (4/4) | PASS (min 0.014095, max 0.713766) | PASS | PASS | empty | **PASS** |
| Function_05 | PASS (exact match) | PASS (4/4) | PASS (min 0.307039, max 0.754614) | PASS | PASS | empty (distinct from Wk4/Wk7 pair) | **PASS** |
| Function_06 | PASS (exact match) | PASS (5/5) | PASS (min 0.022732, max 0.960907) | PASS (formatting-normalised) | PASS | empty | **PASS** |
| Function_07 | PASS (exact match) | PASS (6/6) | PASS (min 0.025337, max 0.910075) | PASS (formatting-normalised) | PASS | empty | **PASS** |
| Function_08 | PASS (exact match) | PASS (8/8) | PASS (min 0.022977, max 0.963757) | PASS | PASS | empty | **PASS** |

**8 of 8 functions: PASS.** No function required stopping — every pair was
independently verifiable across three cross-referenced source files
(canonical ledger, `observations.csv`, `summary.json`), with no data-integrity
discrepancy found for any function. A full-ledger scan across all weeks and
functions found no other row duplicating any of these 8 Week 5 pairs.

## What Was Verified, Per Function

For every function, the following was independently confirmed:

1. The canonical ledger (`Results/query_output_ledger.csv`, SHA-256
   `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d`) Week 5
   row exactly matches that function's own `04_Results/observations.csv`
   row and `04_Results/summary.json` fields, coordinate-by-coordinate and
   value-for-value.
2. `03_Data/provenance.json` consistently names the ledger as the registry
   of record, and its `evidence_gap` field confirms verified evidence
   through Week 5 for every function.
3. The query vector has exactly the expected number of coordinates for
   that function (2, 2, 3, 4, 4, 5, 6, 8 for Functions 01–08).
4. Every coordinate lies within the official bound `[0.000000, 0.999999]`.
5. Every coordinate is consistent with a genuine 6-decimal-place submission
   (round-trip check); two coordinates (Function_06's 5th, Function_07's
   3rd) were stored with a dropped trailing zero in the source files and
   were formatting-normalised to full 6dp in the saved query string — a
   text-representation change only, confirmed numerically identical.
6. The returned output is a single finite scalar in every case.
7. No row's `duplicate_of` field is populated — all 8 pairs are original
   entries, including Function_05's, which is a distinct pair from its
   already-documented Week_04/Week_07 duplicate relationship.
8. No "proposal only"/"no confirmed return" marker (of the kind flagging
   Weeks 12-13 in `HISTORICAL_REPLAY_INVENTORY.md`) was found anywhere for
   Week 5, for any function. **Clarification**: the source project's Week 5
   notebooks do contain `"status": "proposed_not_evaluated"` text, but this
   refers to each notebook's own next-candidate (Week 6) acquisition
   proposal — the standard end-of-notebook output of a BO workflow — not to
   the Week 5 historical return itself. All 8 Week 5 ledger rows carry
   `evidence_status: verified_cumulative_archive_pair`, structurally
   different from the genuine Week 12/13 dispute, which flags the *week's
   own return* (not a future proposal) as unconfirmed. This distinction is
   stated explicitly here to avoid any future reader conflating
   "next-query proposal" language with "this week's return is unconfirmed."

Full per-function detail, including exact source file paths, row numbers,
SHA-256 checksums, and any formatting-only normalisation applied, is in
each function's own `pair_provenance.md`.

## Data-Quality Observation (Non-Blocking)

Function_01's Week 5 output (`7.6512102255565565e-239`) continues a
genuine, project-wide pattern for this function: its full observation
history spans an extreme dynamic range (roughly 1e-16 to 1e-239), close to
double-precision underflow. This is a pre-existing characteristic of
Function_01 across all weeks, not an anomaly introduced by this import,
and does not block the import — but is worth keeping in mind for any
future log-scale GP modelling of this function.

## Agreement Between VERIFIED_PAIRS.csv and Per-Function Files

Every row in `VERIFIED_PAIRS.csv` was written directly from the same values
recorded in that function's own `historical_query.txt`/`historical_output.txt`
— confirmed to match exactly for all 8 functions.

## Separation from the Week 5 GP Diagnostics Notebooks

These eight pairs are labelled explicitly, in every `pair_provenance.md`
file, as **genuine historical Week 5 observations from the original source
project** — not outputs of, or in any way associated with, the Week 5 GP
diagnostics notebooks already built and independently reviewed in this
rebuild project. Those notebooks stop before proposing any query and have
no computational connection to these historical values.

## Files Created by This Import

- `Historical_Replay/Week_05/Function_0X/historical_query.txt` (X = 1–8)
- `Historical_Replay/Week_05/Function_0X/historical_output.txt` (X = 1–8)
- `Historical_Replay/Week_05/Function_0X/pair_provenance.md` (X = 1–8)
- `Historical_Replay/Week_05/VERIFIED_PAIRS.csv`
- `Historical_Replay/Week_05/HISTORICAL_IMPORT_VALIDATION.md` (this file)

No cumulative dataset was appended to. No acquisition-driven query was
generated. No `Week_06` was created. No notebook, existing dataset,
`Week_01`/`Week_02`/`Week_03`/`Week_04` file, or file in
`~/Documents/GitHub/My_Capstone_1_Imperial` was modified.

## Final Verdict

# **PASS — 8/8 genuine historical Week_05 pairs imported and independently verified**
