# Data Quality Report — Function_02 (Week_01)

**Scope:** Read-only audit of `02_Data/initial_inputs.npy` and `02_Data/initial_outputs.npy`.
No files were modified, renamed, or deleted as part of this audit. No models were
fitted and no query points were recommended.

**Legend:** Each item is labelled **[VERIFIED]** (directly computed from the data) or
**[ASSUMPTION/INFERENCE]** (inferred from context, not directly provable from the
arrays alone).

## 1. Shapes
- Input shape: `(10, 2)` **[VERIFIED]**
- Output shape: `(10,)` **[VERIFIED]**

## 2. Observations and Input Dimensions
- Number of observations: 10 **[VERIFIED]**
- Number of input dimensions: 2 **[VERIFIED]**

## 3. Data Types
- Input dtype: `float64` **[VERIFIED]**
- Output dtype: `float64` **[VERIFIED]**

## 4. Minimum / Maximum Values
- Input overall: min = 0.028697719822277867, max = 0.9265641975455574 **[VERIFIED]**
- Input per-column:
  - Column 0: [0.14269907423594608, 0.8777909889953304] **[VERIFIED]**
  - Column 1: [0.028697719822277867, 0.9265641975455574] **[VERIFIED]**
- Output: min = -0.06562362443733738, max = 0.6112052157614438 **[VERIFIED]**

## 5. NaN / Infinite Values
- Inputs: 0 NaN, 0 Inf **[VERIFIED]**
- Outputs: 0 NaN, 0 Inf **[VERIFIED]**

## 6. Duplicate Input Rows
- Exact duplicate rows: 0 **[VERIFIED]**
- Minimum pairwise Euclidean distance between input rows: 0.0749 — no near-duplicates **[VERIFIED]**

## 7. Input/Output Observation Count Match
- 10 input rows vs. 10 output rows: **Match** **[VERIFIED]**

## 8. Bounds Compliance
- No numeric bounds/domain specification file was found for Function_02 in either
  this rebuild project or the original source Capstone project (only descriptive
  README text about dimensionality/application, no numeric bounds field).
- Observed inputs fall within [0, 1] per column, consistent with the [0,1]^2 pattern
  confirmed for Function_01, but this is **not independently confirmed** for
  Function_02. **[ASSUMPTION/INFERENCE]**

## 9. Other Data-Quality Warnings
- None observed beyond the bounds-verification caveat above. No constant columns,
  no mixed dtypes, no non-numeric content found. **[VERIFIED]**

## Overall Verdict
**Minor caveat (bounds unverifiable), otherwise clean.** No missing, duplicate, or
infinite values, and observation counts match; the only open item is that no
written domain-bounds specification exists for this function.

## Addendum (2026-09-16) — Bounds Now Confirmed Course-Wide
The official course document "Capstone Project FAQs" (provided by the user from
`~/Downloads/Capstone Project FAQs (1).pdf`, Imperial College Executive Education)
states: "Input values must always lie in the range 0.000000 to 0.999999 (i.e.
greater than or equal to 0 and strictly less than 1)." **[VERIFIED — official
course source, applies to all eight functions, including Function_02.]**

This resolves the bounds-verification caveat above: the `[0, 1)` domain for this
function's inputs is now a confirmed course rule, not an inference by analogy.
Observed inputs (range [0.0287, 0.9266]) comply with this confirmed bound.
**Revised verdict: Clean.**
