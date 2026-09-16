# Pair Provenance — Function_01, Historical Week_01

**Status: VERIFIED**

## Imported Values
- Query (as stored, full precision): `[0.37454, 0.950714]`
- Query (formatted to official 6-decimal submission convention, value
  unchanged): `0.374540-0.950714`
- Output (exact, full precision as stored): `-1.560646704467778e-117`
- Dimensionality: 2 (matches expected d=2 for Function_01)

## Source Files and Checksums (source project, read-only, unmodified)

All paths relative to `~/Documents/GitHub/My_Capstone_1_Imperial`.

| File | Role | SHA-256 |
|---|---|---|
| `Results/query_output_ledger.csv` | Canonical registry (v1.2), row: Week 1, Function 1 | `303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d` |
| `Week_01/Function_01/04_Results/observations.csv` | Row 11: `11,recorded,0.37454,0.950714,-1.560646704467778e-117` | `6663e0526f7879bbb4d72f5ac3e3d10b9adb731797a9a69a69b1a29cbc1dca5c` |
| `Week_01/Function_01/04_Results/summary.json` | `latest_verified_input: [0.37454, 0.950714]`, `latest_verified_output: -1.560646704467778e-117`, `dimensions: 2` | `8a9c8b3dbf00d4f2ab6d56cf391e635323fe6aae9504bf5b99f0159b81d1d2c9` |
| `Week_01/Function_01/03_Data/provenance.json` | Names `Results/query_output_ledger.csv` as `recorded_pairs_registry` | `52e4f8a1a235bef90c426ec4dbcb4a65224080b8994f09a33046d7719b081849` |

## Cross-Checks Performed

- **Ledger vs. per-function records**: The ledger's query and output match
  `observations.csv` row 11 and `summary.json`'s `latest_verified_input`/
  `latest_verified_output` exactly, coordinate-by-coordinate and
  value-for-value. **Agreement: YES.**
- **Dimensionality**: 2 coordinates present, matches expected d=2. **PASS.**
- **Bounds** ([0.000000, 0.999999]): min=0.37454, max=0.950714, both within
  range. **PASS.**
- **Six-decimal-place consistency**: `round(x * 1e6) / 1e6` reproduces each
  coordinate exactly (round-trip difference = 0.0) for both coordinates —
  consistent with a genuine 6-decimal-place submission (`0.37454` is the
  same value as `0.374540` with a dropped trailing zero). **PASS.**
- **Finite scalar output**: the output is a single finite float, not an
  array, not NaN, not Inf. **PASS.**
- **Ledger file integrity**: the ledger's SHA-256 matches both
  `Results/query_output_ledger.sha256` and the canonical v1.2 entry in
  `Results/query_output_ledger_versions.json`. **CONFIRMED unaltered.**

## Not Independently Verified (Out of Scope, Does Not Affect VERIFIED Status)

- The ledger's `source_input`/`source_output` columns point to
  `Capstone_Folder/initial_data/week_01_inputs.txt`/`week_01_outputs.txt`
  (an external, non-repository archive). This deepest upstream layer was not
  located/re-verified as part of this import; the VERIFIED status above rests
  on the independently confirmed agreement between the ledger and this
  function's own primary records (`observations.csv`/`summary.json`), which
  was directly checked.
- The notebook `Week_01/02_Notebook/Week_1_Capstone.ipynb` referenced by the
  ledger was not opened or executed (not required for this cross-check).

## No Source Changes

No file in the source project was created, modified, renamed, or deleted as
part of this import. The checksums above were computed from the source
project's files exactly as found, and can be used to detect any future
accidental change.
