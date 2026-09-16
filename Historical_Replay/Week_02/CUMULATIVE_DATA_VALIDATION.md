# Cumulative Data Validation — Historical Replay Week_02 Starting Datasets

**Scope:** This validates the creation of `Historical_Replay/Week_02/Function_0X/02_Data/
cumulative_inputs.npy` and `cumulative_outputs.npy` for Function_01 through
Function_08. Each array was built by appending exactly one row/element — the
independently-verified historical Week 1 query-output pair from
`Historical_Replay/Week_01/Function_0X/` — to that function's original
`Week_01/Function_0X/02_Data/initial_inputs.npy` / `initial_outputs.npy` from
the rebuild project. No dataset was fabricated, interpolated, or guessed;
every value comes directly from an already-verified source file.

**Important clarification on row counts:** The task anticipated 11-row
cumulative arrays for every function. This assumed all functions start with
10 observations. In fact, only Function_01 and Function_02 have 10-row
starter datasets — Functions 03–08 have native starter counts of 15, 30, 20,
20, 30, and 40 respectively (already independently verified and documented in
`Week_01/WEEK_01_METHOD.md` and each function's own
`05_Documentation/data_quality_report.md`). This was flagged by the Data
Quality Agent before any file was created, and the user explicitly confirmed
proceeding with each function's **true native count + 1**, rather than
forcing a uniform 11 rows (which would have required fabricating or
discarding real data). The resulting cumulative row counts are 11, 11, 16,
31, 21, 21, 31, 41 for Functions 01–08 respectively.

**No Week 02 historical query or output has been imported, revealed, or
used anywhere in this step.** Only the already-verified Week 1 pair was
appended. No model was built and no query was generated. `Week_01`, the
original source project, and the existing `Historical_Replay/Week_01/` files
were not modified.

## Method

For each function: `initial_inputs.npy`/`initial_outputs.npy` were loaded
with `np.load`; `historical_query.txt`/`historical_output.txt` were parsed
into a float64 vector and scalar; the cumulative arrays were built via
`np.vstack`/`np.append`; all validation checks below were computed
programmatically (not asserted by inspection) before any file was written —
a function would only have its files saved if every check passed, per the
"stop rather than guess" instruction. All 8 functions passed every check on
the first attempt; no function required stopping.

## Per-Function Validation Results

### Function_01

- Input shape: `(10, 2)` → cumulative `(11, 2)`. Output shape: `(10,)` →
  cumulative `(11,)`.
- First 10 rows/elements of the cumulative arrays are byte-identical
  (`np.array_equal`) to the original `initial_inputs.npy`/`initial_outputs.npy`:
  **PASS**.
- Row 11 (index 10) exactly matches the verified historical pair — query
  `[0.374540, 0.950714]`, output `-1.560646704467778e-117`: **PASS**.
- Data types: `float64` throughout (original and cumulative, inputs and
  outputs): **PASS**.
- Finite-value check: no NaN/Inf anywhere in either array: **PASS**.
- Bounds check: min `0.078723`, max `0.950714`, both within
  `[0.000000, 0.999999]`: **PASS**.
- Duplicate-input check: all 11 rows unique when rounded to 6 decimal
  places: **PASS**.
- Source paths and SHA-256:
  - `Week_01/Function_01/02_Data/initial_inputs.npy`:
    `ad0017b5583aababb3d3573bc9139f013fc328aa70eba5748f54ef4f1505ad0a`
  - `Week_01/Function_01/02_Data/initial_outputs.npy`:
    `8335e4febb64aec5e7b62a7761c0e610e5f4611551a393d225b03feec84e37db`
  - `Historical_Replay/Week_01/Function_01/historical_query.txt`:
    `b9452640794a2a0a2d9d4c7643ecc5ab4880e62ab5712a0be7b8f335339cf319`
  - `Historical_Replay/Week_01/Function_01/historical_output.txt`:
    `1aabbf9e044fe711b8fb1316d09c1e1ac1718105d0fe5f02731e66dbda253aa9`
  - `Historical_Replay/Week_02/Function_01/02_Data/cumulative_inputs.npy`
    (new): `b78536d083aa66e42f21c99cf71dd5e9ed1a71b0f2d6a039095bbf2674e013c9`
  - `Historical_Replay/Week_02/Function_01/02_Data/cumulative_outputs.npy`
    (new): `d31f1155689179426de763a750f701283b6d4d09216b7840b6c6b70f947916b`

### Function_02

- Input shape: `(10, 2)` → cumulative `(11, 2)`. Output shape: `(10,)` →
  cumulative `(11,)`.
- First-10 byte-identical: **PASS**. Row 11 exact match — query
  `[0.374540, 0.950714]`, output `-0.03182956281754251`: **PASS**.
- Data types: `float64` throughout: **PASS**.
- Finite-value check: **PASS**.
- Bounds: min `0.028698`, max `0.950714`: **PASS**.
- Duplicate check: no duplicates among 11 rows: **PASS**.
- Source paths and SHA-256:
  - `Week_01/Function_02/02_Data/initial_inputs.npy`:
    `9824ca17efa0fcc70e58040b3e6d6cea7c2d4b8be76023e4df0dfb1788667505`
  - `Week_01/Function_02/02_Data/initial_outputs.npy`:
    `b4f71bdfd3a50452a2864cffc00b4548ea5d1fb746982ab79b439cee84568a2d`
  - `Historical_Replay/Week_01/Function_02/historical_query.txt`:
    `b9452640794a2a0a2d9d4c7643ecc5ab4880e62ab5712a0be7b8f335339cf319`
  - `Historical_Replay/Week_01/Function_02/historical_output.txt`:
    `86285492838899d161ebab70d3c816cf17a1f4fda8222a9d614232345544263`
  - `cumulative_inputs.npy` (new):
    `2e953eaaa0946216b8eb079b4affafbd2eded497976917443915127fb742daf`
  - `cumulative_outputs.npy` (new):
    `756099c0a6250ff407ef0c6ea69a78a42ab7aad545bb15875a6c2e3f53b20a8`

### Function_03

- Input shape: `(15, 3)` → cumulative `(16, 3)`. Output shape: `(15,)` →
  cumulative `(16,)`.
- First-15 byte-identical: **PASS**. Row 16 exact match — query
  `[0.444444, 0.666666, 0.333333]`, output `-0.04090761844901528`: **PASS**.
- Data types: `float64` throughout: **PASS**.
- Finite-value check: **PASS**.
- Bounds: min `0.046809`, max `0.990882`: **PASS**.
- Duplicate check: no duplicates among 16 rows: **PASS**.
- Source paths and SHA-256:
  - `Week_01/Function_03/02_Data/initial_inputs.npy`:
    `e54dc0e1bd94e301d0756faaf94bff74f60497f67d7fbdf7eb167fa10fe0ed71`
  - `Week_01/Function_03/02_Data/initial_outputs.npy`:
    `f7daf48bc753f8314f09c9a2abfad391382010ab789ca8145bc3e028871dc013`
  - `Historical_Replay/Week_01/Function_03/historical_query.txt`:
    `2845cbcc5fa83b03f3e3d01aed9378c750006d15b3ca2a30102e6ac39cd434f`
  - `Historical_Replay/Week_01/Function_03/historical_output.txt`:
    `0613197eb96d8dcbad15adadd364fc17f14caf7a13f237cfa79673f740aead7`
  - `cumulative_inputs.npy` (new):
    `3d2f4d29ffe34ec15629fdfe8c579506d16a0c6377506abf2d7f3aa5c7235e1`
  - `cumulative_outputs.npy` (new):
    `db06a53358bb2c76e7fb2cbf6200138abfee13b095886ab64cf1110e4cbcfb8`

### Function_04

- Input shape: `(30, 4)` → cumulative `(31, 4)`. Output shape: `(30,)` →
  cumulative `(31,)`.
- First-30 byte-identical: **PASS**. Row 31 exact match — query
  `[0.555555, 0.444444, 0.222222, 0.111111]`, output `-8.727516493155957`:
  **PASS**.
- Data types: `float64` throughout: **PASS**.
- Finite-value check: **PASS**.
- Bounds: min `0.006250`, max `0.999483`: **PASS**.
- Duplicate check: no duplicates among 31 rows: **PASS**.
- Source paths and SHA-256:
  - `Week_01/Function_04/02_Data/initial_inputs.npy`:
    `e431a04b3c4678191bf38f398cd79fdc826aea47a3e7c6b6d72e234ec79339e0`
  - `Week_01/Function_04/02_Data/initial_outputs.npy`:
    `dd594bb7929e5a57d852cbdaf7eb0bb184a3dced78b73c7c03f40ccf9046ec3f`
  - `Historical_Replay/Week_01/Function_04/historical_query.txt`:
    `54cffb36253c3be59b636cf46d154d953903c9489df0aa90b99994f448600d0`
  - `Historical_Replay/Week_01/Function_04/historical_output.txt`:
    `af3be88ca261a83e50d7e7cfe471669e985bff36766cebb709296a3e0fea0b8`
  - `cumulative_inputs.npy` (new):
    `396b567c1b58c21c8f926a71615a24ebb9d1597604cba8de618f14092293b1b`
  - `cumulative_outputs.npy` (new):
    `ac3d0aff8947f5a71af10481defb1a66c83b3aa2de6d411f55d28a4cd2d00a0`

### Function_05

- Input shape: `(20, 4)` → cumulative `(21, 4)`. Output shape: `(20,)` →
  cumulative `(21,)`.
- First-20 byte-identical: **PASS**. Row 21 exact match — query
  `[0.224189, 0.846480, 0.879484, 0.878515]`, output `1088.8535114737463`:
  **PASS**.
- Data types: `float64` throughout: **PASS**.
- Finite-value check: **PASS**.
- Bounds: min `0.038193`, max `0.957644`: **PASS**.
- Duplicate check: no duplicates among 21 rows: **PASS**.
- **Informational note (not a failure):** the appended row (index 20) is
  numerically very close to an existing original row (index 15:
  `[0.22418902330288348, 0.8464804904862864, 0.8794841797090803, 0.8785156842249731]`).
  Coordinate differences are on the order of 1e-7 to 7e-7 — near, but not
  crossing, the 6-decimal-place duplicate threshold (the 4th coordinate
  rounds to `0.878516` for the original vs. `0.878515` for the historical
  query). The duplicate check correctly did not flag this, since the rounded
  values differ. This reflects the historical record's own re-query behavior
  at a nearly identical point (already discussed in
  `HISTORICAL_REPLAY_INVENTORY.md`'s Function_05 findings) — it is not a
  defect introduced by this import, but is noted for downstream awareness.
- Source paths and SHA-256:
  - `Week_01/Function_05/02_Data/initial_inputs.npy`:
    `01b8b441980fdfc28af5d2090a478eebf1081ba11da3364d94473ad90f4431aa`
  - `Week_01/Function_05/02_Data/initial_outputs.npy`:
    `00d0346f34dd8388feee4a2b603efa29d1c9b99db6ca59225427cb838923b78`
  - `Historical_Replay/Week_01/Function_05/historical_query.txt`:
    `dc6b49f8f541a3e4552666e1e3f611e150f1683732c439b85ba4f0a42252ed9`
  - `Historical_Replay/Week_01/Function_05/historical_output.txt`:
    `1381bb8d4299c00e77c53135a557ab6c7d26d84f6e274803b93686426942839`
  - `cumulative_inputs.npy` (new):
    `c0664ced172d7d8642ccbc902308177d2b04dbf847912d6d20fd116b340ecbb`
  - `cumulative_outputs.npy` (new):
    `7751a3bb3d95d75d67aa87e58c244b26059c58bf93fb27cc3f74d81804bdc11`

### Function_06

- Input shape: `(20, 5)` → cumulative `(21, 5)`. Output shape: `(20,)` →
  cumulative `(21,)`.
- First-20 byte-identical: **PASS**. Row 21 exact match — query
  `[0.728186, 0.154693, 0.732552, 0.693997, 0.564013]`, output
  `-1.1520351120911565`: **PASS**.
- Data types: `float64` throughout: **PASS**.
- Finite-value check: **PASS**.
- Bounds: min `0.004911`, max `0.978806`: **PASS**.
- Duplicate check: no duplicates among 21 rows: **PASS**.
- **Informational note (not a failure, unconfirmed):** the first 4
  coordinates of the appended historical row (`0.728186, 0.154693, 0.732552,
  0.693997`) are numerically close to the first 4 coordinates of the
  existing original row 1
  (`0.7281861047460138, 0.1546925696237983, 0.7325516687239811, 0.6939965090690888`),
  but the 5th coordinate differs by roughly a factor of 10
  (`0.564013` vs. `0.05640131...`). This pattern is consistent with, but does
  not prove, a possible decimal-place shift somewhere upstream in the
  original source data generation. This was not investigated further as it
  is outside this task's scope (the duplicate/bounds/finite checks all still
  pass correctly regardless), and no value was altered to compensate for it.
- Source paths and SHA-256:
  - `Week_01/Function_06/02_Data/initial_inputs.npy`:
    `3a60333cd371f97a3f4c682d66bf017711ed4229f5f3c477bb5a949793c1a2d4`
  - `Week_01/Function_06/02_Data/initial_outputs.npy`:
    `5819356de777531e31ffa250fff14123bd378421476b068a4ac931cf73a35a82`
  - `Historical_Replay/Week_01/Function_06/historical_query.txt`:
    `16a28425b07debc20e9e048ae942b8340e509e17d87bbf9d2e49a3986845b37`
  - `Historical_Replay/Week_01/Function_06/historical_output.txt`:
    `8723a734fbca90d408b9c0fbdad5099a1abce1d6dcc2b204235ff31d85d95e6`
  - `cumulative_inputs.npy` (new):
    `a02bebc15427f119e5beccef317ea78bde0e71243dc1435ef29f2caa08ac075`
  - `cumulative_outputs.npy` (new):
    `beca42746dcbd38b7151442654519872fb27f57fe51cc35aa29c818ded48b2e`

### Function_07

- Input shape: `(30, 6)` → cumulative `(31, 6)`. Output shape: `(30,)` →
  cumulative `(31,)`.
- First-30 byte-identical: **PASS**. Row 31 exact match — query
  `[0.045091, 0.528666, 0.329265, 0.105350, 0.434667, 0.641164]`, output
  `1.0510148516295004`: **PASS**.
- Data types: `float64` throughout: **PASS**.
- Finite-value check: **PASS**.
- Bounds: min `0.003635`, max `0.998655`: **PASS**.
- Duplicate check: no duplicates among 31 rows: **PASS**.
- Source paths and SHA-256:
  - `Week_01/Function_07/02_Data/initial_inputs.npy`:
    `f45f2203870f77a1e026ceab770305a240d9fdaa5c65db989bd0957450ed935`
  - `Week_01/Function_07/02_Data/initial_outputs.npy`:
    `9dcac9c3aaee049db5de1efde196643bfda55c29079da7658d02c351d10abd9`
  - `Historical_Replay/Week_01/Function_07/historical_query.txt`:
    `61a99756a7fe35307bdf0d73c9fe76754cd4fab182024f82041354cb77150bf`
  - `Historical_Replay/Week_01/Function_07/historical_output.txt`:
    `76f7d3617effea8b29cc9baad595f0d7a501eb2d1204646eaee5c4b61e3d5c4`
  - `cumulative_inputs.npy` (new):
    `462dafc9baeb7551b145ada8fd988d556e2c0311237b07ffd6b6b0a1e7e96e8`
  - `cumulative_outputs.npy` (new):
    `f7e12fe5d14292a998353cdfc3c22831113d1efeed139fab1ac1684ee671276`

### Function_08

- Input shape: `(40, 8)` → cumulative `(41, 8)`. Output shape: `(40,)` →
  cumulative `(41,)`.
- First-40 byte-identical: **PASS**. Row 41 exact match — query
  `[0.273673, 0.260400, 0.073937, 0.078562, 0.862321, 0.230729, 0.108688, 0.352588]`,
  output `9.8157087929671`: **PASS**.
- Data types: `float64` throughout: **PASS**.
- Finite-value check: **PASS**.
- Bounds: min `0.003419`, max `0.998885`: **PASS**.
- Duplicate check: no duplicates among 41 rows: **PASS**.
- Source paths and SHA-256:
  - `Week_01/Function_08/02_Data/initial_inputs.npy`:
    `fb5ebbb93c990cbdecc44d648362502233b4b5daec335319aca300f9e9b6920`
  - `Week_01/Function_08/02_Data/initial_outputs.npy`:
    `693ebca2bb2b738162f701f2587bad0d3f2ac1b96d47d32fa6357f2b8e25779`
  - `Historical_Replay/Week_01/Function_08/historical_query.txt`:
    `756bb2e32f781920168e6d5bbea287dde403699a9c644015f379f0d312840e7`
  - `Historical_Replay/Week_01/Function_08/historical_output.txt`:
    `503a5bd85d44400783b44edfa55c044137866b4f051c3a98bd6f012d00d6877`
  - `cumulative_inputs.npy` (new):
    `3c3e6adddc6ed389b1b1b2e8b67771b5424d402f249288ac8c85707cb5f7f47`
  - `cumulative_outputs.npy` (new):
    `72867d027b9bb995bb93ad1235563552e00ae0643b93552fe801d5a91ae23cb`

## Summary Table

| Function | Original shape | Cumulative shape | First-N identical | Row N+1 match | Dtype | Finite | Bounds | Duplicates | Overall |
|---|---|---|---|---|---|---|---|---|---|
| 01 | (10,2)/(10,) | (11,2)/(11,) | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 02 | (10,2)/(10,) | (11,2)/(11,) | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 03 | (15,3)/(15,) | (16,3)/(16,) | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 04 | (30,4)/(30,) | (31,4)/(31,) | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 05 | (20,4)/(20,) | (21,4)/(21,) | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 06 | (20,5)/(20,) | (21,5)/(21,) | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 07 | (30,6)/(30,) | (31,6)/(31,) | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 08 | (40,8)/(40,) | (41,8)/(41,) | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |

**8 of 8 functions: PASS. No function required stopping — every check
passed for every function on the first attempt, using each function's true
native row count.**

## Confirmations

- **Original `Week_01` datasets unchanged:** all 16 `initial_inputs.npy`/
  `initial_outputs.npy` checksums recomputed and confirmed identical to
  their previously-recorded values from earlier audits in this project.
- **`Historical_Replay/Week_01/` unchanged:** no file in that directory was
  modified as part of this step.
- **Source project unchanged:** `git status --porcelain` in
  `~/Documents/GitHub/My_Capstone_1_Imperial` returns clean (no modified
  files).
- **No Week 02 historical query or output was imported, revealed, or used.**
  Only the already-verified Week 1 pair was appended.
- **No model was built and no query was generated** at this step.
- **No value was fabricated, interpolated, or guessed** — every number in
  the cumulative arrays traces directly to an already-verified source file.

## Final Verdict

# **PASS — 8/8 Historical Replay Week_02 starting datasets created and validated**

---

## Correction Addendum (2026-09-16) — Checksum Transcription Fix

An independent review by the ML Reviewer Agent
(`Historical_Replay/Week_02/INDEPENDENT_CUMULATIVE_REVIEW.md`) found that
many SHA-256 checksums recorded above were rendered as **63 hex characters
instead of the correct 64** — each recorded value is an exact prefix of the
correct 64-character hash, missing only the final hex digit. This affected
roughly 19 of 48 checksum entries above. The independent review confirmed
this is a **transcription/rendering defect in this document only** — the
underlying `.npy` and `.txt` files themselves are unmodified (probability of
a 63-character exact-prefix match against a genuinely different file is
astronomically small, and all 16 original dataset checksums were separately
confirmed unchanged from their previously-recorded values across this
project). No data integrity issue exists; only this document's transcription
needed correcting.

The authoritative, independently-recomputed, full 64-character checksums for
every file are below. These supersede any truncated value in the per-function
sections above; nothing else in this document changes.

| Function | File | SHA-256 (64 hex characters) |
|---|---|---|
| 01 | `Week_01/Function_01/02_Data/initial_inputs.npy` | `ad0017b5583aababb3d3573bc9139f013fc328aa70eba5748f54ef4f1505ad0a` |
| 01 | `Week_01/Function_01/02_Data/initial_outputs.npy` | `8335e4febb64aec5e7b62a7761c0e610e5f4611551a393d225b03feec84e37db` |
| 01 | `Historical_Replay/Week_01/Function_01/historical_query.txt` | `b9452640794a2a0a2d9d4c7643ecc5ab4880e62ab5712a0be7b8f335339cf319` |
| 01 | `Historical_Replay/Week_01/Function_01/historical_output.txt` | `1aabbf9e044fe711b8fb1316d09c1e1ac1718105d0fe5f02731e66dbda253aa9` |
| 01 | `Historical_Replay/Week_02/Function_01/02_Data/cumulative_inputs.npy` | `b78536d083aa66e42f21c99cf71dd5e9ed1a71b0f2d6a039095bbf2674e013c9` |
| 01 | `Historical_Replay/Week_02/Function_01/02_Data/cumulative_outputs.npy` | `d31f1155689179426de763a750f701283b6d4d09216b7840b6c6b70f947916b3` |
| 02 | `Week_01/Function_02/02_Data/initial_inputs.npy` | `9824ca17efa0fcc70e58040b3e6d6cea7c2d4b8be76023e4df0dfb1788667505` |
| 02 | `Week_01/Function_02/02_Data/initial_outputs.npy` | `b4f71bdfd3a50452a2864cffc00b4548ea5d1fb746982ab79b439cee84568a2d` |
| 02 | `Historical_Replay/Week_01/Function_02/historical_query.txt` | `b9452640794a2a0a2d9d4c7643ecc5ab4880e62ab5712a0be7b8f335339cf319` |
| 02 | `Historical_Replay/Week_01/Function_02/historical_output.txt` | `86285492838899d161ebab70d3c816cf17a1f4fda8222a9d614232345544263d` |
| 02 | `Historical_Replay/Week_02/Function_02/02_Data/cumulative_inputs.npy` | `2e953eaaa0946216b8eb079b4affafbd2eded497976917443915127fb742daf7` |
| 02 | `Historical_Replay/Week_02/Function_02/02_Data/cumulative_outputs.npy` | `756099c0a6250ff407ef0c6ea69a78a42ab7aad545bb15875a6c2e3f53b20a87` |
| 03 | `Week_01/Function_03/02_Data/initial_inputs.npy` | `e54dc0e1bd94e301d0756faaf94bff74f60497f67d7fbdf7eb167fa10fe0ed71` |
| 03 | `Week_01/Function_03/02_Data/initial_outputs.npy` | `f7daf48bc753f8314f09c9a2abfad391382010ab789ca8145bc3e028871dc013` |
| 03 | `Historical_Replay/Week_01/Function_03/historical_query.txt` | `2845cbcc5fa83b03f3e3d01aed9378c750006d15b3ca2a30102e6ac39cd434f3` |
| 03 | `Historical_Replay/Week_01/Function_03/historical_output.txt` | `0613197eb96d8dcbad15adadd364fc17f14caf7a13f237cfa79673f740aead71` |
| 03 | `Historical_Replay/Week_02/Function_03/02_Data/cumulative_inputs.npy` | `3d2f4d29ffe34ec15629fdfe8c579506d16a0c6377506abf2d7f3aa5c7235e1d` |
| 03 | `Historical_Replay/Week_02/Function_03/02_Data/cumulative_outputs.npy` | `db06a53358bb2c76e7fb2cbf6200138abfee13b095886ab64cf1110e4cbcfb81` |
| 04 | `Week_01/Function_04/02_Data/initial_inputs.npy` | `e431a04b3c4678191bf38f398cd79fdc826aea47a3e7c6b6d72e234ec79339e0` |
| 04 | `Week_01/Function_04/02_Data/initial_outputs.npy` | `dd594bb7929e5a57d852cbdaf7eb0bb184a3dced78b73c7c03f40ccf9046ec3f` |
| 04 | `Historical_Replay/Week_01/Function_04/historical_query.txt` | `54cffb36253c3be59b636cf46d154d953903c9489df0aa90b99994f448600d0e` |
| 04 | `Historical_Replay/Week_01/Function_04/historical_output.txt` | `af3be88ca261a83e50d7e7cfe471669e985bff36766cebb709296a3e0fea0b8c` |
| 04 | `Historical_Replay/Week_02/Function_04/02_Data/cumulative_inputs.npy` | `396b567c1b58c21c8f926a71615a24ebb9d1597604cba8de618f14092293b1b6` |
| 04 | `Historical_Replay/Week_02/Function_04/02_Data/cumulative_outputs.npy` | `ac3d0aff8947f5a71af10481defb1a66c83b3aa2de6d411f55d28a4cd2d00a03` |
| 05 | `Week_01/Function_05/02_Data/initial_inputs.npy` | `01b8b441980fdfc28af5d2090a478eebf1081ba11da3364d94473ad90f4431aa` |
| 05 | `Week_01/Function_05/02_Data/initial_outputs.npy` | `00d0346f34dd8388feee4a2b603efa29d1c9b99db6ca59225427cb838923b788` |
| 05 | `Historical_Replay/Week_01/Function_05/historical_query.txt` | `dc6b49f8f541a3e4552666e1e3f611e150f1683732c439b85ba4f0a42252ed9c` |
| 05 | `Historical_Replay/Week_01/Function_05/historical_output.txt` | `1381bb8d4299c00e77c53135a557ab6c7d26d84f6e274803b93686426942839a` |
| 05 | `Historical_Replay/Week_02/Function_05/02_Data/cumulative_inputs.npy` | `c0664ced172d7d8642ccbc902308177d2b04dbf847912d6d20fd116b340ecbb9` |
| 05 | `Historical_Replay/Week_02/Function_05/02_Data/cumulative_outputs.npy` | `7751a3bb3d95d75d67aa87e58c244b26059c58bf93fb27cc3f74d81804bdc110` |
| 06 | `Week_01/Function_06/02_Data/initial_inputs.npy` | `3a60333cd371f97a3f4c682d66bf017711ed4229f5f3c477bb5a949793c1a2d4` |
| 06 | `Week_01/Function_06/02_Data/initial_outputs.npy` | `5819356de777531e31ffa250fff14123bd378421476b068a4ac931cf73a35a82` |
| 06 | `Historical_Replay/Week_01/Function_06/historical_query.txt` | `16a28425b07debc20e9e048ae942b8340e509e17d87bbf9d2e49a3986845b378` |
| 06 | `Historical_Replay/Week_01/Function_06/historical_output.txt` | `8723a734fbca90d408b9c0fbdad5099a1abce1d6dcc2b204235ff31d85d95e64` |
| 06 | `Historical_Replay/Week_02/Function_06/02_Data/cumulative_inputs.npy` | `a02bebc15427f119e5beccef317ea78bde0e71243dc1435ef29f2caa08ac0753` |
| 06 | `Historical_Replay/Week_02/Function_06/02_Data/cumulative_outputs.npy` | `beca42746dcbd38b7151442654519872fb27f57fe51cc35aa29c818ded48b2e0` |
| 07 | `Week_01/Function_07/02_Data/initial_inputs.npy` | `f45f2203870f77a1e026ceab770305a240d9fdaa5c65db989bd0957450ed9353` |
| 07 | `Week_01/Function_07/02_Data/initial_outputs.npy` | `9dcac9c3aaee049db5de1efde196643bfda55c29079da7658d02c351d10abd95` |
| 07 | `Historical_Replay/Week_01/Function_07/historical_query.txt` | `61a99756a7fe35307bdf0d73c9fe76754cd4fab182024f82041354cb77150bf6` |
| 07 | `Historical_Replay/Week_01/Function_07/historical_output.txt` | `76f7d3617effea8b29cc9baad595f0d7a501eb2d1204646eaee5c4b61e3d5c49` |
| 07 | `Historical_Replay/Week_02/Function_07/02_Data/cumulative_inputs.npy` | `462dafc9baeb7551b145ada8fd988d556e2c0311237b07ffd6b6b0a1e7e96e85` |
| 07 | `Historical_Replay/Week_02/Function_07/02_Data/cumulative_outputs.npy` | `f7e12fe5d14292a998353cdfc3c22831113d1efeed139fab1ac1684ee6712765` |
| 08 | `Week_01/Function_08/02_Data/initial_inputs.npy` | `fb5ebbb93c990cbdecc44d648362502233b4b5daec335319aca300f9e9b69206` |
| 08 | `Week_01/Function_08/02_Data/initial_outputs.npy` | `693ebca2bb2b738162f701f2587bad0d3f2ac1b96d47d32fa6357f2b8e257798` |
| 08 | `Historical_Replay/Week_01/Function_08/historical_query.txt` | `756bb2e32f781920168e6d5bbea287dde403699a9c644015f379f0d312840e70` |
| 08 | `Historical_Replay/Week_01/Function_08/historical_output.txt` | `503a5bd85d44400783b44edfa55c044137866b4f051c3a98bd6f012d00d68776` |
| 08 | `Historical_Replay/Week_02/Function_08/02_Data/cumulative_inputs.npy` | `3c3e6adddc6ed389b1b1b2e8b67771b5424d402f249288ac8c85707cb5f7f479` |
| 08 | `Historical_Replay/Week_02/Function_08/02_Data/cumulative_outputs.npy` | `72867d027b9bb995bb93ad1235563552e00ae0643b93552fe801d5a91ae23cb1` |

This addendum does not change the PASS verdict above — it corrects a
documentation rendering defect only.
