# Data Quality Report — Function_06 (Week_01)

**Scope:** Read-only audit of `02_Data/initial_inputs.npy` and `02_Data/initial_outputs.npy`.
No files were modified, renamed, or deleted as part of this audit. No models were
fitted and no query points were recommended.

**Legend:** Each item is labelled **[VERIFIED]** (directly computed from the data) or
**[ASSUMPTION/INFERENCE]** (inferred from context, not directly provable from the
arrays alone).

## 1. Shapes
- Input shape: `(20, 5)` **[VERIFIED]**
- Output shape: `(20,)` **[VERIFIED]**

## 2. Observations and Input Dimensions
- Number of observations: 20 **[VERIFIED]**
- Number of input dimensions: 5 **[VERIFIED]**

## 3. Data Types
- Input dtype: `float64` **[VERIFIED]**
- Output dtype: `float64` **[VERIFIED]**

## 4. Minimum / Maximum Values
- Input overall: min = 0.004911495864404647, max = 0.9788057635601997 **[VERIFIED]**
- Input per-column:
  - Column 0: [0.021735307663796388, 0.9577396688833609] **[VERIFIED]**
  - Column 1: [0.11440374416322774, 0.9318712161635005] **[VERIFIED]**
  - Column 2: [0.01652289968556364, 0.9788057635601997] **[VERIFIED]**
  - Column 3: [0.045613185779449616, 0.9616555869902994] **[VERIFIED]**
  - Column 4: [0.004911495864404647, 0.892819191058788] **[VERIFIED]**
- Output: min = -2.5711696316081234, max = -0.7142649478202404 **[VERIFIED]**

## 5. NaN / Infinite Values
- Inputs: 0 NaN, 0 Inf **[VERIFIED]**
- Outputs: 0 NaN, 0 Inf **[VERIFIED]**

## 6. Duplicate Input Rows
- Exact duplicate rows: 0 **[VERIFIED]**
- Minimum pairwise Euclidean distance between input rows: 0.3237 — no near-duplicates **[VERIFIED]**

## 7. Input/Output Observation Count Match
- 20 input rows vs. 20 output rows: **Match** **[VERIFIED]**

## 8. Bounds Compliance
- No numeric bounds/domain specification file was found for Function_06 in either
  this rebuild project or the original source Capstone project.
- Observed inputs fall within [0, 1] per column, consistent with the [0,1]^5 pattern
  seen elsewhere, but this is **not independently confirmed**. **[ASSUMPTION/INFERENCE]**

## 9. Other Data-Quality Warnings
- All outputs are negative (range -2.571 to -0.714). The source README describes
  this as a "negative score" combining competing criteria, where the best value is
  closest to zero — under that description, all-negative values are expected. This
  is an inference about intended semantics, not a verified defect. **[ASSUMPTION/INFERENCE]**
- No constant columns, no mixed dtypes, no non-numeric content found. **[VERIFIED]**

## Overall Verdict
**Minor caveat (bounds unverifiable), otherwise clean.** No missing, duplicate, or
infinite values, and observation counts match; all-negative outputs appear
intentional per source documentation but this should be confirmed before analysis.

## Addendum (2026-09-16) — Bounds Now Confirmed Course-Wide
The official course document "Capstone Project FAQs" (provided by the user from
`~/Downloads/Capstone Project FAQs (1).pdf`, Imperial College Executive Education)
states: "Input values must always lie in the range 0.000000 to 0.999999 (i.e.
greater than or equal to 0 and strictly less than 1)." **[VERIFIED — official
course source, applies to all eight functions, including Function_06.]**

This resolves the bounds-verification caveat above: the `[0, 1)` domain for this
function's inputs is now a confirmed course rule, not an inference by analogy.
Observed inputs (range [0.0049, 0.9788]) comply with this confirmed bound.

The same FAQ document also states, project-wide: "Some real-world analogies
involve minimisation (or side effects). These are transformed into maximisation
by negating the output so that higher values are better." This is an official,
project-wide confirmation of the sign-convention explanation already given in the
source README for this function's all-negative output range. **Revised verdict:
Clean.**
