# Pair Provenance — Function_03, Historical Week_01

**Status: VERIFIED**

## Imported Values
- Query (as stored, full precision): `[0.444444, 0.666666, 0.333333]`
- Query (formatted to official 6-decimal submission convention, value
  unchanged): `0.444444-0.666666-0.333333`
- Output (exact, full precision as stored): `-0.04090761844901528`
- Dimensionality: 3 (matches expected d=3 for Function_03)

## Source Files and Checksums (source project, read-only, unmodified)

All paths relative to `~/Documents/GitHub/My_Capstone_1_Imperial`.

| File | Role | SHA-256 |
|---|---|---|
| `Results/query_output_ledger.csv` | Canonical registry (v1.2), row: Week 1, Function 3 | `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d` |
| `Week_01/Function_03/04_Results/observations.csv` | Row 16: `16,recorded,0.444444,0.666666,0.333333,-0.04090761844901528` | `78d3fd99d7de8c20d7101cf0d9fd3381fdb141967b70af8ed3f5cdad628138d4` |
| `Week_01/Function_03/04_Results/summary.json` | `latest_verified_input: [0.444444, 0.666666, 0.333333]`, `latest_verified_output: -0.04090761844901528`, `dimensions: 3` | `6e6582b7f2d4fe5a8e18e99626c281096d42d8b82675035547802ce196d11ea7` |
| `Week_01/Function_03/03_Data/provenance.json` | Names `Results/query_output_ledger.csv` as `recorded_pairs_registry` | `be19222d1acbb8be56c3d725646bfccce0b58bf9364b8d1f28ba64339e64b67d` |

## Cross-Checks Performed

- **Ledger vs. per-function records**: exact match against `observations.csv`
  row 16 and `summary.json`. **Agreement: YES.**
- **Dimensionality**: 3 coordinates, matches expected d=3. **PASS.**
- **Bounds** ([0.000000, 0.999999]): min=0.333333, max=0.666666. **PASS.**
- **Six-decimal-place consistency**: round-trip via `round(x*1e6)/1e6`
  reproduces all 3 coordinates exactly. **PASS.**
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
