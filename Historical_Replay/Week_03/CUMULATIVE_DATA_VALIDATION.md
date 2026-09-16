# Cumulative Data Validation — Historical Replay Week_03 Starting Datasets

**Scope:** This validates the creation of `Historical_Replay/Week_03/Function_0X/02_Data/
cumulative_inputs.npy` and `cumulative_outputs.npy` for Function_01 through
Function_08. Each array was built by appending exactly one row/element — the
independently-verified genuine historical Week 2 query-output pair from
`Historical_Replay/Week_02/Function_0X/historical_query.txt`/
`historical_output.txt` — to that function's existing Week 2 cumulative
dataset (`Historical_Replay/Week_02/Function_0X/02_Data/cumulative_*.npy`).
No value was fabricated, interpolated, or guessed; every number comes
directly from an already-verified source file.

**No Week 03 historical query or output has been imported, revealed, or
used anywhere in this step.** Only the already-verified Week 2 pair was
appended. No Week 03 notebook was created. No new query was generated and
no model was fit. `Week_01`, `Week_02` (of both the rebuild and the
Historical Replay track), and the original source project were not
modified.

## Method

For each function: the existing `Historical_Replay/Week_02/Function_0X/
02_Data/cumulative_{inputs,outputs}.npy` arrays were loaded with `np.load`;
`Historical_Replay/Week_02/Function_0X/historical_{query,output}.txt` were
parsed into a float64 vector and scalar; the new Week 3 arrays were built
via `np.vstack`/`np.append`; all validation checks below were computed
programmatically — not asserted by inspection — before any file was
written, per the "stop rather than guess" instruction. All 8 functions
passed every check on the first attempt; no function required stopping.

## Per-Function Validation Results

### Function_01

- Prior (Week 2) shape: `(11, 2)` → final (Week 3) shape: `(12, 2)`.
  Outputs: `(11,)` → `(12,)`. Matches the required row count of **12**.
- All 11 prior rows/elements are byte-identical (`np.array_equal`) to the
  Week 2 cumulative arrays, in original order: **PASS**.
- Final row (index 11) exactly matches the verified Week 2 historical pair
  — query `[0.536429, 0.835362]`, output `1.674933466363685e-36`: **PASS**.
- Input/output observation counts match: 12 = 12. **PASS**.
- Data types: `float64` throughout: **PASS**.
- Finite-value check: no NaN/Inf anywhere: **PASS**.
- Bounds check: min `0.078723`, max `0.950714`, within
  `[0.000000, 0.999999]`: **PASS**.
- Duplicate-input check: all 12 rows unique when rounded to 6 decimal
  places: **PASS**.
- Source paths and SHA-256:
  - `Historical_Replay/Week_02/Function_01/02_Data/cumulative_inputs.npy`:
    `b78536d083aa66e42f21c99cf71dd5e9ed1a71b0f2d6a039095bbf2674e013c9`
  - `Historical_Replay/Week_02/Function_01/02_Data/cumulative_outputs.npy`:
    `d31f1155689179426de763a750f701283b6d4d09216b7840b6c6b70f947916b3`
  - `Historical_Replay/Week_02/Function_01/historical_query.txt`:
    `d2d403cab4d6c79abcb03e9595f04f5c5d5f2bba0cb7f2108a5ca10285ef1a23`
  - `Historical_Replay/Week_02/Function_01/historical_output.txt`:
    `86a7982fac5cfe7aa59f89cef5082d7ce6b87ee4e4728cea0589794f4e8e89b3`
  - `Historical_Replay/Week_03/Function_01/02_Data/cumulative_inputs.npy`
    (new): `e30d9c1adef2752f59ba2e4a6005c439ff23867b40bfa3776935fdfd937949ea`
  - `Historical_Replay/Week_03/Function_01/02_Data/cumulative_outputs.npy`
    (new): `f53221c8c178cf73aae46a6d27ff88a6e7a4733a222ea240a21f1bc1469177d1`

### Function_02

- Prior shape: `(11, 2)` → final `(12, 2)`. Outputs: `(11,)` → `(12,)`.
  Matches required row count **12**.
- Prior 11 rows byte-identical: **PASS**. Final row exact match — query
  `[0.978752, 0.932731]`, output `0.022666631114895516`: **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.028698`, max `0.978752`: **PASS**.
- Duplicate check: no duplicates among 12 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_02/Function_02/02_Data/cumulative_inputs.npy`:
    `2e953eaaa0946216b8eb079b4affafbd2eded497976917443915127fb742daf7`
  - `.../Week_02/Function_02/02_Data/cumulative_outputs.npy`:
    `756099c0a6250ff407ef0c6ea69a78a42ab7aad545bb15875a6c2e3f53b20a87`
  - `.../Week_02/Function_02/historical_query.txt`:
    `5ce8ae2538ecd730c2bd1ce821ef4789e6d6c2c6174e176b6958ebafde7742cf`
  - `.../Week_02/Function_02/historical_output.txt`:
    `e93554e9d0a85db5a996f12810328af0369686d5186fdd00c9267e33b66f3ea7`
  - `.../Week_03/Function_02/02_Data/cumulative_inputs.npy` (new):
    `ddb20064e337cc47b0c912fdbf5e1b757c19a842de5ff13e17416bb64d42a675`
  - `.../Week_03/Function_02/02_Data/cumulative_outputs.npy` (new):
    `9748449ec40362fdc4a8fdac4de6cf213f9342d6c05dd9180360c0dd3bd58b0d`

### Function_03

- Prior shape: `(16, 3)` → final `(17, 3)`. Outputs: `(16,)` → `(17,)`.
  Matches required row count **17**.
- Prior 16 rows byte-identical: **PASS**. Final row exact match — query
  `[0.657452, 0.998464, 0.817253]`, output `-0.08987474979637637`: **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.046809`, max `0.998464`: **PASS**.
- Duplicate check: no duplicates among 17 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_02/Function_03/02_Data/cumulative_inputs.npy`:
    `3d2f4d29ffe34ec15629fdfe8c579506d16a0c6377506abf2d7f3aa5c7235e1d`
  - `.../Week_02/Function_03/02_Data/cumulative_outputs.npy`:
    `db06a53358bb2c76e7fb2cbf6200138abfee13b095886ab64cf1110e4cbcfb81`
  - `.../Week_02/Function_03/historical_query.txt`:
    `ae69b8ff4caecfe1e467cb47eeca5698cf73cd678fd2a8b02bb3419359a89d0f`
  - `.../Week_02/Function_03/historical_output.txt`:
    `4eef687aa3f5b67062002f5700df3596d094e6d339b111b8cda40d2a5d5efa30`
  - `.../Week_03/Function_03/02_Data/cumulative_inputs.npy` (new):
    `c098826b1d906d28882818166b920a63b461a33d4c3e00d2f8ca08df27786cba`
  - `.../Week_03/Function_03/02_Data/cumulative_outputs.npy` (new):
    `b9bc22cc2605f74879ceda746c334263421e3168504087854dee0a338efd448c`

### Function_04

- Prior shape: `(31, 4)` → final `(32, 4)`. Outputs: `(31,)` → `(32,)`.
  Matches required row count **32**.
- Prior 31 rows byte-identical: **PASS**. Final row exact match — query
  `[0.006280, 0.281840, 0.932013, 0.960202]`, output `-31.73535839431216`:
  **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.006250`, max `0.999483`: **PASS**.
- Duplicate check: no duplicates among 32 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_02/Function_04/02_Data/cumulative_inputs.npy`:
    `396b567c1b58c21c8f926a71615a24ebb9d1597604cba8de618f14092293b1b6`
  - `.../Week_02/Function_04/02_Data/cumulative_outputs.npy`:
    `ac3d0aff8947f5a71af10481defb1a66c83b3aa2de6d411f55d28a4cd2d00a03`
  - `.../Week_02/Function_04/historical_query.txt`:
    `ff839dd3f9bb4cd7e0014b3689a3fc8eef4eb8fc27202dfd1f1cdff1cd1faa20`
  - `.../Week_02/Function_04/historical_output.txt`:
    `2368dc2c2f9c6f9521bd01cffddd1b854f3ad402bf7bebbba90c38b398b9e854`
  - `.../Week_03/Function_04/02_Data/cumulative_inputs.npy` (new):
    `812864e2ff52ca68abb8e77dea522ea7b03595a976578ea6fa7142f5d5dc3656`
  - `.../Week_03/Function_04/02_Data/cumulative_outputs.npy` (new):
    `629b93581cc7b2fa68a504c5f0d5e3b7f80640ba80b9a4bd965d05b8448eae45`

### Function_05

- Prior shape: `(21, 4)` → final `(22, 4)`. Outputs: `(21,)` → `(22,)`.
  Matches required row count **22**.
- Prior 21 rows byte-identical: **PASS**. Final row exact match — query
  `[0.255841, 0.841692, 0.888984, 0.860260]`, output `1035.6341457754475`:
  **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.038193`, max `0.957644`: **PASS**.
- Duplicate check: no duplicates among 22 rows: **PASS** (this includes a
  check against the near-duplicate row already flagged for this function in
  `Historical_Replay/Week_02/INDEPENDENT_CUMULATIVE_REVIEW.md` — the newly
  appended Week 2 row remains distinct at 6-decimal rounding from that
  earlier near-duplicate, as it was in Week 2).
- Source paths and SHA-256:
  - `.../Week_02/Function_05/02_Data/cumulative_inputs.npy`:
    `c0664ced172d7d8642ccbc902308177d2b04dbf847912d6d20fd116b340ecbb9`
  - `.../Week_02/Function_05/02_Data/cumulative_outputs.npy`:
    `7751a3bb3d95d75d67aa87e58c244b26059c58bf93fb27cc3f74d81804bdc110`
  - `.../Week_02/Function_05/historical_query.txt`:
    `87365228384852a9378b01fe09f08bcd6c561c729d13b2f515c3c6d455d0e96e`
  - `.../Week_02/Function_05/historical_output.txt`:
    `cf53f0420a42ba4b59241b78b3bc22d37e078393f92a0a738d53927551d03b31`
  - `.../Week_03/Function_05/02_Data/cumulative_inputs.npy` (new):
    `aa4a3e3e61bef2f9d3b4bbb726929f9116e304761b83ba9c7a3653f59294d48c`
  - `.../Week_03/Function_05/02_Data/cumulative_outputs.npy` (new):
    `a771e7d88d4d54828b174d1f6acc55ac001182f23cf31e27bda9c29fd54f1977`

### Function_06

- Prior shape: `(21, 5)` → final `(22, 5)`. Outputs: `(21,)` → `(22,)`.
  Matches required row count **22**.
- Prior 21 rows byte-identical: **PASS**. Final row exact match — query
  `[0.433654, 0.486678, 0.282753, 0.972905, 0.323321]`, output
  `-0.8782405650300305`: **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.004911`, max `0.978806`: **PASS**.
- Duplicate check: no duplicates among 22 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_02/Function_06/02_Data/cumulative_inputs.npy`:
    `a02bebc15427f119e5beccef317ea78bde0e71243dc1435ef29f2caa08ac0753`
  - `.../Week_02/Function_06/02_Data/cumulative_outputs.npy`:
    `beca42746dcbd38b7151442654519872fb27f57fe51cc35aa29c818ded48b2e0`
  - `.../Week_02/Function_06/historical_query.txt`:
    `0cd4b48084b1c1f39db89ed5e521d879082b1407b916031198775a1a23667d8a`
  - `.../Week_02/Function_06/historical_output.txt`:
    `c269936ea60d2b976436105a20792cc8537e36f6f006970dd86db54ec13f704f`
  - `.../Week_03/Function_06/02_Data/cumulative_inputs.npy` (new):
    `37fe82ae3794d7a8dde4d3f767b717addf87c28f8725ef3b648d6b86ce40fe9c`
  - `.../Week_03/Function_06/02_Data/cumulative_outputs.npy` (new):
    `8ab324cf6bb6fde17be96b890d0981ca86adfaac37d01ff2ad9df678c2bfea30`

### Function_07

- Prior shape: `(31, 6)` → final `(32, 6)`. Outputs: `(31,)` → `(32,)`.
  Matches required row count **32**.
- Prior 31 rows byte-identical: **PASS**. Final row exact match — query
  `[0.243848, 0.900100, 0.696978, 0.193630, 0.374149, 0.826324]`, output
  `0.308765180253091`: **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.003635`, max `0.998655`: **PASS**.
- Duplicate check: no duplicates among 32 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_02/Function_07/02_Data/cumulative_inputs.npy`:
    `462dafc9baeb7551b145ada8fd988d556e2c0311237b07ffd6b6b0a1e7e96e85`
  - `.../Week_02/Function_07/02_Data/cumulative_outputs.npy`:
    `f7e12fe5d14292a998353cdfc3c22831113d1efeed139fab1ac1684ee6712765`
  - `.../Week_02/Function_07/historical_query.txt`:
    `ad7a64ebdbac46db18c1276f0a1213f176a2d235c195bad087c58ba64fce7392`
  - `.../Week_02/Function_07/historical_output.txt`:
    `0d1dd43a4549988360edd2cfe6cf6a496ca711eccdc233c0deaabf37072b0f20`
  - `.../Week_03/Function_07/02_Data/cumulative_inputs.npy` (new):
    `d8001097512ac61a051d9cbc85c51f1f3df8904f0eb69acc656277a81486d61c`
  - `.../Week_03/Function_07/02_Data/cumulative_outputs.npy` (new):
    `20f043d762991acbd359627957f489c0ca5136d3fc12971f2c7a7ed54fd92ea7`

### Function_08

- Prior shape: `(41, 8)` → final `(42, 8)`. Outputs: `(41,)` → `(42,)`.
  Matches required row count **42**.
- Prior 41 rows byte-identical: **PASS**. Final row exact match — query
  `[0.163160, 0.184786, 0.152644, 0.083802, 0.999322, 0.544113, 0.184124, 0.123846]`,
  output `9.9399041910574`: **PASS**.
- Counts match: **PASS**. Dtype `float64`: **PASS**. Finite: **PASS**.
- Bounds: min `0.003419`, max `0.999322`: **PASS**.
- Duplicate check: no duplicates among 42 rows: **PASS**.
- Source paths and SHA-256:
  - `.../Week_02/Function_08/02_Data/cumulative_inputs.npy`:
    `3c3e6adddc6ed389b1b1b2e8b67771b5424d402f249288ac8c85707cb5f7f479`
  - `.../Week_02/Function_08/02_Data/cumulative_outputs.npy`:
    `72867d027b9bb995bb93ad1235563552e00ae0643b93552fe801d5a91ae23cb1`
  - `.../Week_02/Function_08/historical_query.txt`:
    `6a7dc1891093c5bf7f2679e8b3caf23dca4f28dd386079f290023eb22d086781`
  - `.../Week_02/Function_08/historical_output.txt`:
    `ffe242a05f991754a5db86dd86d778ae3709a3f30e568dab200df240511dccec`
  - `.../Week_03/Function_08/02_Data/cumulative_inputs.npy` (new):
    `ac4a99ab2d7645df82f1ad2118837d24f08b9b7dc3dc34b623d5d811058c9482`
  - `.../Week_03/Function_08/02_Data/cumulative_outputs.npy` (new):
    `c702965d4842478d80c1314515614627f52e4604e8257ef332fb889db738a732`

## Summary Table

| Function | Prior (Week 2) shape | Final (Week 3) shape | Required rows | Prior-identical | Final-row match | Dtype | Finite | Bounds | Duplicates | Overall |
|---|---|---|---|---|---|---|---|---|---|---|
| 01 | (11,2)/(11,) | (12,2)/(12,) | 12 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 02 | (11,2)/(11,) | (12,2)/(12,) | 12 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 03 | (16,3)/(16,) | (17,3)/(17,) | 17 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 04 | (31,4)/(31,) | (32,4)/(32,) | 32 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 05 | (21,4)/(21,) | (22,4)/(22,) | 22 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 06 | (21,5)/(21,) | (22,5)/(22,) | 22 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 07 | (31,6)/(31,) | (32,6)/(32,) | 32 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |
| 08 | (41,8)/(41,) | (42,8)/(42,) | 42 | PASS | PASS | PASS | PASS | PASS | PASS | **PASS** |

**8 of 8 functions: PASS. No function required stopping — every check
passed for every function on the first attempt, and every required Week 3
row count (12, 12, 17, 32, 22, 22, 32, 42) matched exactly.**

## Confirmations

- **Week 2 cumulative datasets unchanged**: all 16
  `Historical_Replay/Week_02/Function_0X/02_Data/cumulative_{inputs,outputs}.npy`
  checksums recomputed and confirmed identical to their previously-recorded
  values.
- **Week 2 historical pair files unchanged**: all 16
  `Historical_Replay/Week_02/Function_0X/historical_{query,output}.txt`
  checksums recomputed and confirmed identical to their previously-recorded
  values.
- **`Week_01` (rebuild) and the original source project unchanged**:
  confirmed via `git status` — no tracked file outside the new
  `Historical_Replay/Week_03/` directory was modified.
- **No Week 03 historical query or output was imported, revealed, or
  used.** Only the already-verified Week 2 pair was appended.
- **No Week 03 notebook was created.** No model was fit and no new query
  was generated at this step.
- **No value was fabricated, interpolated, or guessed** — every number in
  the Week 3 cumulative arrays traces directly to an already-verified
  source file.

## Final Verdict

# **PASS — 8/8 Historical Replay Week_03 starting datasets created and validated**
