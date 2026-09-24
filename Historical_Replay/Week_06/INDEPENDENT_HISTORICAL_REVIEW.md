# Independent Historical Review — Week_06 Genuine Historical Pair Import

**Scope:** Independent re-verification, by the ML Reviewer Agent, of the
eight genuine historical Week 6 query-output pairs imported into
`Historical_Replay/Week_06/Function_0X/` and documented in
`HISTORICAL_IMPORT_VALIDATION.md` and `VERIFIED_PAIRS.csv`. This mirrors
the same independent-review step already completed for the Week 1–5
imports. No claim in this project's own documentation was trusted without
independent re-derivation from the source project's primary files.

**Method:** every value was independently re-extracted from
`~/Documents/GitHub/My_Capstone_1_Imperial/Results/query_output_ledger.csv`
(all 8 Week 6 rows located via `csv.DictReader`, filtered `week==6`),
cross-checked against each function's own `04_Results/observations.csv`
(final row) and `04_Results/summary.json`, and against a third,
independent raw-text source outside the source git repo
(`~/Downloads/Capstone_Folder/week_06_data/`), with numeric (not merely
textual) equality checks against this rebuild project's
`historical_query.txt`/`historical_output.txt` files.

## Per-Function Results

| Function | d | Ledger match | observations.csv/summary.json match | 3rd-source match | Bounds | Finite | duplicate_of | Result |
|---|---|---|---|---|---|---|---|---|
| Function_01 | 2 | PASS | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_02 | 2 | PASS | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_03 | 3 | PASS | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_04 | 4 | PASS | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_05 | 4 | PASS | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_06 | 5 | PASS | PASS | PASS (formatting-normalised) | PASS | PASS | empty | **PASS** |
| Function_07 | 6 | PASS | PASS | PASS (formatting-normalised) | PASS | PASS | empty | **PASS** |
| Function_08 | 8 | PASS | PASS | PASS (formatting-normalised) | PASS | PASS | empty | **PASS** |

All 8 functions' `historical_query.txt`/`historical_output.txt` values
were confirmed to numerically equal (via `float()` comparison, not string
comparison) the independently re-extracted source ledger row, corroborated
by a third, independent raw-text source outside the source git repo.

## Function_05 Duplicate-Relationship Check — Confirmed Distinct

Independently pulled all 12 of Function_05's ledger rows directly:
- **Week 4**: `[0.0,0.0,0.0,0.0] → 163.1225`, `duplicate_of` empty (the
  documented original occurrence, later duplicated by Week 7).
- **Week 6**: `[0.654981,0.602894,0.898891,0.589622] → 281.9115728306016`,
  `duplicate_of` empty.
- **Week 7**: `[0.0,0.0,0.0,0.0] → 163.1225`, `duplicate_of = 'week:4'`.

A full-ledger scan (all 96 data rows) found exactly **one** populated
`duplicate_of` value in the entire ledger — confirming the Week 6 pair is
completely unrelated to the documented Week_04/Week_07 duplicate.

## Full-Ledger Duplicate Scan

Each of the 8 Week 6 query strings was searched across its own function's
full row history; each matched only its own Week 6 row. **No other week
duplicates any of these 8 Week 6 pairs.**

## "Proposal Only" Check — None Found, With Clarification

All 96 ledger rows carry `evidence_status` of either
`verified_cumulative_archive_pair` or
`verified_cumulative_archive_pair_after_query_record_reconciliation` — no
row anywhere carries a proposal-only/unavailable status.

The `"status": "proposed_not_evaluated"` text found in the source
project's Week_06 notebooks was independently confirmed to describe each
notebook's own **next-week (Week 7) acquisition proposal** (via a UCB
computation over Sobol candidates), not the Week 6 return itself —
structurally identical to the pattern already documented for Week 5, and
correctly does not affect the fully-verified status of Week 6's own
return.

## Formatting Normalisation Checks

All five claimed normalisations independently confirmed value-preserving:
`0.78177 == 0.781770`, `0.56594 == 0.565940` (Function_06);
`0.20709 == 0.207090` (Function_07); `0.44026 == 0.440260`,
`0.96555 == 0.965550` (Function_08) — all trailing-zero padding only.

## VERIFIED_PAIRS.csv Agreement

Confirmed row-by-row against each function's `historical_query.txt`/
`historical_output.txt` — all 8 rows match exactly, including all five
normalised coordinates.

## Checksum Spot-Check

SHA-256 independently recomputed for all 4 cited source files
(ledger, observations.csv, summary.json, provenance.json) for Functions
05 and 06 — all match the values recorded in their `pair_provenance.md`
exactly.

## Scope-Boundary Confirmation

- No `Week_07` directory exists anywhere in the project.
- No `03_Queries` directory exists anywhere under `Historical_Replay/Week_06/`.
- No cumulative dataset was appended: each function's
  `02_Data/cumulative_inputs.npy` row count is exactly one row short of
  the full Week 6 total in the source `summary.json`, confirming the
  historical pair has not yet been merged in.
- All 8 GP diagnostics notebooks grepped for each function's historical
  output value — zero occurrences in every case, confirming no leakage.
- `git status` on the source project: clean, confirming no modification.
- `git status`/`git diff --stat` on the rebuild project itself: the only
  change anywhere in the working tree is the new, untracked
  `Historical_Replay/Week_06/` directory — no Week_01–05 file changed.

## Open Items Noted by the Reviewer (Not Failures)

- Checksums were independently recomputed for only 2 of 8 functions
  (Function_05, Function_06); the other 6 were verified via the
  ledger/observations.csv/summary.json/third-source cross-check, the
  higher-value check for data-integrity purposes.
- Only the source project's current working-tree status was checked, not
  its full commit history.
- The Function_05 Week 9 out-of-bounds coordinate (exactly `1.0`) flagged
  in `HISTORICAL_IMPORT_VALIDATION.md` was independently confirmed
  present in the ledger during this review, and is correctly scoped as a
  future (Week 9) issue with no bearing on this Week 6 import.

None of these affect the verdict below.

## Final Verdict

# **PASS — Week_06 historical pair import independently re-verified, 8/8 functions, all 10 checks PASS**

No numeric discrepancy, no leakage into cumulative datasets, no
fabricated value, no populated `duplicate_of` outside the already-documented
Week_04/Week_07 Function_05 relationship, no duplicate Week 6 query
anywhere else in the ledger, no genuine "proposal only" marker for Week 6,
and no unauthorized modification to the source project or to this
rebuild project's prior weeks was found.
