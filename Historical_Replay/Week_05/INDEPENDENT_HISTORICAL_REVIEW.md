# Independent Historical Review — Week_05 Genuine Historical Pair Import

**Scope:** Independent re-verification, by the ML Reviewer Agent, of the
eight genuine historical Week 5 query-output pairs imported into
`Historical_Replay/Week_05/Function_0X/` and documented in
`HISTORICAL_IMPORT_VALIDATION.md` and `VERIFIED_PAIRS.csv`. This mirrors
the same independent-review step already completed for the Week 1–4
imports. No claim in this project's own documentation was trusted without
independent re-derivation from the source project's primary files.

**Method:** every value was independently re-extracted from
`~/Documents/GitHub/My_Capstone_1_Imperial/Results/query_output_ledger.csv`
(all 8 Week 5 rows located via direct row filtering), cross-checked
against each function's own `04_Results/observations.csv` (final row) and
`04_Results/summary.json`, with numeric (not merely textual) equality
checks against this rebuild project's `historical_query.txt`/
`historical_output.txt` files.

## Per-Function Results

| Function | d | Ledger match | observations.csv/summary.json match | Bounds | Finite | duplicate_of | Result |
|---|---|---|---|---|---|---|---|
| Function_01 | 2 | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_02 | 2 | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_03 | 3 | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_04 | 4 | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_05 | 4 | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_06 | 5 | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_07 | 6 | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_08 | 8 | PASS | PASS | PASS | PASS | empty | **PASS** |

All 8 functions' `historical_query.txt`/`historical_output.txt` values
were confirmed to numerically equal (via `float()` comparison, not string
comparison) the independently re-extracted source ledger row.

## Function_05 Duplicate-Relationship Check — Confirmed Distinct

Independently pulled both relevant ledger rows directly:
- **Week 4, Function 5**: `[0.0,0.0,0.0,0.0] → 163.1225`, `duplicate_of`
  empty (the documented original occurrence, later duplicated by Week 7).
- **Week 5, Function 5**: `[0.754614,0.504566,0.432986,0.307039] →
  0.9401160531598542`, `duplicate_of` empty.

A full-ledger scan (`csv.DictReader` across all 96 data rows) found
exactly **one** populated `duplicate_of` value in the entire ledger — Week
7, Function 5, referencing Week 4 — confirming the Week 5 pair is
completely unrelated to that documented duplicate relationship.

## Full-Ledger Duplicate Scan

Each of the 8 Week 5 query strings was searched across the entire 97-line
ledger; each returned exactly one match (the Week 5 row itself). **No
other week or function duplicates any of these 8 Week 5 pairs.**

## "Proposal Only" Check — None Found, With Clarification

All 96 ledger rows carry `evidence_status` of either
`verified_cumulative_archive_pair` or
`verified_cumulative_archive_pair_after_query_record_reconciliation` — no
row anywhere carries the `proposal_only_return_unavailable` status used
for the genuinely disputed Weeks 12/13.

**Clarification added to `HISTORICAL_IMPORT_VALIDATION.md`** as a result
of this review: the source project's Week 5 notebooks do contain
`"status": "proposed_not_evaluated"` text, but this refers to each
notebook's own next-candidate (Week 6) acquisition proposal — standard
end-of-notebook BO workflow output — not to the Week 5 historical return
itself, which is fully verified. This is structurally different from the
Week 12/13 dispute (which flags the week's *own* return, not a future
proposal, as unconfirmed).

## Formatting Normalisation Checks

Both claimed normalisations independently confirmed value-preserving:
`float('0.89756') == float('0.897560')` (Function_06) and
`float('0.72779') == float('0.727790')` (Function_07) — trailing-zero
padding only.

## VERIFIED_PAIRS.csv Agreement

Confirmed row-by-row against each function's `historical_query.txt`/
`historical_output.txt` — all 8 rows match exactly, including the two
normalised coordinates.

## Scope-Boundary Confirmation

- No `Week_06` directory exists anywhere in the project.
- No `03_Queries` directory exists anywhere under `Historical_Replay/Week_05/`.
- No cumulative dataset was appended: each function's
  `02_Data/cumulative_inputs.npy` row count is exactly one row short of
  the full Week 5 total in `summary.json` (e.g. 14 vs. 15 for
  Function_01), confirming the historical pair has not yet been merged in.
- Spot-checked two GP diagnostics notebooks (Function_01, Function_05) via
  direct JSON parse — neither references either historical pair's values.
- `git status` on the source project: clean, confirming no modification.
- `git status`/`git diff --stat` on the rebuild project itself: the only
  change anywhere in the working tree is the new, untracked
  `Historical_Replay/Week_05/` directory — no Week_01–04 file changed.

## Open Items Noted by the Reviewer (Not Failures)

- Only 2 of 8 Week 5 notebooks were spot-checked via JSON parse for
  absence of historical values (a full 8/8 parse, as done for Week_04,
  was not repeated).
- Only Function_05's `pair_provenance.md` source checksums were
  independently recomputed; the other 7 were spot-checked for
  value/dimension/bounds consistency but not re-hashed byte-for-byte.
- Only the source project's current working-tree status was checked, not
  its full commit history.

None of these affect the verdict below.

## Final Verdict

# **PASS — Week_05 historical pair import independently re-verified, 8/8 functions, all 10 checks PASS**

No numeric discrepancy, no leakage or premature append into cumulative
datasets, no fabricated value, no populated `duplicate_of` outside the
already-documented Week_04/Week_07 Function_05 relationship, no duplicate
Week 5 query anywhere else in the ledger, and no unauthorized modification
to the source project or to this rebuild project's prior weeks was found.
