# Pair Provenance — Function_04, Historical Week_01

**Status: VERIFIED**

## Imported Values
- Query (as stored, full precision): `[0.555555, 0.444444, 0.222222, 0.111111]`
- Query (formatted to official 6-decimal submission convention, value
  unchanged): `0.555555-0.444444-0.222222-0.111111`
- Output (exact, full precision as stored): `-8.727516493155957`
- Dimensionality: 4 (matches expected d=4 for Function_04)

## Source Files and Checksums (source project, read-only, unmodified)

All paths relative to `~/Documents/GitHub/My_Capstone_1_Imperial`.

| File | Role | SHA-256 |
|---|---|---|
| `Results/query_output_ledger.csv` | Canonical registry (v1.2), row: Week 1, Function 4 | `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d` |
| `Week_01/Function_04/04_Results/observations.csv` | Row 31: `31,recorded,0.555555,0.444444,0.222222,0.111111,-8.727516493155957` | `a27c7be120eb43560b4d49749ebb8e554bc92faa51efb96213e2dd88d0fb0e28` |
| `Week_01/Function_04/04_Results/summary.json` | `latest_verified_input: [0.555555, 0.444444, 0.222222, 0.111111]`, `latest_verified_output: -8.727516493155957`, `dimensions: 4` | `779dcc294d1982917a5496e0afcd879a9034a40977a011e88b8ced0168dcc393` |
| `Week_01/Function_04/03_Data/provenance.json` | Names `Results/query_output_ledger.csv` as `recorded_pairs_registry` | `87dc745d79592462d86cc5b67edecfcee7d8d1ef51da6fbe9afdad8e7c34a5ab` |

## Cross-Checks Performed

- **Ledger vs. per-function records**: exact match against `observations.csv`
  row 31 and `summary.json`. **Agreement: YES.**
- **Dimensionality**: 4 coordinates, matches expected d=4. **PASS.**
- **Bounds** ([0.000000, 0.999999]): min=0.111111, max=0.555555. **PASS.**
- **Six-decimal-place consistency**: round-trip via `round(x*1e6)/1e6`
  reproduces all 4 coordinates exactly. **PASS.**
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
