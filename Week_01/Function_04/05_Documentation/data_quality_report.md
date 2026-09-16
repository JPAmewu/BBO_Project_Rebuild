# Data Quality Report — Function_04 (Week_01)

**Scope:** Read-only audit of `02_Data/initial_inputs.npy` and `02_Data/initial_outputs.npy`.
No files were modified, renamed, or deleted as part of this audit. No models were
fitted and no query points were recommended.

**Legend:** Each item is labelled **[VERIFIED]** (directly computed from the data) or
**[ASSUMPTION/INFERENCE]** (inferred from context, not directly provable from the
arrays alone).

## 1. Shapes
- Input shape: `(30, 4)` **[VERIFIED]**
- Output shape: `(30,)` **[VERIFIED]**

## 2. Observations and Input Dimensions
- Number of observations: 30 **[VERIFIED]**
- Number of input dimensions: 4 **[VERIFIED]**

## 3. Data Types
- Input dtype: `float64` **[VERIFIED]**
- Output dtype: `float64` **[VERIFIED]**

## 4. Minimum / Maximum Values
- Input overall: min = 0.006250400244917853, max = 0.9994825612275692 **[VERIFIED]**
- Input per-column:
  - Column 0: [0.03782482853334601, 0.9856218929694016] **[VERIFIED]**
  - Column 1: [0.006250400244917853, 0.9195923219593923] **[VERIFIED]**
  - Column 2: [0.04218634513719777, 0.9391779073552307] **[VERIFIED]**
  - Column 3: [0.08151656419033015, 0.9994825612275692] **[VERIFIED]**
- Output: min = -32.625660215962455, max = -4.025542281908162 **[VERIFIED]**

## 5. NaN / Infinite Values
- Inputs: 0 NaN, 0 Inf **[VERIFIED]**
- Outputs: 0 NaN, 0 Inf **[VERIFIED]**

## 6. Duplicate Input Rows
- Exact duplicate rows: 0 **[VERIFIED]**
- Minimum pairwise Euclidean distance between input rows: 0.0956 — no near-duplicates **[VERIFIED]**

## 7. Input/Output Observation Count Match
- 30 input rows vs. 30 output rows: **Match** **[VERIFIED]**

## 8. Bounds Compliance
- No numeric bounds/domain specification file was found for Function_04 in either
  this rebuild project or the original source Capstone project.
- Observed inputs fall within [0, 1] per column, consistent with the [0,1]^4 pattern
  seen elsewhere, but this is **not independently confirmed**. **[ASSUMPTION/INFERENCE]**

## 9. Other Data-Quality Warnings
- Output range is entirely negative and notably wider (-32.6 to -4.0) than
  Functions 01–03. This scale difference is an observed fact **[VERIFIED]**;
  whether this scale is "correct" for the intended function requires the
  ground-truth function definition, which was not available. **[ASSUMPTION/INFERENCE]**
- No constant columns, no mixed dtypes, no non-numeric content found. **[VERIFIED]**

## Overall Verdict
**Minor caveat (bounds unverifiable), otherwise clean.** No missing, duplicate, or
infinite values, and observation counts match; the wider negative output scale is
flagged for awareness in cross-function comparisons.

## Addendum (2026-09-16) — Bounds Now Confirmed Course-Wide
The official course document "Capstone Project FAQs" (provided by the user from
`~/Downloads/Capstone Project FAQs (1).pdf`, Imperial College Executive Education)
states: "Input values must always lie in the range 0.000000 to 0.999999 (i.e.
greater than or equal to 0 and strictly less than 1)." **[VERIFIED — official
course source, applies to all eight functions, including Function_04.]**

This resolves the bounds-verification caveat above: the `[0, 1)` domain for this
function's inputs is now a confirmed course rule, not an inference by analogy.
Observed inputs (range [0.0063, 0.9995]) comply with this confirmed bound.

The same FAQ document also states, project-wide: "Some real-world analogies
involve minimisation (or side effects). These are transformed into maximisation
by negating the output so that higher values are better." Unlike Function_03 and
Function_06, this function's own source README does not give a function-specific
explanation for its all-negative outputs, but this project-wide FAQ rule provides
a general, course-confirmed explanation applicable to any function in this
capstone, including Function_04. **Revised verdict: Clean** (wider negative scale
remains worth noting for cross-function comparisons, but is no longer an
unexplained anomaly).
