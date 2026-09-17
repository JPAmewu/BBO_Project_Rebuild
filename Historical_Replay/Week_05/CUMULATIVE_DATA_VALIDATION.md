# Cumulative Data Validation — Week_05

**Scope:** Documents the construction of each function's Week_05 starting
cumulative dataset by appending exactly one row — that function's own
genuine Week_04 historical query-output pair — to its Week_04 cumulative
dataset. This mirrors the same append procedure used for Week_02, Week_03,
and Week_04.

**Pre-verification:** the Data Quality Agent independently audited this
exact proposed operation *before* it was performed (row counts,
dimensionality, bounds, duplicate-row check, finiteness, and the
Function_05 duplicate-of-Week_07 caveat) — see the agent's report,
summarised below. The build script was then run and its own output
independently confirms the same row counts and hashes.

**Build method:** a standalone Python script
(`build_week05_cumulative.py`, run from the session scratchpad, not
committed to the project) loaded each function's Week_04
`cumulative_inputs.npy`/`cumulative_outputs.npy`, parsed that function's
own `Week_04/Function_0X/historical_query.txt`/`historical_output.txt`,
asserted dimensionality/bounds/finiteness/non-duplication, appended the
single new row via `np.vstack`/`np.append`, and saved the result to
`Week_05/Function_0X/02_Data/`. This avoids hand-transcribing large
arrays.

## Per-Function Results

| Function | Week_04 rows | Week_05 rows | Dim | Appended query | Appended output |
|---|---|---|---|---|---|
| Function_01 | 13 | **14** | 2 | `0.872626-0.321598` | `-2.4666412561217706e-107` |
| Function_02 | 13 | **14** | 2 | `0.197553-0.808683` | `0.038774938576018964` |
| Function_03 | 18 | **19** | 3 | `0.650132-0.218327-0.110542` | `-0.08251349582236739` |
| Function_04 | 33 | **34** | 4 | `0.715715-0.504442-0.275065-0.552962` | `-9.312811809021998` |
| Function_05 | 23 | **24** | 4 | `0.000000-0.000000-0.000000-0.000000` | `163.1225` |
| Function_06 | 23 | **24** | 5 | `0.973181-0.022807-0.714484-0.145453-0.974790` | `-2.4375082517089726` |
| Function_07 | 33 | **34** | 6 | `0.045103-0.016167-0.773003-0.156031-0.147073-0.616114` | `1.2087334449610816` |
| Function_08 | 43 | **44** | 8 | `0.356502-0.157574-0.172997-0.610561-0.106477-0.260297-0.411421-0.904740` | `9.2540644925525` |

All appended values are the exact genuine Week_04 historical pairs
already independently verified twice (`HISTORICAL_IMPORT_VALIDATION.md`
and `INDEPENDENT_HISTORICAL_REVIEW.md`, both Week_04, both PASS) — no new
verification of the raw values was performed here; this step only
verifies the *append operation itself*.

## SHA-256 Checksums

| Function | Week_04 inputs (unchanged) | Week_04 outputs (unchanged) | Week_05 inputs (new) | Week_05 outputs (new) |
|---|---|---|---|---|
| Function_01 | `f67d109bd12601b4fa619a4800ecdf09958c623fbce91508d698f40e3acf9ed4` | `d9f0cdcf01527b38d06bc2bcb01ea87f22ff5757e98ce6966db9dc6506278fd0` | `9110097247da3612a6d5c64ae34609943ee2b8856362bad23f5d0bbe71e98eb1` | `a8930be10ad9827edfd63e61664245e34357d93db259ade49536133d19485579` |
| Function_02 | `5d8492b0a72a2871d4316b6e285a751e890b4e07abf91f759002d75f869ff278` | `a11a3aceca09b7befe30b28bf5646646c885253b1ae1aeb9ad16b4d9f7069de6` | `520b9874af859d3fc42a7fc15aba7e84a837037d1434d34216c01946f68dc066` | `74ff0e77c7a9fb4cbd83ff4d0e37182221865987df0a6151cfb5f88db619e302` |
| Function_03 | `123b59db93a0dbc502f9d2f726b0db561f48decf8419285d2e7be1cf7edcf819` | `4b0bc580281b8ca1c4f4b08c7299bcd4d4a7fd0d003b5cff47ba2ca942cd8991` | `48dfed392f4a19079a9d2292c936bb5bffb543ec915ce740b0c17f6885b2150c` | `202d75a4f9e2ef76c3bc0719f260b5797caf3888c4299802ca89584ebcedb7a5` |
| Function_04 | `087a8511ac49c9a6a2812e3d01d0795f89612a408eb1424823c0ee5c004f1e4b` | `03583df8513990f078f77f32bf545cb238ca07226ff5a612474a03a5d172e4dd` | `625c5ae3518ce8e7ce5267393d42463990c5640fbdbce225d1f3d37ff3297071` | `6488bfc634247ba06bcf2019be8dcbe656e6407c5efa7155daedf1d42e9bf4d7` |
| Function_05 | `e470c824a872d1a18e19187120bd163a8d4eca66d8410bf9b740643a75dbf8b2` | `382f69bab7983bded9acc28875f9daa2e273760731bc06df23972e6274563314` | `4ced1a2c3249b4abff62a15242865872f590a653a7bc402737ba6bfb07d27df7` | `cb8d0a4dc8a40659dbf711c62dc4ed8233ce0c46db967a8d66200a405dc573f2` |
| Function_06 | `ce75e214a823ecc1697fe8e4be20a769b338d813b606c2cd15761b2a362f87cf` | `40dcf1d32f027c4fe90242126b38c601c3704c6638e399ce6d1a45e74fb061a6` | `cc2a64b5283e994c5f54d1e86c250a8c656e945259a6ea9e937ebf1b9720152e` | `67aa1fd091d741cd6a066818d853ae6012c8a682536718d6839c273372bbadbe` |
| Function_07 | `2444603c07f137264ec2ecc8a97ddd10731941d103bfd633b67b7d5e626b2503` | `4b1f10aec2961eaf1030ff4997777631b1d04d77bfe8aa79e43bc598d98a9276` | `0ce808ddce29e7f62572367233c49c88c234489f767529cd84174de232543f89` | `421df3d4758d2d01b8c47fa6a229e83d4d9a3b3300d7aca5ab6a633e15c13800` |
| Function_08 | `583653ee66402b544b02b6cf87f911ebf19d5e971b090f80cc83d25e007a9dd2` | `3ae50165a3fec887fe3f5a02ec9bcbb19b90aeac8455c52bddea679ebf27ae90` | `8e104d6555998e84079342ff82ff19b164a528e9d47cee5f2cc162ed841c145b` | `87b493af4dbf178ed035c5fcea2bccd7eabc1f425467cf5abbc0b69b9456d435` |

The Week_04 checksums above were recomputed fresh (not copied from
`Historical_Replay/Week_04/CUMULATIVE_DATA_VALIDATION.md`) and matched
those on record exactly, confirming the Week_04 source files were
untouched by this operation.

## Verification Checks Performed (via the build script's own assertions)

For every function:
- **Row count** matches expected Week_04 count (13,13,18,33,23,23,33,43)
  before the append. **PASS** for all 8.
- **Dimensionality** of the new row matches the existing array's column
  count and the expected per-function dimensionality (2,2,3,4,4,5,6,8).
  **PASS** for all 8.
- **Bounds**: every coordinate of the new row lies in
  `[0.000000, 0.999999]` inclusive (including Function_05's exact-zero
  coordinates). **PASS** for all 8.
- **Finiteness**: new row and new output are finite. **PASS** for all 8.
- **No duplicate row**: the new row does not exactly match any existing
  row in that function's Week_04 cumulative_inputs.npy. **PASS** for all 8
  (confirmed independently for Function_05: its existing 23 rows have a
  minimum coordinate of 0.038193, so the new all-zero row is genuinely
  new).
- **Resulting row count**: 14,14,19,34,24,24,34,44 for Functions 01–08.
  **PASS** for all 8, matching the pre-verification exactly.

## Function_05 Duplicate-of-Week_07 Note

This Week_05 dataset build appends **only** the Week_04 pair (the
original, non-duplicate occurrence, `duplicate_of` empty in the source
ledger). It does not touch, reference, or anticipate Week_07 in any way.
Per `HISTORICAL_REPLAY_PROTOCOL.md` Rule 9, when Week_07 is eventually
processed, its Function_05 pair (flagged `duplicate_of: week:4` in the
source ledger) must **not** be appended again as an independent
observation — this note is a forward reminder, not an action taken now.

## Scope Boundary

No Week_01/Week_02/Week_03/Week_04 file was modified. No file in the
original source project (`~/Documents/GitHub/My_Capstone_1_Imperial`) was
read or modified beyond the pre-existing Week_04 historical pair files
already in this rebuild project. No GP notebook, code review, or
Week_05 historical pair import has been performed yet — this document
covers only the cumulative-dataset append step.

## Final Verdict

# **PASS — Week_05 cumulative datasets built for all 8 functions, byte-verified via SHA-256, no data-quality issues found**
