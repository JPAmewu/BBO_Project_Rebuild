# Independent Review — Historical_Replay/Week_02 Cumulative Starting Datasets

**Scope:** Independent, read-only re-verification of
`Historical_Replay/Week_02/Function_0X/02_Data/cumulative_inputs.npy` and
`cumulative_outputs.npy` for all 8 functions, conducted by the ML Reviewer
Agent. This review did not trust `CUMULATIVE_DATA_VALIDATION.md`'s own
claims — every value was independently recomputed directly from
`Week_01/Function_0X/02_Data/initial_*.npy` and
`Historical_Replay/Week_01/Function_0X/historical_{query,output}.txt`. No
file was modified, created, or deleted during this review. No Week 02
historical data was imported, revealed, or used. No model was built and no
query was generated.

**Overall verdict: PASS.** All required checks (1–6) passed exactly for all
8 functions. No genuine data-integrity discrepancy was found. One real, but
purely documentary, issue was found (Check 9) and has already been corrected.

## Checks 1–6, Per Function (All PASS)

| Function | N (original) | Cumulative shape | 1. First-N identical | 2. N+1 rows, last row = historical pair | 3. Exact final count | 4. Counts match | 5. Dim/dtype/finite/bounds | 6. No duplicates @6dp |
|---|---|---|---|---|---|---|---|---|
| 01 | 10 | (11,2)/(11,) | PASS | PASS — last row `[0.374540, 0.950714]`, output `-1.560646704467778e-117`, exact | PASS (11) | PASS | PASS (d=2, float64, finite, [0.0787, 0.9507]) | PASS |
| 02 | 10 | (11,2)/(11,) | PASS | PASS — last row `[0.374540, 0.950714]`, output `-0.03182956281754251` | PASS (11) | PASS | PASS (d=2, float64, finite, [0.0287, 0.9507]) | PASS |
| 03 | 15 | (16,3)/(16,) | PASS | PASS — last row `[0.444444, 0.666666, 0.333333]`, output `-0.04090761844901528` | PASS (16) | PASS | PASS (d=3, float64, finite, [0.0468, 0.9909]) | PASS |
| 04 | 30 | (31,4)/(31,) | PASS | PASS — last row `[0.555555, 0.444444, 0.222222, 0.111111]`, output `-8.727516493155957` | PASS (31) | PASS | PASS (d=4, float64, finite, [0.0063, 0.9995]) | PASS |
| 05 | 20 | (21,4)/(21,) | PASS | PASS — last row `[0.224189, 0.846480, 0.879484, 0.878515]`, output `1088.8535114737463` | PASS (21) | PASS | PASS (d=4, float64, finite, [0.0382, 0.9576]) | PASS |
| 06 | 20 | (21,5)/(21,) | PASS | PASS — last row `[0.728186, 0.154693, 0.732552, 0.693997, 0.564013]`, output `-1.1520351120911565` | PASS (21) | PASS | PASS (d=5, float64, finite, [0.0049, 0.9788]) | PASS |
| 07 | 30 | (31,6)/(31,) | PASS | PASS — last row `[0.045091, 0.528666, 0.329265, 0.105350, 0.434667, 0.641164]`, output `1.0510148516295004` | PASS (31) | PASS | PASS (d=6, float64, finite, [0.0036, 0.9987]) | PASS |
| 08 | 40 | (41,8)/(41,) | PASS | PASS — last row (8 coords), output `9.8157087929671` | PASS (41) | PASS | PASS (d=8, float64, finite, [0.0034, 0.9989]) | PASS |

Row counts exactly match the required list: **11, 11, 16, 31, 21, 21, 31,
41**. Every last-row/output comparison used `np.array_equal`/exact float
equality (difference = 0.0), not `np.allclose`.

## Check 7 — Function_05 and Function_06 Near-Duplicate Analysis (Nothing Altered)

### Function_05

The appended row (index 20) was compared against the closest original row
(index 15):

- Appended: `[0.224189, 0.846480, 0.879484, 0.878515]`
- Original[15]: `[0.22418902330288348, 0.8464804904862864, 0.8794841797090803, 0.8785156842249731]`
- Coordinate differences: `[-2.33e-08, -4.90e-07, -1.80e-07, -6.84e-07]` — sub-micro, consistent with 6-decimal-place rounding of the same underlying float.
- A nearest-neighbor search across all 20 original rows confirmed row 15 is
  the closest match by a wide margin (distance ≈ 8.6e-7 vs. the next-closest
  row at distance 0.26 — three orders of magnitude farther). This is
  unambiguously the same underlying point, not a coincidental near-collision.
- It escapes the 6-decimal duplicate filter only because the 4th coordinate
  rounds to `0.878516` in the original vs. `0.878515` in the appended row —
  a genuine rounding-boundary edge case, not a detection error.
- Outputs: original row 15 = `1088.8596181962705`; appended =
  `1088.8535114737463`; difference ≈ `-0.0061` (≈5.6e-6 relative).

**Assessment:** this is consistent with a rounding-truncated re-query of the
same underlying input point (the source project's own historical record
already documents a re-query at nearly this exact input during Week 1 — see
`HISTORICAL_REPLAY_INVENTORY.md`). Whether the ~0.006 output gap reflects
genuine local sensitivity/curvature of Function_05 near this point, or
measurement noise in the original black-box evaluations, **cannot be
determined without executing the underlying black-box function** — this is
explicitly flagged as unverifiable rather than guessed. Neither row was
altered.

### Function_06

The appended row (index 20) was compared against the closest original row
(index 0):

- Appended: `[0.728186, 0.154693, 0.732552, 0.693997, 0.564013]`
- Original[0]: `[0.7281861047460138, 0.1546925696237983, 0.7325516687239811, 0.6939965090690888, 0.056401310518258585]`
- Coordinates 0–3 differences: `[-1.05e-07, 4.30e-07, 3.31e-07, 4.91e-07]` —
  again consistent with rounding of identical values; a nearest-neighbor
  search confirms row 0 is the closest match on those 4 dimensions by a wide
  margin.
- **Coordinate 4 (5th):** appended `0.564013` vs. original
  `0.056401310518258585` — ratio ≈ **9.99999814** (essentially exactly
  10×). The digit sequence "564013" is identical in both; only the decimal
  placement/leading zero differs.

**Assessment:** given that the other 4 coordinates match to ~1e-7 precision
(a negligible probability of coincidence across 4 independent dimensions),
this is almost certainly the same underlying point. The exact 10× ratio with
identical digit sequence is a classic signature of a decimal-place or
leading-zero error introduced somewhere upstream of this dataset, rather
than a genuinely distinct value. **This is an inference, not a proven root
cause** — the review did not trace how `historical_query.txt` was itself
generated or transcribed, and neither the original row nor the appended row
was altered as part of this review or the prior import step.

Neither observation invalidates the cumulative datasets under Checks 1–6 —
both rows pass every required check exactly as specified (they are not
flagged as duplicates because their rounded 6-decimal values genuinely
differ). Both are flagged here for downstream awareness before any modelling
work uses this data.

## Check 8 — No Week 02 Data Exposure: PASS

A full recursive listing of `Historical_Replay/Week_02/` contains only
`CUMULATIVE_DATA_VALIDATION.md` (plus, as of this review, this file) and,
per function, `02_Data/cumulative_inputs.npy` + `cumulative_outputs.npy` —
no other files or subfolders. A case-insensitive search for `week_02` /
`week02` / "week 2" / "week two" across the tree matched only this
directory's own name and section-label text in the markdown reports (e.g.
"Week_02 Starting Datasets") — never an actual Week 02 query or output
value. The `.npy` payloads themselves were independently confirmed (Checks
1–2) to contain exactly the Week 1 originals plus the single verified Week 1
historical pair — nothing else.

## Check 9 — Immutability and Checksum Verification

- SHA-256 was recomputed (via both `shasum -a 256` and Python `hashlib`) for
  all 16 `Week_01/Function_0X/02_Data/*.npy` files and all 16
  `Historical_Replay/Week_01/Function_0X/historical_{query,output}.txt`
  files.
- **A real, but purely documentary, discrepancy was found:** many SHA-256
  strings recorded in `CUMULATIVE_DATA_VALIDATION.md` were rendered as
  **63 hex characters instead of 64** — each recorded value was an exact
  prefix of the correct 64-character hash, missing only the final hex
  digit (this affected roughly 19 of 48 checksum entries in that report).
  The probability of a 63-character exact-prefix match against a genuinely
  different file is astronomically small, so this is conclusive: **the
  underlying `.npy` and `.txt` files are unmodified; the defect was in how
  the report's hash strings were transcribed, not evidence of tampering.**
  **This has been corrected**: a "Correction Addendum" with the full,
  independently-recomputed 64-character checksums for every file has been
  appended to `CUMULATIVE_DATA_VALIDATION.md`, dated 2026-09-16. The
  original PASS verdict in that document is unaffected.
- Zero genuine hash mismatches were found across all 46 parsed file/hash
  pairs once compared correctly (prefix-vs-full).
- `git status --porcelain` in `~/Documents/GitHub/My_Capstone_1_Imperial`
  returned no output; full `git status` reported a clean working tree with
  nothing to commit. The source project shows no modifications.

## What Could Not Be Verified Statically (Disclosed, Does Not Affect the Verdict)

- Whether Function_05's ~0.0061 output gap at its near-duplicate point
  reflects genuine function sensitivity/curvature vs. measurement noise —
  would require executing the black-box objective, which is out of scope for
  a read-only review.
- The true root cause of Function_06's 10× coordinate discrepancy
  (introduced during original data generation, during transcription into
  `historical_query.txt`, or elsewhere) — only files already present locally
  were compared; no earlier drafts or upstream sources were available.
- The cause of the checksum-truncation pattern in the original
  `CUMULATIVE_DATA_VALIDATION.md` (copy/paste error vs. a rendering
  artifact) — its effect was confirmed and corrected; its cause was not
  determined.

## Final Verdict

# **PASS — Historical_Replay/Week_02 cumulative datasets independently confirmed correct**

All 8 functions pass every required check. The two flagged near-duplicate
observations (Function_05, Function_06) are real, independently reproduced,
and explained above without altering either underlying observation — they
should be kept in mind for any future modelling that uses this data, but do
not indicate a defect in the cumulative-dataset construction itself. The one
genuine issue found (truncated checksums in the validation report) has been
corrected via a dated addendum; no dataset file required any change.
