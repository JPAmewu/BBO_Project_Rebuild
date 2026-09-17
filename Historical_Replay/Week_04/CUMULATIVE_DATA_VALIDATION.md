# Cumulative Data Validation — Historical Replay Week_04 Starting Datasets

**Scope:** This validates the creation of `Historical_Replay/Week_04/Function_0X/02_Data/
cumulative_inputs.npy` and `cumulative_outputs.npy` for Function_01 through
Function_08. Each array was built by appending exactly one row/element — the
independently-verified genuine historical Week 3 query-output pair from
`Historical_Replay/Week_03/Function_0X/historical_query.txt`/
`historical_output.txt` — to that function's existing Week 3 cumulative
dataset (`Historical_Replay/Week_03/Function_0X/02_Data/cumulative_*.npy`).
No value was fabricated, interpolated, or guessed; every number comes
directly from an already-verified source file.

**No Week 04 historical query or output has been imported, revealed, or
used anywhere in this step.** Only the already-verified Week 3 pair was
appended. No Week 04 notebook was created. No new query was generated and
no model was fit. `Week_01`, `Week_02`, `Week_03` (of both the rebuild and
the Historical Replay track), and the original source project were not
modified.

## Method

For each function: the existing `Historical_Replay/Week_03/Function_0X/
02_Data/cumulative_{inputs,outputs}.npy` arrays were loaded with `np.load`;
`Historical_Replay/Week_03/Function_0X/historical_{query,output}.txt` were
parsed into a float64 vector and scalar; the new Week 4 arrays were built
via `np.vstack`/`np.append`; all validation checks below were computed
programmatically — not asserted by inspection — before any file was
written, per the "stop rather than guess" instruction. All 8 functions
passed every check on the first attempt; no function required stopping.

## Per-Function Validation Results

### Function_01

- Prior (Week 3) shape: `(12, 2)` → final (Week 4) shape: `(13, 2)`.
  Outputs: `(12,)` → `(13,)`. Matches the required row count of **13**.
- All 12 prior rows/elements are byte-identical (`np.array_equal`) to the
  Week 3 cumulative arrays, in original order: **PASS**.
- Final row (index 12) exactly matches the verified Week 3 historical pair
  — query `[0.614168, 0.334143]`, output `-1.0755942664604116e-32`: **PASS**.
- Input/output observation counts match: 13 = 13. **PASS**.
- Data types: `float64` throughout: **PASS**.
- Finite-value check: no NaN/Inf anywhere: **PASS**.
- Bounds check: min `0.078723`, max `0.950714`, within
  `[0.000000, 0.999999]`: **PASS**.
- Duplicate-input check: all 13 rows unique when rounded to 6 decimal
  places: **PASS**.
- Source paths and SHA-256:
  - `Historical_Replay/Week_03/Function_01/02_Data/cumulative_inputs.npy`:
    `e30d9c1adef2752f59ba2e4a6005c439ff23867b40bfa3776935fdfd937949ea`
  - `Historical_Replay/Week_03/Function_01/02_Data/cumulative_outputs.npy`:
    `f53221c8c178cf73aae46a6d27ff88a6e7a4733a222ea240a21f1bc1469177d1`
  - `Historical_Replay/Week_03/Function_01/historical_query.txt`:
    `43039384419b71d66227a1fa5cefd568d5e96ca70d25344a6b6e42591b9582ab`
  - `Historical_Replay/Week_03/Function_01/historical_output.txt`:
    `59e50de1d87f7b957fb83f305e83736fc2055b0de0daaa2d93bc7e23fb257c76`
  - `Historical_Replay/Week_04/Function_01/02_Data/cumulative_inputs.npy`
    (new): `f67d109bd12601b4fa619a4800ecdf09958c623fbce91508d698f40e3acf9ed4`
  - `Historical_Replay/Week_04/Function_01/02_Data/cumulative_outputs.npy`
    (new): `d9f0cdcf01527b38d06bc2bcb01ea87f22ff5757e98ce6966db9dc6506278fd0`

### Function_02

- Prior shape: `(12, 2)` → final `(13, 2)`. Outputs: `(12,)` → `(13,)`.
  Matches required row count **13**.
- Prior 12 rows byte-identical: **PASS**. Final row exact match — query
  `[6e-06, 0.334143]`, output `0.04868370128566149`: **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.000006` (confirmed ≥0, genuine), max `0.978752`: **PASS**.
- Duplicate check: no duplicates among 13 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_03/Function_02/02_Data/cumulative_inputs.npy`:
    `ddb20064e337cc47b0c912fdbf5e1b757c19a842de5ff13e17416bb64d42a675`
  - `.../Week_03/Function_02/02_Data/cumulative_outputs.npy`:
    `9748449ec40362fdc4a8fdac4de6cf213f9342d6c05dd9180360c0dd3bd58b0d`
  - `.../Week_03/Function_02/historical_query.txt`:
    `4bab8c9ee83be8864b58e2e8920cb5a31beae2257d6a2f07af3ceefca73c975d`
  - `.../Week_03/Function_02/historical_output.txt`:
    `2431a5a47309afbf581251faaa1673f2cd23a8b3bb220d8a9d52c74f051dc93e`
  - `.../Week_04/Function_02/02_Data/cumulative_inputs.npy` (new):
    `5d8492b0a72a2871d4316b6e285a751e890b4e07abf91f759002d75f869ff278`
  - `.../Week_04/Function_02/02_Data/cumulative_outputs.npy` (new):
    `a11a3aceca09b7befe30b28bf5646646c885253b1ae1aeb9ad16b4d9f7069de6`

### Function_03

- Prior shape: `(17, 3)` → final `(18, 3)`. Outputs: `(17,)` → `(18,)`.
  Matches required row count **18**.
- Prior 17 rows byte-identical: **PASS**. Final row exact match — query
  `[0.670026, 0.057881, 0.658241]`, output `-0.18323876643005035`: **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.046809`, max `0.998464`: **PASS**.
- Duplicate check: no duplicates among 18 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_03/Function_03/02_Data/cumulative_inputs.npy`:
    `c098826b1d906d28882818166b920a63b461a33d4c3e00d2f8ca08df27786cba`
  - `.../Week_03/Function_03/02_Data/cumulative_outputs.npy`:
    `b9bc22cc2605f74879ceda746c334263421e3168504087854dee0a338efd448c`
  - `.../Week_03/Function_03/historical_query.txt`:
    `0d5b91f7bc927a328e4f273cbef412c15eaa6d85cc479248935a39edf1991c07`
  - `.../Week_03/Function_03/historical_output.txt`:
    `c2f33dbce3b3c69abb2fc1a275f8d65d088634093b539029e5a33a7f022a061b`
  - `.../Week_04/Function_03/02_Data/cumulative_inputs.npy` (new):
    `123b59db93a0dbc502f9d2f726b0db561f48decf8419285d2e7be1cf7edcf819`
  - `.../Week_04/Function_03/02_Data/cumulative_outputs.npy` (new):
    `4b0bc580281b8ca1c4f4b08c7299bcd4d4a7fd0d003b5cff47ba2ca942cd8991`

### Function_04

- Prior shape: `(32, 4)` → final `(33, 4)`. Outputs: `(32,)` → `(33,)`.
  Matches required row count **33**.
- Prior 32 rows byte-identical: **PASS**. Final row exact match — query
  `[0.394519, 0.361122, 0.256803, 0.461856]`, output `-1.9810750402526334`:
  **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.006250`, max `0.999483`: **PASS**.
- Duplicate check: no duplicates among 33 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_03/Function_04/02_Data/cumulative_inputs.npy`:
    `812864e2ff52ca68abb8e77dea522ea7b03595a976578ea6fa7142f5d5dc3656`
  - `.../Week_03/Function_04/02_Data/cumulative_outputs.npy`:
    `629b93581cc7b2fa68a504c5f0d5e3b7f80640ba80b9a4bd965d05b8448eae45`
  - `.../Week_03/Function_04/historical_query.txt`:
    `40c960601896c239e92a6801be751caa3278b988c3a16d368335302b52dcd354`
  - `.../Week_03/Function_04/historical_output.txt`:
    `dd7728a223b9e63cad2df8cfb62ee33815263c5554775477fa889b51924c0baa`
  - `.../Week_04/Function_04/02_Data/cumulative_inputs.npy` (new):
    `087a8511ac49c9a6a2812e3d01d0795f89612a408eb1424823c0ee5c004f1e4b`
  - `.../Week_04/Function_04/02_Data/cumulative_outputs.npy` (new):
    `03583df8513990f078f77f32bf545cb238ca07226ff5a612474a03a5d172e4dd`

### Function_05

- Prior shape: `(22, 4)` → final `(23, 4)`. Outputs: `(22,)` → `(23,)`.
  Matches required row count **23**.
- Prior 22 rows byte-identical: **PASS**. Final row exact match — query
  `[0.255842, 0.841692, 0.888985, 0.860266]`, output `1035.6647479285914`:
  **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.038193`, max `0.957644`: **PASS**.
- Duplicate check: no duplicates among 23 rows: **PASS** (this includes a
  check against the already-flagged near-duplicate rows for this function
  — the newly appended row remains distinct at 6-decimal rounding from the
  closest existing row, distance ≈6.16e-06, three of four coordinates
  differ at the 6th decimal place).
- Source paths and SHA-256:
  - `.../Week_03/Function_05/02_Data/cumulative_inputs.npy`:
    `aa4a3e3e61bef2f9d3b4bbb726929f9116e304761b83ba9c7a3653f59294d48c`
  - `.../Week_03/Function_05/02_Data/cumulative_outputs.npy`:
    `a771e7d88d4d54828b174d1f6acc55ac001182f23cf31e27bda9c29fd54f1977`
  - `.../Week_03/Function_05/historical_query.txt`:
    `ed596394cf42557f6863a1ffbbcfc1aca9c5326167b65665566b45683ec7ad4f`
  - `.../Week_03/Function_05/historical_output.txt`:
    `b05a87ac9feecc18fabd898b5d4b77c89b71c3d6ac4614763a812fe28c3e4fea`
  - `.../Week_04/Function_05/02_Data/cumulative_inputs.npy` (new):
    `e470c824a872d1a18e19187120bd163a8d4eca66d8410bf9b740643a75dbf8b2`
  - `.../Week_04/Function_05/02_Data/cumulative_outputs.npy` (new):
    `382f69bab7983bded9acc28875f9daa2e273760731bc06df23972e6274563314`

### Function_06

- Prior shape: `(22, 5)` → final `(23, 5)`. Outputs: `(22,)` → `(23,)`.
  Matches required row count **23**.
- Prior 22 rows byte-identical: **PASS**. Final row exact match — query
  `[0.495070, 0.097152, 0.672921, 0.000007, 0.351268]`, output
  `-1.339542402620705`: **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.000007` (confirmed ≥0, genuine), max `0.978806`: **PASS**.
- Duplicate check: no duplicates among 23 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_03/Function_06/02_Data/cumulative_inputs.npy`:
    `37fe82ae3794d7a8dde4d3f767b717addf87c28f8725ef3b648d6b86ce40fe9c`
  - `.../Week_03/Function_06/02_Data/cumulative_outputs.npy`:
    `8ab324cf6bb6fde17be96b890d0981ca86adfaac37d01ff2ad9df678c2bfea30`
  - `.../Week_03/Function_06/historical_query.txt`:
    `2d3c9f8374f9adc88d53a30dc05bcc46524de1cb87697dfa8fb3175cf2e10e62`
  - `.../Week_03/Function_06/historical_output.txt`:
    `738b0920621162f8e9b778d87be259859faa6039b800fe3ecbffdc34898a7036`
  - `.../Week_04/Function_06/02_Data/cumulative_inputs.npy` (new):
    `ce75e214a823ecc1697fe8e4be20a769b338d813b606c2cd15761b2a362f87cf`
  - `.../Week_04/Function_06/02_Data/cumulative_outputs.npy` (new):
    `40dcf1d32f027c4fe90242126b38c601c3704c6638e399ce6d1a45e74fb061a6`

### Function_07

- Prior shape: `(32, 6)` → final `(33, 6)`. Outputs: `(32,)` → `(33,)`.
  Matches required row count **33**.
- Prior 32 rows byte-identical: **PASS**. Final row exact match — query
  `[0.143585, 0.302559, 0.571101, 0.194533, 0.395561, 0.815792]`, output
  `2.149905456773691`: **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.003635`, max `0.998655`: **PASS**.
- Duplicate check: no duplicates among 33 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_03/Function_07/02_Data/cumulative_inputs.npy`:
    `d8001097512ac61a051d9cbc85c51f1f3df8904f0eb69acc656277a81486d61c`
  - `.../Week_03/Function_07/02_Data/cumulative_outputs.npy`:
    `20f043d762991acbd359627957f489c0ca5136d3fc12971f2c7a7ed54fd92ea7`
  - `.../Week_03/Function_07/historical_query.txt`:
    `272bbca08a0d41db416e5eb63a2ad48589cefc53e6579f38c470782f4f171c5b`
  - `.../Week_03/Function_07/historical_output.txt`:
    `5e876c4c1194b9a17ccbfbfcc51a5f4e7dc14921d8600a3aa39bed103f5d72b6`
  - `.../Week_04/Function_07/02_Data/cumulative_inputs.npy` (new):
    `2444603c07f137264ec2ecc8a97ddd10731941d103bfd633b67b7d5e626b2503`
  - `.../Week_04/Function_07/02_Data/cumulative_outputs.npy` (new):
    `4b1f10aec2961eaf1030ff4997777631b1d04d77bfe8aa79e43bc598d98a9276`

### Function_08

- Prior shape: `(42, 8)` → final `(43, 8)`. Outputs: `(42,)` → `(43,)`.
  Matches required row count **43**.
- Prior 42 rows byte-identical: **PASS**. Final row exact match — query
  `[0.088894, 0.525132, 0.030986, 0.992538, 0.870819, 0.217060, 0.011239, 0.447691]`,
  output `8.9636037557014`: **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.003419`, max `0.999322`: **PASS**.
- Duplicate check: no duplicates among 43 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_03/Function_08/02_Data/cumulative_inputs.npy`:
    `ac4a99ab2d7645df82f1ad2118837d24f08b9b7dc3dc34b623d5d811058c9482`
  - `.../Week_03/Function_08/02_Data/cumulative_outputs.npy`:
    `c702965d4842478d80c1314515614627f52e4604e8257ef332fb889db738a732`
  - `.../Week_03/Function_08/historical_query.txt`:
    `be7abedea65f7ccc9c4dd81de923a8eb859ca8952a02924242697ae9d7198ff1`
  - `.../Week_03/Function_08/historical_output.txt`:
    `4c99408ddcac4d91d757bcfc91e47176309ee3fc0a20dcc827b07068df37cbc7`
  - `.../Week_04/Function_08/02_Data/cumulative_inputs.npy` (new):
    `583653ee66402b544b02b6cf87f911ebf19d5e971b090f80cc83d25e007a9dd2`
  - `.../Week_04/Function_08/02_Data/cumulative_outputs.npy` (new):
    `3ae50165a3fec887fe3f5a02ec9bcbb19b90aeac8455c52bddea679ebf27ae90`

## Summary Table

| Function | Prior (Week 3) shape | Final (Week 4) shape | Required rows | Prior-identical | Final-row match | Dtype | Finite | Bounds | Duplicates | Overall |
|---|---|---|---|---|---|---|---|---|---|---|
| 01 | (12,2)/(12,) | (13,2)/(13,) | 13 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 02 | (12,2)/(12,) | (13,2)/(13,) | 13 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 03 | (17,3)/(17,) | (18,3)/(18,) | 18 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 04 | (32,4)/(32,) | (33,4)/(33,) | 33 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 05 | (22,4)/(22,) | (23,4)/(23,) | 23 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 06 | (22,5)/(22,) | (23,5)/(23,) | 23 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 07 | (32,6)/(32,) | (33,6)/(33,) | 33 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 08 | (42,8)/(42,) | (43,8)/(43,) | 43 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |

**8 of 8 functions: PASS. No function required stopping — every check
passed for every function on the first attempt, and every required Week 4
row count (13, 13, 18, 33, 23, 23, 33, 43) matched exactly.**

## Confirmations

- **Week 3 cumulative datasets unchanged**: all 16
  `Historical_Replay/Week_03/Function_0X/02_Data/cumulative_{inputs,outputs}.npy`
  checksums recomputed and confirmed identical to their previously-recorded
  values.
- **Week 3 historical pair files unchanged**: all 16
  `Historical_Replay/Week_03/Function_0X/historical_{query,output}.txt`
  checksums recomputed and confirmed identical to their previously-recorded
  values.
- **`Week_01`, `Week_02` (rebuild and Historical Replay), and the original
  source project unchanged**: confirmed via `git status` — no tracked file
  outside the new `Historical_Replay/Week_04/` directory was modified.
- **No Week 04 historical query or output was imported, revealed, or
  used.** Only the already-verified Week 3 pair was appended.
- **No Week 04 notebook was created.** No model was fit and no new query
  was generated at this step.
- **No value was fabricated, interpolated, or guessed** — every number in
  the Week 4 cumulative arrays traces directly to an already-verified
  source file.

## Final Verdict

# **PASS — 8/8 Historical Replay Week_04 starting datasets created and validated**
