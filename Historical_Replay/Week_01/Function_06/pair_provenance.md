# Pair Provenance — Function_06, Historical Week_01

**Status: VERIFIED**

## Imported Values
- Query (as stored, full precision): `[0.728186, 0.154693, 0.732552, 0.693997, 0.564013]`
- Query (formatted to official 6-decimal submission convention, value
  unchanged): `0.728186-0.154693-0.732552-0.693997-0.564013`
- Output (exact, full precision as stored): `-1.1520351120911565`
- Dimensionality: 5 (matches expected d=5 for Function_06)

## Source Files and Checksums (source project, read-only, unmodified)

All paths relative to `~/Documents/GitHub/My_Capstone_1_Imperial`.

| File | Role | SHA-256 |
|---|---|---|
| `Results/query_output_ledger.csv` | Canonical registry (v1.2), row: Week 1, Function 6 | `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d` |
| `Week_01/Function_06/04_Results/observations.csv` | Row 21: `21,recorded,0.728186,0.154693,0.732552,0.693997,0.564013,-1.1520351120911565` | `bc975ec5a12e28fa75788f68f0bf176099000a38fff24cb7b25b9f2683688273` |
| `Week_01/Function_06/04_Results/summary.json` | `latest_verified_input: [0.728186, 0.154693, 0.732552, 0.693997, 0.564013]`, `latest_verified_output: -1.1520351120911565`, `dimensions: 5` | `3ae875ee157e09d97c092679bdd4a1e45a5784c51a3aa2091f70d6c5fb38b0b8` |
| `Week_01/Function_06/03_Data/provenance.json` | Names `Results/query_output_ledger.csv` as `recorded_pairs_registry` | `693cd90deee635a38851ba73f5a864f637e35656690dcc4ea5fd64e6dd863ad1` |

## Cross-Checks Performed

- **Ledger vs. per-function records**: exact match against `observations.csv`
  row 21 and `summary.json`. **Agreement: YES.**
- **Dimensionality**: 5 coordinates, matches expected d=5. **PASS.**
- **Bounds** ([0.000000, 0.999999]): min=0.154693, max=0.732552. **PASS.**
- **Six-decimal-place consistency**: round-trip via `round(x*1e6)/1e6`
  reproduces all 5 coordinates exactly. **PASS.**
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
