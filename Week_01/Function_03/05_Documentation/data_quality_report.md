# Data Quality Report — Function_03 (Week_01)

**Scope:** Read-only audit of `02_Data/initial_inputs.npy` and `02_Data/initial_outputs.npy`.
No files were modified, renamed, or deleted as part of this audit. No models were
fitted and no query points were recommended.

**Legend:** Each item is labelled **[VERIFIED]** (directly computed from the data) or
**[ASSUMPTION/INFERENCE]** (inferred from context, not directly provable from the
arrays alone).

## 1. Shapes
- Input shape: `(15, 3)` **[VERIFIED]**
- Output shape: `(15,)` **[VERIFIED]**

## 2. Observations and Input Dimensions
- Number of observations: 15 **[VERIFIED]**
- Number of input dimensions: 3 **[VERIFIED]**

## 3. Data Types
- Input dtype: `float64` **[VERIFIED]**
- Output dtype: `float64` **[VERIFIED]**

## 4. Minimum / Maximum Values
- Input overall: min = 0.046808949722497384, max = 0.990881866558951 **[VERIFIED]**
- Input per-column:
  - Column 0: [0.046808949722497384, 0.9659948489488087] **[VERIFIED]**
  - Column 1: [0.2199172404897456, 0.9413598305723256] **[VERIFIED]**
  - Column 2: [0.06608864149534865, 0.990881866558951] **[VERIFIED]**
- Output: min = -0.3989255131463011, max = -0.034835313350078584 **[VERIFIED]**

## 5. NaN / Infinite Values
- Inputs: 0 NaN, 0 Inf **[VERIFIED]**
- Outputs: 0 NaN, 0 Inf **[VERIFIED]**

## 6. Duplicate Input Rows
- Exact duplicate rows: 0 **[VERIFIED]**
- Minimum pairwise Euclidean distance between input rows: 0.1163 — no near-duplicates **[VERIFIED]**

## 7. Input/Output Observation Count Match
- 15 input rows vs. 15 output rows: **Match** **[VERIFIED]**

## 8. Bounds Compliance
- No numeric bounds/domain specification file was found for Function_03 in either
  this rebuild project or the original source Capstone project.
- Observed inputs fall within [0, 1] per column, consistent with the [0,1]^3 pattern
  seen elsewhere, but this is **not independently confirmed**. **[ASSUMPTION/INFERENCE]**

## 9. Other Data-Quality Warnings
- All output values are negative (range -0.399 to -0.035). The source README
  describes this function as a raw "number of side effects" count converted to a
  maximization-friendly score, under which negative values are expected. This
  explanation of *why* the values are negative is an inference about intended
  semantics, not a verified defect. **[ASSUMPTION/INFERENCE]**
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
course source, applies to all eight functions, including Function_03.]**

This resolves the bounds-verification caveat above: the `[0, 1)` domain for this
function's inputs is now a confirmed course rule, not an inference by analogy.
Observed inputs (range [0.0468, 0.9909]) comply with this confirmed bound.

The same FAQ document also states, project-wide: "Some real-world analogies
involve minimisation (or side effects). These are transformed into maximisation
by negating the output so that higher values are better." This is an official,
project-wide confirmation of the sign-convention explanation already given in the
source README for this function's all-negative output range — no longer a
function-specific inference alone. **Revised verdict: Clean.**
