# Data Quality Report — Function_01 (Week_01)

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
- Input overall: min = 0.07872277794971883, max = 0.8838898288181171 **[VERIFIED]**
- Input per-column:
  - Column 0: [0.08250725182080587, 0.8838898288181171] **[VERIFIED]**
  - Column 1: [0.07872277794971883, 0.879898104984359] **[VERIFIED]**
- Output: min = -0.0036060626443634764, max = 7.710875114502849e-16 **[VERIFIED]**

## 5. NaN / Infinite Values
- Inputs: 0 NaN, 0 Inf **[VERIFIED]**
- Outputs: 0 NaN, 0 Inf **[VERIFIED]**

## 6. Duplicate Input Rows
- Exact duplicate rows: 0 **[VERIFIED]**
- Minimum pairwise Euclidean distance between input rows: 0.0959 — no near-duplicates **[VERIFIED]**

## 7. Input/Output Observation Count Match
- 10 input rows vs. 10 output rows: **Match** **[VERIFIED]**

## 8. Bounds Compliance
- The source project's `06_Documentation/methodology.md` (in the original Capstone
  repository, not in this rebuild) explicitly states inputs must have shape `(n, 2)`
  and lie inside `[0, 1]^2`. **[VERIFIED — a written bounds spec exists for this
  function only.]**
- Observed input range [0.0787, 0.8839] lies entirely within [0, 1]: **no violation**
  **[VERIFIED]**.
- Output has no stated bound (unbounded objective). Its observed range is very
  narrow (≈ -0.0036 to ≈ 7.7e-16, i.e. close to zero). Whether this narrow range is
  correct for the true underlying function cannot be confirmed without the function
  definition itself. **[ASSUMPTION/INFERENCE]**

## 9. Other Data-Quality Warnings
- The output magnitude is extremely small (order 1e-3 down to 1e-16), a narrow
  dynamic range. This is not a proven defect, but is worth double-checking against
  the intended function formula before use in modelling. **[ASSUMPTION/INFERENCE]**
- No constant columns, no mixed dtypes, no non-numeric content found. **[VERIFIED]**

## Overall Verdict
**Clean** — no missing, duplicate, or infinite values; a bounds specification exists
for this function and is respected. Only the narrow output dynamic range is flagged
for awareness, not as an error.

## Addendum (2026-09-16) — Bounds Confirmed Course-Wide
The official course document "Capstone Project FAQs" (provided by the user from
`~/Downloads/Capstone Project FAQs (1).pdf`, Imperial College Executive Education)
states: "Input values must always lie in the range 0.000000 to 0.999999 (i.e.
greater than or equal to 0 and strictly less than 1)." **[VERIFIED — official
course source, applies to all eight functions.]** This is consistent with, and now
independently confirms at the whole-project level, the function-specific
`[0, 1]^2` bound already found for Function_01 in the source project's
`methodology.md`. No change to this function's audit conclusions is needed.
