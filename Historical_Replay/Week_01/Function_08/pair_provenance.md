# Pair Provenance — Function_08, Historical Week_01

**Status: VERIFIED**

## Imported Values
- Query (as stored, full precision): `[0.273673, 0.2604, 0.073937, 0.078562, 0.862321, 0.230729, 0.108688, 0.352588]`
- Query (formatted to official 6-decimal submission convention, value
  unchanged): `0.273673-0.260400-0.073937-0.078562-0.862321-0.230729-0.108688-0.352588`
- Output (exact, full precision as stored): `9.8157087929671`
- Dimensionality: 8 (matches expected d=8 for Function_08)

## Source Files and Checksums (source project, read-only, unmodified)

All paths relative to `~/Documents/GitHub/My_Capstone_1_Imperial`.

| File | Role | SHA-256 |
|---|---|---|
| `Results/query_output_ledger.csv` | Canonical registry (v1.2), row: Week 1, Function 8 | `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d` |
| `Week_01/Function_08/04_Results/observations.csv` | Row 41: `41,recorded,0.273673,0.2604,0.073937,0.078562,0.862321,0.230729,0.108688,0.352588,9.8157087929671` | `c4b7dfd0fabc168df6bf182b87f445711e5ac3493bfe3ba900710ead210a8d49` |
| `Week_01/Function_08/04_Results/summary.json` | `latest_verified_input` matches; `latest_verified_output: 9.8157087929671`, `dimensions: 8`; also `best_query: 41` (this Week-1 point is currently F8's best recorded query) | `0ae6e7fa6b01d5ac2a216947343272f901dd68169647b4b2179b32154a1047f3` |
| `Week_01/Function_08/03_Data/provenance.json` | Names `Results/query_output_ledger.csv` as `recorded_pairs_registry` | `12dec29ad9cf60c458e2b288dc7794a5fe1df0589aa21c610f185b890fd7c16b` |

## Cross-Checks Performed

- **Ledger vs. per-function records**: exact match against `observations.csv`
  row 41 and `summary.json`. **Agreement: YES.**
- **Dimensionality**: 8 coordinates, matches expected d=8. **PASS.**
- **Bounds** ([0.000000, 0.999999]): min=0.073937, max=0.862321. **PASS.**
- **Six-decimal-place consistency**: round-trip via `round(x*1e6)/1e6`
  reproduces all 8 coordinates exactly (`0.2604` is the same value as
  `0.260400` with dropped trailing zeros). **PASS.**
- **Finite scalar output**: single finite float. **PASS.**
- **Ledger file integrity**: SHA-256 matches `Results/query_output_ledger.sha256`
  and the canonical v1.2 entry in `Results/query_output_ledger_versions.json`.
  **CONFIRMED unaltered.**

## Not Independently Verified (Out of Scope, Does Not Affect VERIFIED Status)

- Upstream external archive files (`Capstone_Folder/initial_data/*`) were not
  located/re-verified. VERIFIED status rests on the directly-confirmed
  agreement between the ledger and this function's own primary records.
- The notebook `Week_01/02_Notebook/Week_1_Capstone.ipynb` was not opened or
  executed.

## No Source Changes

No file in the source project was created, modified, renamed, or deleted as
part of this import.
