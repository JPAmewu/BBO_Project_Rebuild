# Pair Provenance — Function_07, Historical Week_01

**Status: VERIFIED**

## Imported Values
- Query (as stored, full precision): `[0.045091, 0.528666, 0.329265, 0.10535, 0.434667, 0.641164]`
- Query (formatted to official 6-decimal submission convention, value
  unchanged): `0.045091-0.528666-0.329265-0.105350-0.434667-0.641164`
- Output (exact, full precision as stored): `1.0510148516295004`
- Dimensionality: 6 (matches expected d=6 for Function_07)

## Source Files and Checksums (source project, read-only, unmodified)

All paths relative to `~/Documents/GitHub/My_Capstone_1_Imperial`.

| File | Role | SHA-256 |
|---|---|---|
| `Results/query_output_ledger.csv` | Canonical registry (v1.2), row: Week 1, Function 7 | `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d` |
| `Week_01/Function_07/04_Results/observations.csv` | Row 31: `31,recorded,0.045091,0.528666,0.329265,0.10535,0.434667,0.641164,1.0510148516295004` | `01dd56eb22817e44c488170e5f68f531c1ae00e6baa55730f9f5fdd1f52e8b9e` |
| `Week_01/Function_07/04_Results/summary.json` | `latest_verified_input: [0.045091, 0.528666, 0.329265, 0.10535, 0.434667, 0.641164]`, `latest_verified_output: 1.0510148516295004`, `dimensions: 6` | `8a0dcdde1c5712c724c30905dbe0eb420a2712ac1fef38683af13085b5a16e07` |
| `Week_01/Function_07/03_Data/provenance.json` | Names `Results/query_output_ledger.csv` as `recorded_pairs_registry` | `703f07bfec298b47ee5e6a6d8733cf987b9424968abd46ba9ec83a06b923d0bb` |

## Cross-Checks Performed

- **Ledger vs. per-function records**: exact match against `observations.csv`
  row 31 and `summary.json`. **Agreement: YES.**
- **Dimensionality**: 6 coordinates, matches expected d=6. **PASS.**
- **Bounds** ([0.000000, 0.999999]): min=0.045091, max=0.641164. **PASS.**
- **Six-decimal-place consistency**: round-trip via `round(x*1e6)/1e6`
  reproduces all 6 coordinates exactly (`0.10535` is the same value as
  `0.105350` with a dropped trailing zero). **PASS.**
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
