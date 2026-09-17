# Independent Cumulative Review — Week_05

**Scope:** Independent re-verification, by the ML Reviewer Agent, of the
Week_05 cumulative-dataset append documented in
`CUMULATIVE_DATA_VALIDATION.md`. Every value was re-derived from scratch
by loading the actual `.npy`/`.txt` files directly — no claim from this
project's own prior documentation was trusted without independent
re-derivation.

## Per-Function Results

| Function | Rows (Week_04 → Week_05) | Dim | Prefix identical | New row = historical_query.txt | New output = historical_output.txt | Bounds [0,0.999999] | Finite | No duplicate rows | Result |
|---|---|---|---|---|---|---|---|---|---|
| Function_01 | 13 → 14 | 2 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_02 | 13 → 14 | 2 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_03 | 18 → 19 | 3 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_04 | 33 → 34 | 4 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_05 | 23 → 24 | 4 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_06 | 23 → 24 | 5 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_07 | 33 → 34 | 6 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| Function_08 | 43 → 44 | 8 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |

Prefix-equality was checked via `np.array_equal(week5_array[:N], week4_array)`
for both inputs and outputs — byte-for-byte equal, confirming no historical
row was altered and only one row was appended, for all 8 functions. The new
row/output was confirmed to exactly match (not merely close to) the parsed
contents of that function's `historical_query.txt`/`historical_output.txt`.

**Function_05 special check:** independently re-derived that the prior 23
rows have a minimum coordinate of `0.03819337135150802`, confirming the
new all-zero row (`[0,0,0,0]`) is genuinely non-duplicate — matches the
value already stated in `CUMULATIVE_DATA_VALIDATION.md`.

## Checksum Verification

- All 16 Week_05 `.npy` files (8 functions × inputs/outputs): SHA-256
  independently recomputed and matched exactly against
  `CUMULATIVE_DATA_VALIDATION.md`. **PASS.**
- All 8 Week_04 `.npy` files referenced as "unchanged": SHA-256
  independently recomputed and matched exactly against
  `Historical_Replay/Week_04/CUMULATIVE_DATA_VALIDATION.md`'s original
  build-time hashes — confirms the Week_05 append did not touch Week_04's
  files. **PASS.**

## Scope-Boundary Confirmation

- `git status` in the rebuild project confirms the **only** change since
  the last commit (which already contains Week_01–Week_04) is the new,
  entirely untracked `Historical_Replay/Week_05/` directory — strong
  independent confirmation that no Week_01/02/03/04 file was modified.
- Each `Historical_Replay/Week_05/Function_0X/` directory was confirmed to
  contain only `02_Data/` at this stage — no notebook, figures, or code
  review yet, as expected for this step.
- No `Week_06` directory exists anywhere under `Historical_Replay`.
- `~/Documents/GitHub/My_Capstone_1_Imperial` confirmed unmodified (clean
  working tree, read-only check only).

## Open Items Noted by the Reviewer (Not Failures)

- The reviewer did not re-run `build_week05_cumulative.py` itself (a
  scratchpad script, not part of the project); it instead verified the
  resulting files independently from first principles, which is the
  stronger check.
- No exhaustive byte-diff of every Week_01–03 file against a
  pre-operation baseline was performed beyond what a clean `git status`
  already guarantees for tracked files — a theoretical blind spot for an
  untracked stray file, though none was found.

Neither item affects the verdict below.

## Final Verdict

# **PASS — Week_05 cumulative-dataset append independently re-verified from scratch, 8/8 functions, no discrepancies found**
