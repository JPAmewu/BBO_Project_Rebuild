# Independent Historical Review — Week_07 Genuine Historical Pair Import

**Scope:** Independent re-verification, by the ML Reviewer Agent, of the
eight genuine historical Week 7 query-output pairs imported into
`Historical_Replay/Week_07/Function_0X/` and documented in
`HISTORICAL_IMPORT_VALIDATION.md` and `VERIFIED_PAIRS.csv`. This mirrors
the same independent-review step already completed for the Week 1–6
imports, with particular emphasis this week on Function_05's confirmed
duplicate-of-Week_04 relationship. No claim in this project's own
documentation was trusted without independent re-derivation — the prior
documentation was read only *after* the independent checks below were
completed, and compared against them afterward.

**Method:** every value was independently re-extracted from
`~/Documents/GitHub/My_Capstone_1_Imperial/Results/query_output_ledger.csv`
(all 96 rows parsed via `csv.DictReader`, filtered `week==7`), cross-checked
against each function's own `04_Results/observations.csv` and
`04_Results/summary.json`, and against a third, independent raw-text
source outside the source git repo
(`~/Downloads/Capstone_Folder/week_07_data/`), with numeric equality
checks against this rebuild project's `historical_query.txt`/
`historical_output.txt` files.

## Per-Function Results

| Function | d | Ledger match | observations.csv/summary.json match | 3rd-source match | Bounds | Finite | duplicate_of | Result |
|---|---|---|---|---|---|---|---|---|
| Function_01 | 2 | PASS | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_02 | 2 | PASS | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_03 | 3 | PASS | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_04 | 4 | PASS | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_05 | 4 | PASS | PASS | PASS | PASS | PASS | **week:4 — CONFIRMED** | **PASS (documented, not appended)** |
| Function_06 | 5 | PASS | PASS | PASS (formatting-normalised) | PASS | PASS | empty | **PASS** |
| Function_07 | 6 | PASS | PASS | PASS | PASS | PASS | empty | **PASS** |
| Function_08 | 8 | PASS | PASS | PASS (formatting-normalised) | PASS | PASS | empty | **PASS** |

All 8 functions' `historical_query.txt`/`historical_output.txt` values
were confirmed to numerically equal the independently re-extracted
source ledger row.

## Function_05 Duplicate Claim — Confirmed Airtight

- **Week 4 ledger row**: query `[0.0,0.0,0.0,0.0]`, output `163.1225`,
  `duplicate_of` = **empty** (the confirmed, undisputed original).
- **Week 7 ledger row**: query `[0.0,0.0,0.0,0.0]`, output `163.1225`,
  `duplicate_of` = **`week:4`** exactly, read directly from the raw
  ledger field, not summarized.
- A full-ledger scan across all 96 rows found **exactly one** non-empty
  `duplicate_of` value anywhere — this Week_07/Function_05 row. No other
  function or week carries any duplicate flag.
- Cross-checked against a genuine third, independent source outside both
  git repos (`~/Downloads/Capstone_Folder/week_07_data/`) — the identical
  `[0,0,0,0] → 163.1225` value appears at both the Week 4 and Week 7
  positions, corroborating this is a real feature of the original
  historical data, not an artifact introduced anywhere in this project.
- `pair_provenance.md`'s distinct duplicate-handling banner and
  `VERIFIED_PAIRS.csv`'s `VERIFIED_DUPLICATE_NOT_APPENDED` status
  (vs. the other seven functions' plain `VERIFIED`) were confirmed
  present exactly as documented.

## Cumulative Dataset Integrity — Confirmed Untouched by This Import

Loaded every function's `02_Data/cumulative_inputs.npy`/`cumulative_outputs.npy`
directly and inspected the final row: for all 8 functions, the last row
is Week_06's recorded value (built earlier in this week's cumulative-append
step), **not** any of the newly-imported Week_07 pairs. Function_05's
array specifically ends in Week_06's pair
(`[0.654981, 0.602894, 0.898891, 0.589622] → 281.91...`), confirming its
Week_07 duplicate pair is genuinely absent, consistent with "will never
be appended." Mtime ordering independently corroborates this: all
`cumulative_*.npy` files predate every `historical_query.txt`/
`historical_output.txt`/`pair_provenance.md` file created by this import.

## Formatting Normalisation Checks

All five claimed normalisations independently confirmed value-preserving:
`0.35042 == 0.350420` (Function_01), `0.66981 == 0.669810` (Function_03),
`0.68383 == 0.683830` and `0.62931 == 0.629310` (Function_06),
`0.26353 == 0.263530` (Function_08) — all trailing-zero padding only.

## Documentation-Inconsistency Check — Confirmed a Copy-Paste Artifact, Not a Genuine Dispute

`Week_07/01_Queries/README.md`'s "unavailable-return evidence gap"
boilerplate was independently confirmed to also appear verbatim in
`Week_05/01_Queries/README.md` — a copy-paste template artifact, not
week-specific language. Contrasted directly against the genuine,
unambiguous non-evidence markers present in Weeks 11-13's own READMEs
("no verified Week 12 returned outputs are preserved," etc.) — Week 7's
phrasing is structurally different and contradicted by the ledger's own
`evidence_status` field (all 8 Week 7 rows: `verified_cumulative_archive_pair`).
Confirmed as a documentation artifact, not a real dispute.

## VERIFIED_PAIRS.csv Agreement

Confirmed row-by-row against each function's `historical_query.txt`/
`historical_output.txt` — all 8 rows match exactly, with Function_05's
row correctly distinguished as `VERIFIED_DUPLICATE_NOT_APPENDED`.

## Scope-Boundary Confirmation

- No `Week_08` directory exists anywhere in the project.
- No Week_01-06 file, in either this project or the source project, was
  modified (source project `git status`: clean).
- No notebook was touched by this import step (notebook mtimes predate
  the import).

## Open Items Noted by the Reviewer (Not Failures)

- The ultimate provenance of the third-source Downloads copy itself
  (i.e., how it was originally produced) was not independently
  auditable — only its internal consistency with the ledger and rebuild
  files was confirmed.
- The GP diagnostics notebooks were not executed to confirm non-involvement;
  mtime ordering was used as strong, non-conclusive corroborating evidence.

Neither item affects the verdict below.

## Final Verdict

# **PASS — Week_07 historical pair import independently re-verified, 8/8 functions, all checks PASS. Function_05's duplicate-of-Week_04 status is confirmed airtight, and the "documented but never appended" handling is confirmed genuinely in effect (no cumulative dataset touched by this import).**
