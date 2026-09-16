# Data Quality Report — Function_08 (Week_01)

**Scope:** Read-only audit of `02_Data/initial_inputs.npy` and `02_Data/initial_outputs.npy`.
No files were modified, renamed, or deleted as part of this audit. No models were
fitted and no query points were recommended.

**Legend:** Each item is labelled **[VERIFIED]** (directly computed from the data) or
**[ASSUMPTION/INFERENCE]** (inferred from context, not directly provable from the
arrays alone).

## 1. Shapes
- Input shape: `(40, 8)` **[VERIFIED]**
- Output shape: `(40,)` **[VERIFIED]**

## 2. Observations and Input Dimensions
- Number of observations: 40 **[VERIFIED]**
- Number of input dimensions: 8 **[VERIFIED]**

## 3. Data Types
- Input dtype: `float64` **[VERIFIED]**
- Output dtype: `float64` **[VERIFIED]**

## 4. Minimum / Maximum Values
- Input overall: min = 0.0034194994840646142, max = 0.9988854968558359 **[VERIFIED]**
- Input per-column:
  - Column 0: [0.009076976680729043, 0.9859453896331098] **[VERIFIED]**
  - Column 1: [0.0034194994840646142, 0.9739797874919709] **[VERIFIED]**
  - Column 2: [0.02292867798954412, 0.9988854968558359] **[VERIFIED]**
  - Column 3: [0.009043485399991336, 0.9029857747482919] **[VERIFIED]**
  - Column 4: [0.009648877991745852, 0.986901999632929] **[VERIFIED]**
  - Column 5: [0.022113406876058894, 0.9902438143772585] **[VERIFIED]**
  - Column 6: [0.03590887616954386, 0.992914494924961] **[VERIFIED]**
  - Column 7: [0.04195606945702324, 0.9887551042522277] **[VERIFIED]**
- Output: min = 5.5921933895401965, max = 9.598482002566342 **[VERIFIED]**

## 5. NaN / Infinite Values
- Inputs: 0 NaN, 0 Inf **[VERIFIED]**
- Outputs: 0 NaN, 0 Inf **[VERIFIED]**

## 6. Duplicate Input Rows
- Exact duplicate rows: 0 **[VERIFIED]**
- Minimum pairwise Euclidean distance between input rows: 0.3779 — no near-duplicates **[VERIFIED]**

## 7. Input/Output Observation Count Match
- 40 input rows vs. 40 output rows: **Match** **[VERIFIED]**

## 8. Bounds Compliance
- No numeric bounds/domain specification file was found for Function_08 in either
  this rebuild project or the original source Capstone project.
- Observed inputs fall within [0, 1] per column, consistent with the [0,1]^8 pattern
  seen elsewhere, but this is **not independently confirmed**. **[ASSUMPTION/INFERENCE]**

## 9. Other Data-Quality Warnings
- The source project's `04_Results/summary.json` reports `starter_observations: 40`
  and a `best_output` of 9.8157. That 9.8157 figure comes from one additional
  recorded query appended after the 40 starter points (per the source project's
  `recorded_pairs_registry`), **not** from `initial_outputs.npy` itself. This is
  expected behaviour, not a data-quality problem in this file, but is noted so the
  discrepancy (9.60 max in this raw starter array vs. 9.82 "best" in the source
  summary) is not mistaken for corrupted or missing data. **[VERIFIED — the
  discrepancy and its documented cause were both confirmed by reading the source
  project's files.]**
- No constant columns, no mixed dtypes, no non-numeric content found. **[VERIFIED]**

## Overall Verdict
**Clean.** No missing, duplicate, or infinite values, and observation counts match.
The only note is a documented, expected discrepancy between this file's max output
and a separately-recorded "best output" figure in the source project.

## Addendum (2026-09-16) — Bounds Confirmed Course-Wide
The official course document "Capstone Project FAQs" (provided by the user from
`~/Downloads/Capstone Project FAQs (1).pdf`, Imperial College Executive Education)
states: "Input values must always lie in the range 0.000000 to 0.999999 (i.e.
greater than or equal to 0 and strictly less than 1)." **[VERIFIED — official
course source, applies to all eight functions, including Function_08.]**

This resolves the bounds-verification caveat above: the `[0, 1)` domain for this
function's inputs is now a confirmed course rule, not an inference by analogy.
Observed inputs (range [0.0034, 0.9989]) comply with this confirmed bound. No
change to this function's "Clean" verdict.
