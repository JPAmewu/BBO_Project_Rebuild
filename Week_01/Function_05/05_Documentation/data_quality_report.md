# Data Quality Report — Function_05 (Week_01)

**Scope:** Read-only audit of `02_Data/initial_inputs.npy` and `02_Data/initial_outputs.npy`.
No files were modified, renamed, or deleted as part of this audit. No models were
fitted and no query points were recommended.

**Legend:** Each item is labelled **[VERIFIED]** (directly computed from the data) or
**[ASSUMPTION/INFERENCE]** (inferred from context, not directly provable from the
arrays alone).

## 1. Shapes
- Input shape: `(20, 4)` **[VERIFIED]**
- Output shape: `(20,)` **[VERIFIED]**

## 2. Observations and Input Dimensions
- Number of observations: 20 **[VERIFIED]**
- Number of input dimensions: 4 **[VERIFIED]**

## 3. Data Types
- Input dtype: `float64` **[VERIFIED]**
- Output dtype: `float64` **[VERIFIED]**

## 4. Minimum / Maximum Values
- Input overall: min = 0.03819337135150802, max = 0.957643898670113 **[VERIFIED]**
- Input per-column:
  - Column 0: [0.11987922582428101, 0.8364779930351233] **[VERIFIED]**
  - Column 1: [0.03819337135150802, 0.8625403059525095] **[VERIFIED]**
  - Column 2: [0.08894684260658514, 0.8794841797090803] **[VERIFIED]**
  - Column 3: [0.07288048110014078, 0.957643898670113] **[VERIFIED]**
- Output: min = 0.1129397953712203, max = 1088.8596181962705 **[VERIFIED]**

## 5. NaN / Infinite Values
- Inputs: 0 NaN, 0 Inf **[VERIFIED]**
- Outputs: 0 NaN, 0 Inf **[VERIFIED]**

## 6. Duplicate Input Rows
- Exact duplicate rows: 0 **[VERIFIED]**
- Minimum pairwise Euclidean distance between input rows: 0.204 — no near-duplicates **[VERIFIED]**

## 7. Input/Output Observation Count Match
- 20 input rows vs. 20 output rows: **Match** **[VERIFIED]**

## 8. Bounds Compliance
- No numeric bounds/domain specification file was found for Function_05 in either
  this rebuild project or the original source Capstone project.
- Observed inputs fall within [0, 1] per column, consistent with the [0,1]^4 pattern
  seen elsewhere, but this is **not independently confirmed**. **[ASSUMPTION/INFERENCE]**

## 9. Other Data-Quality Warnings
- **Output magnitude is a clear outlier relative to every other function**: max
  ≈ 1088.86, roughly 2–3 orders of magnitude larger than any other function in this
  Week_01 dataset (which range from about -33 to +9.6). This is a **[VERIFIED]**
  observed fact from the raw array.
- The source README describes this as a manufacturing-yield problem with "one main
  peak," which could plausibly produce a large positive value at the peak, but this
  explanation is an inference, not a confirmed cause. **[ASSUMPTION/INFERENCE]**
- This scale disparity should be flagged for anyone doing cross-function comparison,
  pooling, or normalization, since Function_05 could dominate any combined analysis
  if not scaled or handled separately.
- No constant columns, no mixed dtypes, no non-numeric content found. **[VERIFIED]**

## Overall Verdict
**Needs attention.** The extreme output magnitude (max 1088.86 vs. O(1)–O(10)
elsewhere) is a verified outlier that should be sanity-checked against the true
function definition before being used in any cross-function modeling or
normalization. It is not proven to be erroneous from the data alone, but it is the
single most notable data-quality flag across the eight functions in this audit.

## Addendum (2026-09-16) — Bounds Confirmed; Outlier Corroborated by Official FAQ
**Bounds:** The official course document "Capstone Project FAQs" (provided by the
user from `~/Downloads/Capstone Project FAQs (1).pdf`, Imperial College Executive
Education) states: "Input values must always lie in the range 0.000000 to
0.999999." **[VERIFIED — official course source, applies to all eight functions,
including Function_05.]** Observed inputs (range [0.0382, 0.9576]) comply.

**Output magnitude:** The same FAQ document, in a worked example of a sample
submission ("Q: What type of processed data points will I receive after every
submission?"), shows a Function 5 output value of **6619.923795891087** — of the
same order of magnitude as (and larger than) this dataset's observed max of
1088.8596181962705. **[VERIFIED — this exact number appears in the official FAQ
document.]** This is strong corroborating evidence that large-magnitude outputs,
substantially larger than the other seven functions, are an expected, by-design
characteristic of Function 5 specifically, rather than a data-quality defect or
transcription artifact.

**Caveat:** The FAQ's example uses different input values than this dataset's
starter observations, so it does not mathematically prove that 1088.8596181962705
is the exact correct value for this specific input point — no formula or
generator code is available to confirm that. However, it removes the concern that
this magnitude is implausible or symptomatic of corrupted data.

**Revised verdict: Minor caveat (large output scale, now understood to be
expected by design for this function) — no longer classified as "needs
attention."** The scale disparity is still relevant for any cross-function
normalization or pooling decisions, but is not treated as a data-quality error.
