# Cumulative Data Validation — Week_07

**Scope:** Documents the construction of each function's Week_07 starting
cumulative dataset by appending exactly one row — that function's own
genuine Week_06 historical query-output pair — to its Week_06 cumulative
dataset. This mirrors the same append procedure used for every prior
week.

**Important — this is not affected by Function_05's known Week_07
duplicate.** `HISTORICAL_REPLAY_PROTOCOL.md` Rule 9 requires that
Function_05's own **Week_07** historical query-output pair (to be
imported in a separate, later step of this week's build-verify loop) not
be appended twice, since it duplicates its Week_04 pair
(`[0,0,0,0] → 163.1225`). This document covers a different, earlier step
— appending the already-verified **Week_06** pair — which is confirmed
distinct from that duplicate (Function_05's Week_06 query is
`[0.654981, 0.602894, 0.898891, 0.589622]`, not `[0,0,0,0]`). Rule 9 will
be honoured explicitly when Week_07's own historical pair import is
performed later in this week's loop.

**Pre-verification:** the Data Quality Agent independently audited this
exact proposed operation *before* it was performed (row counts,
dimensionality, bounds, duplicate-row check, finiteness) — PASS for all 8
functions, and explicitly confirmed this append is unaffected by the
Rule 9 concern. The build script was then run and its own output
independently confirms the same row counts and hashes.

**Build method:** a standalone Python script
(`build_week07_cumulative.py`, run from outside the session scratchpad
after the same coincidental `inspect.py` name-collision noted in
`Historical_Replay/Week_06/CUMULATIVE_DATA_VALIDATION.md` was encountered
again — a pre-existing scratchpad artifact, not a code defect) loaded
each function's Week_06 `cumulative_inputs.npy`/`cumulative_outputs.npy`,
parsed that function's own
`Week_06/Function_0X/historical_query.txt`/`historical_output.txt`,
asserted dimensionality/bounds/finiteness/non-duplication, appended the
single new row via `np.vstack`/`np.append`, and saved the result to
`Week_07/Function_0X/02_Data/`. This avoids hand-transcribing large
arrays.

## Per-Function Results

| Function | Week_06 rows | Week_07 rows | Dim | Appended query | Appended output |
|---|---|---|---|---|---|
| Function_01 | 15 | **16** | 2 | `0.222521-0.999673` | `6.131855211153796e-211` |
| Function_02 | 15 | **16** | 2 | `0.473151-0.950706` | `0.16053152307795218` |
| Function_03 | 20 | **21** | 3 | `0.418776-0.420613-0.695421` | `-0.11700517654347184` |
| Function_04 | 35 | **36** | 4 | `0.753422-0.441826-0.942545-0.489846` | `-20.879363209082445` |
| Function_05 | 25 | **26** | 4 | `0.654981-0.602894-0.898891-0.589622` | `281.9115728306016` |
| Function_06 | 25 | **26** | 5 | `0.781770-0.192076-0.805757-0.695733-0.565940` | `-1.3125396070217887` |
| Function_07 | 35 | **36** | 6 | `0.042873-0.466831-0.379623-0.207090-0.367754-0.515646` | `1.611967464642638` |
| Function_08 | 45 | **46** | 8 | `0.115956-0.821931-0.587418-0.022809-0.440260-0.965550-0.368459-0.301774` | `8.5570343323444` |

All appended values are the exact genuine Week_06 historical pairs
already independently verified twice (`HISTORICAL_IMPORT_VALIDATION.md`
and `INDEPENDENT_HISTORICAL_REVIEW.md`, both Week_06, both PASS) — no new
verification of the raw values was performed here; this step only
verifies the *append operation itself*.

## SHA-256 Checksums

| Function | Week_06 inputs (unchanged) | Week_06 outputs (unchanged) | Week_07 inputs (new) | Week_07 outputs (new) |
|---|---|---|---|---|
| Function_01 | `5ed53e39990ed4b078d853fd936d286bda7e5c2378becdd8777bcd2bad075224` | `8e926aa89fb796f65745cbfdb8638393e0639053a00bcdad3b2ae50e10c1afac` | `17a670685b8a3b6f840f84115b7597c6ddf91c403d130ffe28abd0029f8635dc` | `79b3da37a01419ea46cb9321f050ce0ee4e24641a846724d7bee178013028cf7` |
| Function_02 | `030d394b19daf433e1b4c66d0536bb9ba2e80fc66b41f4b6c99bef20096f4769` | `dd81d9bd978b763ceb6d6ab1da1e3265022322df2b23e71dbf0c49ad64f7301c` | `a56b0c7fd196f8c7cb410bec3692e10769c9443f5a46898c1e18bc3238d24f75` | `97af680d1839e81e96015c5306074d8e9ff1878ece353c2d75cc5b3a2941b0c1` |
| Function_03 | `f04dae02a42b33f1d91814ebd1dee576dd4c7f9f189781ffdca83ed94dede8f0` | `1e18d9f79250767b63e17c77e49235104302a0e6faef2ec04e58d23a2ae23ad4` | `749e248710e70eacd765518a0ac45172715a19a543f4d92a167fe161f5a1151a` | `044d568bd07d5ae3fdc2c582e595188501d17026d2179c7214b18531b5299acb` |
| Function_04 | `327900be83ad285f4695a67dda92d8c001b14c2e99ef113ca68fa910cc62b8de` | `1d078b5ea42af2e1a7d3a4bd444cab406ceb81bf78b8404ebdae15a9d7a69a41` | `dc9d39b79b2321d1db357a3f78e9020fb702e7c8e016c21ee935e4282d2bcdb0` | `0e0418e88fbaac3a7bfba49c5c3245212d848302433d694f16942244c4f84061` |
| Function_05 | `a97c8320639b011a4c6616a4d7d649e523d379e0d03c1ea9f3c0b41dd8bb9518` | `2a481fab3c054317530a95679026f1f1d19d01fa2f088a02f9e1fe5f2b435bcc` | `a7fc701f9f9ed3537ccf09a69423c02be3a093beedb462fc2d3ad7ec8aa66aa6` | `1072c5b0359581bb8b0ae435c83e1f2ced3f26bd766f8ba8c34379d0a170dbd3` |
| Function_06 | `eb312e17d1689a2e78bb3c2e8f24a2fdfbf05f9bf9baa6cd0dde0777b8e2b233` | `71e11937a7262a3da8db526a0446ad58ffb0f8f28189cc8a8868004ee46655ec` | `15cfdf4ebb210d69d47629d32c62cf955690590a3a01c70714aaea62c407ac93` | `f1a1ddc08e494fb1ca1704d61fa836dd36e37acc5b5fed21f829a829009bad6e` |
| Function_07 | `df08161ab5f664568e1fa011ac174cffc53e4eb57fb9650cb0189517619afa93` | `0d83fa518524e51ce4fe8677ba7e919f52d6e6688675233973ba149132ee95ea` | `a6adba80fc7ddc3a28232f9a221731a98533d8bc99810d6568e504d8f03b54db` | `6e175e52e5fa61fb390c2f32914d63e25aafe826cb1780cb1efde4c8ae399f2a` |
| Function_08 | `0019897e4b284ae42e09e08867498a9a199a85f59bc774bd24b554d1376b5365` | `0e8e0bf5edd27833e1796210c7fb9cdedf2bd67486bc64b4bcd9d048ae171449` | `459801b7c14b254b9d8fb0aa1bed5715ead15af60e24e6e4f7cb10e4c3bed00b` | `6cefd514d61df6b150b2216d7bd06a97c69e23a9417c3ade9c756dc0df730833` |

The Week_06 checksums above were recomputed fresh (not copied from
`Historical_Replay/Week_06/CUMULATIVE_DATA_VALIDATION.md`) and matched
those on record exactly, confirming the Week_06 source files were
untouched by this operation.

## Verification Checks Performed (via the build script's own assertions)

For every function:
- **Row count** matches expected Week_06 count (15,15,20,35,25,25,35,45)
  before the append. **PASS** for all 8.
- **Dimensionality** of the new row matches the existing array's column
  count and the expected per-function dimensionality (2,2,3,4,4,5,6,8).
  **PASS** for all 8.
- **Bounds**: every coordinate of the new row lies in
  `[0.000000, 0.999999]` inclusive. **PASS** for all 8.
- **Finiteness**: new row and new output are finite. **PASS** for all 8.
- **No duplicate row**: the new row does not exactly match any existing
  row in that function's Week_06 cumulative_inputs.npy. **PASS** for all 8.
- **Resulting row count**: 16,16,21,36,26,26,36,46 for Functions 01–08.
  **PASS** for all 8, matching the pre-verification exactly.

## Scope Boundary

No Week_01–Week_06 file was modified. No file in the original source
project (`~/Documents/GitHub/My_Capstone_1_Imperial`) was read or modified
beyond the pre-existing Week_06 historical pair files already in this
rebuild project. No GP notebook, code review, or Week_07 historical pair
import has been performed yet — this document covers only the
cumulative-dataset append step. **Function_05's own Week_07 historical
pair import (the duplicate-of-Week_04 case per Rule 9) is a separate,
later step and has not been touched by this document.**

## Final Verdict

# **PASS — Week_07 cumulative datasets built for all 8 functions, byte-verified via SHA-256, no data-quality issues found**
