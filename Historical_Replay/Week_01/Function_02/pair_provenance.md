# Pair Provenance — Function_02, Historical Week_01

**Status: VERIFIED**

## Imported Values
- Query (as stored, full precision): `[0.37454, 0.950714]`
- Query (formatted to official 6-decimal submission convention, value
  unchanged): `0.374540-0.950714`
- Output (exact, full precision as stored): `-0.03182956281754251`
- Dimensionality: 2 (matches expected d=2 for Function_02)

## Source Files and Checksums (source project, read-only, unmodified)

All paths relative to `~/Documents/GitHub/My_Capstone_1_Imperial`.

| File | Role | SHA-256 |
|---|---|---|
| `Results/query_output_ledger.csv` | Canonical registry (v1.2), row: Week 1, Function 2 | `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d` |
| `Week_01/Function_02/04_Results/observations.csv` | Row 11: `11,recorded,0.37454,0.950714,-0.03182956281754251` | `a9a41658c0de26ff6534ebb019f912e550763673aea0992d167026a30fadf6cf` |
| `Week_01/Function_02/04_Results/summary.json` | `latest_verified_input: [0.37454, 0.950714]`, `latest_verified_output: -0.03182956281754251`, `dimensions: 2` | `3fa256e38b1a59f8018454d9a8c7846f487e25b72cca98242949d55b2ea26e66` |
| `Week_01/Function_02/03_Data/provenance.json` | Names `Results/query_output_ledger.csv` as `recorded_pairs_registry` | `351a2ffcb67cda559a757b2591ebec37e433f6b3d7d0cd90488552a23d7e35bd` |

## Cross-Checks Performed

- **Ledger vs. per-function records**: exact match against `observations.csv`
  row 11 and `summary.json`. **Agreement: YES.**
- **Dimensionality**: 2 coordinates, matches expected d=2. **PASS.**
- **Bounds** ([0.000000, 0.999999]): min=0.37454, max=0.950714. **PASS.**
- **Six-decimal-place consistency**: round-trip via `round(x*1e6)/1e6`
  reproduces both coordinates exactly. **PASS.**
- **Finite scalar output**: single finite float. **PASS.**
- **Ledger file integrity**: SHA-256 matches `Results/query_output_ledger.sha256`
  and the canonical v1.2 entry in `Results/query_output_ledger_versions.json`.
  **CONFIRMED unaltered.**

## Not Independently Verified (Out of Scope, Does Not Affect VERIFIED Status)

- Upstream external archive files (`Capstone_Folder/initial_data/*`) referenced
  by the ledger's `source_input`/`source_output` columns were not located/
  re-verified. VERIFIED status rests on the directly-confirmed agreement
  between the ledger and this function's own primary records.
- The notebook `Week_01/02_Notebook/Week_1_Capstone.ipynb` was not opened or
  executed.

## No Source Changes

No file in the source project was created, modified, renamed, or deleted as
part of this import.
