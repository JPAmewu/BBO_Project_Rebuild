# Pair Provenance — Function_05, Historical Week_01

**Status: VERIFIED**

## Imported Values
- Query (as stored, full precision): `[0.224189, 0.84648, 0.879484, 0.878515]`
- Query (formatted to official 6-decimal submission convention, value
  unchanged): `0.224189-0.846480-0.879484-0.878515`
- Output (exact, full precision as stored): `1088.8535114737463`
- Dimensionality: 4 (matches expected d=4 for Function_05)

## Source Files and Checksums (source project, read-only, unmodified)

All paths relative to `~/Documents/GitHub/My_Capstone_1_Imperial`.

| File | Role | SHA-256 |
|---|---|---|
| `Results/query_output_ledger.csv` | Canonical registry (v1.2), row: Week 1, Function 5 | `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d` |
| `Week_01/Function_05/04_Results/observations.csv` | Row 21: `21,recorded,0.224189,0.84648,0.879484,0.878515,1088.8535114737463` | `0754f00d9e90f5c76f737b635a28a84c183462882895f11f69c7f8fb5a597e59` |
| `Week_01/Function_05/04_Results/summary.json` | `latest_verified_input: [0.224189, 0.84648, 0.879484, 0.878515]`, `latest_verified_output: 1088.8535114737463`, `dimensions: 4` | `68ad5a6621ea63e63abf0d7d796621164a6b5156bbc9fb3e446781eeb7090ae1` |
| `Week_01/Function_05/03_Data/provenance.json` | Names `Results/query_output_ledger.csv` as `recorded_pairs_registry` | `976afac48628cc392c9979c23ae713a47fbcecef45e31c41a1cb07718a02197e` |

## Cross-Checks Performed

- **Ledger vs. per-function records**: exact match against `observations.csv`
  row 21 and `summary.json`. **Agreement: YES.**
- **Dimensionality**: 4 coordinates, matches expected d=4. **PASS.**
- **Bounds** ([0.000000, 0.999999]): min=0.224189, max=0.879484. **PASS.**
- **Six-decimal-place consistency**: round-trip via `round(x*1e6)/1e6`
  reproduces all 4 coordinates exactly (`0.84648` is the same value as
  `0.846480` with a dropped trailing zero). **PASS.**
- **Finite scalar output**: single finite float. **PASS.**
- **Ledger file integrity**: SHA-256 matches `Results/query_output_ledger.sha256`
  and the canonical v1.2 entry in `Results/query_output_ledger_versions.json`.
  **CONFIRMED unaltered.**

## Note on This Function's Output Magnitude

This function's Week 1 output (`1088.8535114737463`) is the same
large-magnitude value previously investigated and cross-referenced in
`Week_01/PRE_MODELLING_VERIFICATION.md` (there, the rebuild project's own
Function_05 `initial_outputs.npy` max of `1088.8596181962705` was the subject
of that investigation; this historical Week-1 value is a distinct, separately
recorded query at nearly the same input, from the source project's own
historical record, not the rebuild's initial dataset). Its magnitude is
consistent with the same, already-documented finding that Function_05
genuinely produces large-magnitude outputs by design.

## Not Independently Verified (Out of Scope, Does Not Affect VERIFIED Status)

- Upstream external archive files (`Capstone_Folder/initial_data/*`) were not
  located/re-verified. VERIFIED status rests on the directly-confirmed
  agreement between the ledger and this function's own primary records.
- The notebook `Week_01/02_Notebook/Week_1_Capstone.ipynb` was not opened or
  executed.

## No Source Changes

No file in the source project was created, modified, renamed, or deleted as
part of this import.
