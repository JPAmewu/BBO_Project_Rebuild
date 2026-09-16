# Week_01 Data Quality Summary — Functions 01–08

**Scope:** Consolidated, read-only audit of `initial_inputs.npy` and
`initial_outputs.npy` for all eight functions under
`Week_01/Function_XX/02_Data/`. Performed by the Data Quality Agent. No dataset
files were modified, renamed, deleted, or overwritten. No models were fitted and
no query points were recommended as part of this audit.

Full per-function detail is in each function's
`05_Documentation/data_quality_report.md`. This file summarizes findings across
functions.

**Legend:** **[VERIFIED]** = directly computed/observed from the data.
**[ASSUMPTION/INFERENCE]** = inferred from context (e.g. README text, analogy
across functions), not directly provable from the arrays alone.

## Per-Function Overview

| Function | Input shape | Output shape | Obs. | Dims | Dtype | Output range | Duplicates | NaN/Inf | Verdict |
|---|---|---|---|---|---|---|---|---|---|
| Function_01 | (10, 2) | (10,) | 10 | 2 | float64 | -0.0036 to 7.7e-16 | 0 | 0 / 0 | Clean |
| Function_02 | (10, 2) | (10,) | 10 | 2 | float64 | -0.0656 to 0.6112 | 0 | 0 / 0 | Minor caveat (bounds unverifiable) |
| Function_03 | (15, 3) | (15,) | 15 | 3 | float64 | -0.3989 to -0.0348 | 0 | 0 / 0 | Minor caveat (bounds unverifiable) |
| Function_04 | (30, 4) | (30,) | 30 | 4 | float64 | -32.626 to -4.026 | 0 | 0 / 0 | Minor caveat (bounds unverifiable) |
| Function_05 | (20, 4) | (20,) | 20 | 4 | float64 | 0.1129 to 1088.860 | 0 | 0 / 0 | **Needs attention** |
| Function_06 | (20, 5) | (20,) | 20 | 5 | float64 | -2.5712 to -0.7143 | 0 | 0 / 0 | Minor caveat (bounds unverifiable) |
| Function_07 | (30, 6) | (30,) | 30 | 6 | float64 | 0.0027 to 1.3650 | 0 | 0 / 0 | Minor caveat (bounds unverifiable) |
| Function_08 | (40, 8) | (40,) | 40 | 8 | float64 | 5.5922 to 9.5985 | 0 | 0 / 0 | Clean |

All figures above are **[VERIFIED]** exact values computed directly from the
`.npy` arrays (see per-function reports for full per-column input ranges).

## Cross-Function Findings

1. **Dtype consistency** — **[VERIFIED]** All 8 functions use `float64` for both
   inputs and outputs; no mixed, object, or string dtypes found anywhere.

2. **Dimensionality and observation-count progression** — **[VERIFIED]**
   Dimensionality increases from Function_01 (2D) through Function_08 (8D):
   2, 2, 3, 4, 4, 5, 6, 8. Observation counts (10, 10, 15, 30, 20, 20, 30, 40)
   match the `starter_observations` field recorded in each function's source
   `summary.json`.

3. **Duplicate input rows** — **[VERIFIED]** Zero exact duplicate rows in every
   function; zero across the whole Week_01 dataset. Minimum pairwise Euclidean
   distance within each function's input set ranges from 0.0749 (Function_02) to
   0.3779 (Function_08) — no near-duplicates of concern.

4. **Missing / infinite values** — **[VERIFIED]** Zero NaNs and zero Infs across
   all 16 arrays (8 inputs + 8 outputs).

5. **Observation-count alignment (inputs vs. outputs)** — **[VERIFIED]** Matches
   exactly in all 8 functions.

6. **Constant or degenerate columns** — **[VERIFIED]** None found; no input
   column has `min == max` in any function.

7. **Bounds verification** — Only **Function_01** has a written bounds
   specification (`[0, 1]^2`, stated in the source project's
   `06_Documentation/methodology.md`) — **[VERIFIED]** and respected. Functions
   02–08 have no equivalent bounds/spec file in either this rebuild project or
   the original source Capstone project — only descriptive README text about
   dimensionality and application domain, with no numeric bounds field. Their
   apparent `[0, 1]`-per-dimension pattern is an
   **[ASSUMPTION/INFERENCE]** by observation and analogy with Function_01, not
   a confirmed specification. **This should be explicitly caveated in any
   further formal documentation or modelling that assumes a `[0, 1]` domain for
   Functions 02–08.**

8. **Notable outlier — Function_05** — **[VERIFIED]** Output magnitude (max ≈
   1088.86) is roughly 2–3 orders of magnitude larger than every other function
   in this dataset (which range from about -33 to +9.6). Whether this is a
   legitimate feature of that function's true (undisclosed) formula or an
   artifact of data generation **cannot be determined without the original
   problem specification** — flagged as **[ASSUMPTION/INFERENCE]** for cause,
   but the scale disparity itself is a verified fact. This function needs
   attention before any cross-function pooling or normalization, since it could
   dominate a combined analysis.

9. **Function_08 output discrepancy (documented, not an error)** — **[VERIFIED]**
   The source project's `04_Results/summary.json` records a `best_output` of
   9.8157, which comes from one additional query recorded after the 40 starter
   observations, not from `initial_outputs.npy` itself (whose max is 9.5985).
   This is expected and traceable to the source project's own records, not a
   sign of corrupted or missing data.

10. **Provenance cross-check (extra, beyond the requested scope)** — **[VERIFIED]**
    Every one of the 16 rebuild `.npy` files (8 inputs + 8 outputs) is
    array-identical (`np.array_equal`) to its counterpart in the original source
    project (`~/Documents/GitHub/My_Capstone_1_Imperial/Week_01/Function_XX/03_Data/`).
    The rebuild did not introduce any transcription or corruption differences
    relative to the original source data.

11. **Sensitive data** — **[VERIFIED]** None found; no credentials, keys, or PII
    observed in any file inspected.

## What Was Checked

`initial_inputs.npy` / `initial_outputs.npy` for all 8 functions in this rebuild
project; for context and provenance cross-checking only, `provenance.json`,
`03_Data/README.md`, `06_Documentation/` content, and `04_Results/summary.json`
in the original source Capstone project were also read (not modified).

## What Was Not Checked (Out of Scope / Not Available)

- The true mathematical definitions of Functions 01–08 were not found anywhere
  in either project, so true domain bounds and expected output ranges for
  Functions 02–08 remain unverifiable from documentation alone.
- `04_Results`, `03_Queries`, and any notebook (`01_Notebook/*.ipynb`) contents
  in the rebuild project were not part of this audit's requested scope.
- No numeric bounds file exists for Functions 02–08 in either project, so
  bounds-compliance for those functions rests on inference alone, as stated
  above.

## Overall Conclusion

Across all 8 functions: **zero missing values, zero infinite values, zero
duplicate input rows, and fully matching input/output observation counts.** All
arrays use consistent `float64` dtypes. The dataset is broadly clean. The two
items warranting attention before further analysis are:

- **Function_05's output scale**, which is a verified outlier relative to the
  rest of the dataset and should be sanity-checked against its true function
  definition.
- **The absence of a written bounds specification for Functions 02–08**, which
  means their `[0, 1]`-domain assumption is inferred, not confirmed, and should
  be treated accordingly in any documentation or modelling that follows.
