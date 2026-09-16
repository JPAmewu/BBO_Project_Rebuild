# Data Quality Report — Function_07 (Week_01)

**Scope:** Read-only audit of `02_Data/initial_inputs.npy` and `02_Data/initial_outputs.npy`.
No files were modified, renamed, or deleted as part of this audit. No models were
fitted and no query points were recommended.

**Legend:** Each item is labelled **[VERIFIED]** (directly computed from the data) or
**[ASSUMPTION/INFERENCE]** (inferred from context, not directly provable from the
arrays alone).

## 1. Shapes
- Input shape: `(30, 6)` **[VERIFIED]**
- Output shape: `(30,)` **[VERIFIED]**

## 2. Observations and Input Dimensions
- Number of observations: 30 **[VERIFIED]**
- Number of input dimensions: 6 **[VERIFIED]**

## 3. Data Types
- Input dtype: `float64` **[VERIFIED]**
- Output dtype: `float64` **[VERIFIED]**

## 4. Minimum / Maximum Values
- Input overall: min = 0.0036345648296357558, max = 0.9986546964586872 **[VERIFIED]**
- Input per-column:
  - Column 0: [0.057895541971385245, 0.9424508393797228] **[VERIFIED]**
  - Column 1: [0.011812836708676278, 0.9246939036961066] **[VERIFIED]**
  - Column 2: [0.0036345648296357558, 0.9245705141349381] **[VERIFIED]**
  - Column 3: [0.07365918727355969, 0.9610171372277223] **[VERIFIED]**
  - Column 4: [0.014944176429292355, 0.9986546964586872] **[VERIFIED]**
  - Column 5: [0.05109986421364299, 0.9510139152470551] **[VERIFIED]**
- Output: min = 0.0027014650245082332, max = 1.3649683044991994 **[VERIFIED]**

## 5. NaN / Infinite Values
- Inputs: 0 NaN, 0 Inf **[VERIFIED]**
- Outputs: 0 NaN, 0 Inf **[VERIFIED]**

## 6. Duplicate Input Rows
- Exact duplicate rows: 0 **[VERIFIED]**
- Minimum pairwise Euclidean distance between input rows: 0.1798 — no near-duplicates **[VERIFIED]**

## 7. Input/Output Observation Count Match
- 30 input rows vs. 30 output rows: **Match** **[VERIFIED]**

## 8. Bounds Compliance
- No numeric bounds/domain specification file was found for Function_07 in either
  this rebuild project or the original source Capstone project.
- Observed inputs fall within [0, 1] per column, consistent with the [0,1]^6 pattern
  seen elsewhere, but this is **not independently confirmed**. **[ASSUMPTION/INFERENCE]**

## 9. Other Data-Quality Warnings
- None observed beyond the general bounds-verification caveat above. No constant
  columns, no mixed dtypes, no non-numeric content found. **[VERIFIED]**

## Overall Verdict
**Minor caveat (bounds unverifiable), otherwise clean.** No missing, duplicate, or
infinite values, and observation counts match.

## Addendum (2026-09-16) — Bounds Now Confirmed Course-Wide
The official course document "Capstone Project FAQs" (provided by the user from
`~/Downloads/Capstone Project FAQs (1).pdf`, Imperial College Executive Education)
states: "Input values must always lie in the range 0.000000 to 0.999999 (i.e.
greater than or equal to 0 and strictly less than 1)." **[VERIFIED — official
course source, applies to all eight functions, including Function_07.]**

This resolves the bounds-verification caveat above: the `[0, 1)` domain for this
function's inputs is now a confirmed course rule, not an inference by analogy.
Observed inputs (range [0.0036, 0.9987]) comply with this confirmed bound.
**Revised verdict: Clean.**
