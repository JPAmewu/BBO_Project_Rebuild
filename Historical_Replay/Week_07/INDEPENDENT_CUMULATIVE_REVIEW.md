# Independent Cumulative Review — Week_07

**Scope:** Independent re-verification, by the ML Reviewer Agent, of the
Week_07 cumulative-dataset append documented in
`CUMULATIVE_DATA_VALIDATION.md`. Every value was re-derived from scratch
by loading the actual `.npy`/`.txt` files directly — no claim from this
project's own prior documentation was trusted without independent
re-derivation.

## Per-Function Results

| Function | Rows (Week_06 → Week_07) | Dim | Prefix identical | New row = historical_query.txt | New output = historical_output.txt | Bounds [0,0.999999] | Finite | No duplicate rows | Result |
|---|---|---|---|---|---|---|---|---|---|
| Function_01 | 15 → 16 | 2 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_02 | 15 → 16 | 2 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_03 | 20 → 21 | 3 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_04 | 35 → 36 | 4 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_05 | 25 → 26 | 4 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_06 | 25 → 26 | 5 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_07 | 35 → 36 | 6 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_08 | 45 → 46 | 8 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |

Prefix-equality was verified for both inputs and outputs — byte-for-byte
equal, confirming no historical row was altered and only one row was
appended, for all 8 functions.

## Function_05 Rule 9 Cross-Check

Independently confirmed the appended Week_06 pair
(`[0.654981, 0.602894, 0.898891, 0.589622] → 281.9115728306016`) is
distinct from the `[0,0,0,0] → 163.1225` duplicate pair (present at both
`Week_04/Function_05` and referenced for Week_07's own, separate,
not-yet-imported historical pair in `HISTORICAL_REPLAY_PROTOCOL.md`
Rule 9). This append is correctly unaffected by that future duplicate
requirement, confirmed with no contradiction found. Also confirmed
Function_05's existing minimum coordinate of exactly `0.0` (a pre-existing
Week_06 value) is a permitted, inclusive-bound value, not a data-quality
issue.

## Checksum Verification

- All 16 Week_07 `.npy` files: SHA-256 independently recomputed and
  matched exactly against `CUMULATIVE_DATA_VALIDATION.md`. **PASS.**
- All 16 Week_06 `.npy` files: SHA-256 independently recomputed and
  matched exactly against both Week_06's own `CUMULATIVE_DATA_VALIDATION.md`
  and the "unchanged" column reproduced in the Week_07 doc — confirms the
  Week_07 append did not touch Week_06's files. **PASS.**

Zero mismatches across all 32 independently-recomputed hash comparisons.

## Scope-Boundary Confirmation

- `Historical_Replay/Week_07` contains exactly 17 files: the one
  `CUMULATIVE_DATA_VALIDATION.md` plus 2 `.npy` files per function × 8
  functions — no notebook, code review, or other subfolder exists yet.
- No file in `Historical_Replay/Week_01` through `Week_06` was modified
  after this operation.
- No `Week_08` directory exists anywhere.
- `~/Documents/GitHub/My_Capstone_1_Imperial` confirmed unmodified (clean
  working tree, read-only check only).

## Open Items Noted by the Reviewer (Not Failures)

- Provenance of the underlying Week_06 `historical_query.txt`/
  `historical_output.txt` values themselves was not re-audited here —
  that chain of custody was already independently covered by Week_06's
  own `HISTORICAL_IMPORT_VALIDATION.md`/`INDEPENDENT_HISTORICAL_REVIEW.md`;
  this review's scope was specifically the Week_06→Week_07 append.
- The reviewer did not execute `build_week07_cumulative.py` itself; it
  verified the resulting files independently from first principles, which
  is the stronger check.
- File mtimes were used only as a soft, corroborating signal; SHA-256
  checksum matches are the authoritative evidence and fully agree with
  the mtime spot-checks.

Neither item affects the verdict below.

## Final Verdict

# **PASS — Week_07 cumulative-dataset append independently re-verified from scratch, 8/8 functions, no discrepancies found**
