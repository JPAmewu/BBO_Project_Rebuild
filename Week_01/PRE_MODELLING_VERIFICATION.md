# Pre-Modelling Verification — Week_01, Functions 01–08

**Scope:** Read-only pre-modelling verification conducted by the ML Reviewer Agent.
No model was built, no query points were recommended, and no dataset, `CLAUDE.md`,
or file in the original `My_Capstone_1_Imperial` project was modified, renamed,
moved, or deleted. This review builds on `DATA_QUALITY_SUMMARY.md` and the eight
`05_Documentation/data_quality_report.md` files already produced by the Data
Quality Agent.

**Legend:** **[VERIFIED]** = directly confirmed from source files/data.
**[UNRESOLVED]** = an open question that cannot be settled from files available in
either repository. **[RECOMMENDATION]** = a suggested future action — nothing
listed here has been carried out. **[RESOLVED]** = an item originally raised as
unresolved in this report that has since been settled by new evidence (see
Addendum below); the original finding is kept for audit-trail purposes and
annotated rather than deleted.

---

## Addendum (2026-09-16) — New Evidence from Official Course Documentation

After this report was first issued, the user supplied the official course
document **"Capstone Project FAQs"** (Imperial College Executive Education, from
`~/Downloads/Capstone Project FAQs (1).pdf`). This is an authoritative source
external to both the rebuild and source repositories, and it resolves two items
this report had previously left open:

1. **Bounds for Functions 02–08 are now [RESOLVED].** The FAQ states: "Input
   values must always lie in the range 0.000000 to 0.999999 (i.e. greater than or
   equal to 0 and strictly less than 1)." This is stated as a rule for the
   capstone project as a whole, not specific to Function_01 — it therefore
   confirms a `[0, 1)` domain for **all eight functions**, superseding the
   previous "inference by analogy only" status for Functions 02–08.
2. **Function_05's output magnitude is now [RESOLVED] as expected-by-design, not
   an anomaly.** The FAQ's own worked example (in "What type of processed data
   points will I receive after every submission?") shows a **Function 5 output
   of 6619.923795891087** — the same order of magnitude as, and larger than,
   this dataset's observed max of 1088.8596181962705. This does not
   mathematically prove the exact value 1088.8596181962705 is correct (no
   formula is available), but it removes the concern that such a large magnitude
   is implausible or indicative of corrupted/erroneous data for this function.
3. The FAQ also confirms, project-wide: every function is framed as a
   **maximisation problem**, and outputs "may be negative by design (for
   example, when penalties are applied)... Some real-world analogies involve
   minimisation (or side effects). These are transformed into maximisation by
   negating the output so that higher values are better." This provides a
   general, course-confirmed explanation for all-negative output ranges
   (Functions 03, 04, 06), including Function_04, which previously had no
   function-specific documented rationale.

Sections below retain their original findings as issued, with **[RESOLVED]**
annotations added inline where this new evidence applies. Nothing has been
deleted or rewritten to conceal the original (correct, at the time) assessment.

---

## 1. Function_05 Outlier Investigation

**Question:** Is the output maximum of ≈1088.86 present in the original source
data, and does documentation suggest it is legitimate?

### Verified facts
- **[VERIFIED]** The value `1088.8596181962705` is present, identically, in the
  *original source project's own files*:
  - `Function_05/03_Data/initial_outputs.npy` (shape `(20,)`), at index 15.
  - `Function_05/03_Data/verified_cumulative_outputs.npy` (shape `(21,)`), at the
    same position, which also contains one additional appended value,
    `1088.8535114737463` (index 20).
  - `Function_05/04_Results/observations.csv`, row 17 (query 16, tagged
    `starter` — i.e. part of the original starter dataset, not injected later).
  - `Function_05/04_Results/summary.json`: `"best_output": 1088.8596181962705`,
    `"best_query": 16`, consistent with the raw arrays.
  - `Function_05/01_Notebook/Week_01_Function_05.ipynb`: the rounded value
    `1088.859618` recurs across multiple analysis/table-display cells.
- **[VERIFIED]** This is **not a rebuild-only artifact**: the value was
  independently re-loaded directly from the source project's own `.npy` files
  (not only inferred from the prior `np.array_equal` provenance check), confirming
  it originates in the source data itself.
- **[VERIFIED]** A second, independently recorded query (query 21, tagged
  `recorded`) at a nearly identical input point produced a nearly identical large
  output, `1088.8535114737463` — described in `Function_05/07_Reflection/Reflection.md`
  as "the strongest result obtained among all functions during Week 1." This is at
  least circumstantial corroboration that this magnitude is reproducible at that
  region of the input space, rather than a one-off numeric glitch.
- **[VERIFIED]** The magnitude (~1088) remains 2–3 orders of magnitude above every
  other function's output range in this dataset (all other functions range from
  approximately -33 to +9.6).

### Documentation search for an explanation
- `Function_05/03_Data/README.md` describes it as "a four-variable
  process-optimisation problem. It can represent the selection of chemical inputs
  that produces the greatest manufacturing yield. The response is expected to have
  one main peak..." — qualitatively consistent with a large positive peak, but
  **[UNRESOLVED]** does not state a formula, units, or explicit expected scale.
- `Function_05/06_Documentation/README.md` is only a generic one-line stub; unlike
  Function_01, **no `methodology.md` exists for Function_05**.
- `Function_05/03_Data/provenance.json` contains only pipeline/process metadata,
  no formula or scale statement.
- `Function_05/02_Code/` contains only a README stub and cached bytecode; no
  Python source defining the function exists anywhere in the source project
  (unlike Function_01, which has an analysis script — though even that script does
  not define the objective itself).

### Unresolved (original) / Resolved (see Addendum)
- **[UNRESOLVED — original]** No mathematical formula, generator code, or
  explicit output-scale/units statement exists anywhere in the source project for
  Function_05. Whether ~1088 is a genuine feature of the true underlying function
  or reflects an unintended scale/unit issue upstream (outside both repositories)
  cannot be determined from static file inspection alone.
- **[RESOLVED — see Addendum]** The official course FAQ's own worked example
  shows a Function 5 output of 6619.92, corroborating that large magnitudes are
  expected by design for this function. This does not supply a formula (so the
  *exact* value 1088.8596181962705 still cannot be mathematically re-derived),
  but it resolves the practical question of whether this magnitude is
  plausible/legitimate for Function 5 — it is.

---

## 2. Documented Search Bounds and Dimensionality — Functions 01–08

**Original findings (per-repository search only):**

| Function | Dimensionality | Matches data-quality report? | Documented bounds found *(in the two project repositories)*? | Source | Consistent with audited data? |
|---|---|---|---|---|---|
| Function_01 | 2 | Yes | **YES** — explicit `[0, 1]^2` | `Function_01/06_Documentation/methodology.md`: "Inputs must have shape (n, 2)... and inputs must lie inside [0, 1]^2." | Yes — observed range [0.0787, 0.8839] lies entirely within [0,1] |
| Function_02 | 2 | Yes | No (repo-only search) | `03_Data/README.md` gives only a qualitative description ("two-variable noisy optimisation problem"); no numeric bounds field anywhere | Observed inputs happen to lie within [0,1]; inference/analogy only |
| Function_03 | 3 | Yes | No (repo-only search) | `03_Data/README.md` ("three-variable formulation problem"); no numeric range stated | Observed inputs lie within [0,1]; unconfirmed |
| Function_04 | 4 | Yes | No (repo-only search) | `03_Data/README.md` ("four-variable model-tuning problem"); no numeric bounds | Observed inputs lie within [0,1]; unconfirmed |
| Function_05 | 4 | Yes | No (repo-only search) | `03_Data/README.md` ("four-variable process-optimisation problem"); no numeric bounds, no methodology.md | Observed inputs lie within [0,1]; unconfirmed |
| Function_06 | 5 | Yes | No (repo-only search) | `03_Data/README.md` ("five-variable multi-criteria design problem"); no numeric bounds | Observed inputs lie within [0,1]; unconfirmed |
| Function_07 | 6 | Yes | No (repo-only search) | `03_Data/README.md` ("six-variable hyperparameter-tuning problem"); no numeric bounds | Observed inputs lie within [0,1]; unconfirmed |
| Function_08 | 8 | Yes | No (repo-only search) | `03_Data/README.md` ("eight-variable high-dimensional optimisation problem"); no numeric bounds | Observed inputs lie within [0,1]; unconfirmed |

**[VERIFIED]** Dimensionality for all 8 functions (2, 2, 3, 4, 4, 5, 6, 8) was
cross-verified directly against each function's own `04_Results/summary.json`
`"dimensions"` field, matching both the array shapes and the previously issued
data-quality reports.

**[VERIFIED — original, repo-scoped]** Only Function_01 has an explicit numeric
bounds specification anywhere *in the two project repositories*. A recursive
search of every `.md`/`.json` file under all 8 function folders for terms
including "bound", "[0,1]", "domain", "range", and "dimension" confirms this.
Note: several `07_Reflection/README.md` template files mention the word "bounds"
generically (e.g. "checked the response trace, running incumbent, bounds, and
provenance") — this is boilerplate reflection-template language, **not** a
bounds specification, and should not be mistaken for one.

**[RESOLVED — see Addendum]** Functions 02–08's numeric domain bounds, left
unresolved by the original repo-only search, are now confirmed by the official
course FAQ: **all eight functions share the same documented bound, `[0.000000,
0.999999]` per input dimension** (i.e. `[0, 1)`). This is external, authoritative
course documentation, not a project-repository file, which is why the original
search (scoped to the two repositories) did not find it. All previously audited
input data for Functions 02–08 (see each function's `data_quality_report.md`)
falls within this confirmed bound.

---

## 3. Output Transformation / Normalisation / Special Numerical Treatment Considerations

**No transformation has been applied. The items below are considerations for a
future modelling step only.**

| Function | Output range | Consideration |
|---|---|---|
| Function_01 | ≈ -0.0036 to 7.7e-16 | Extremely narrow, near-machine-epsilon dynamic range. Would likely need independent per-function standardization before any pooled/comparative surrogate, since a shared global scaler would effectively zero it out. Values this close to float64 machine epsilon (~2.2e-16) warrant a check for whether they represent genuine signal or partial floating-point noise. |
| Function_02 | ≈ -0.0656 to 0.6112 | Moderate range spanning zero; source README describes this as a "log-likelihood score" — i.e. already log-scaled by description, so an additional log-transform would likely be redundant/inappropriate. |
| Function_03 | ≈ -0.3989 to -0.0348 (all negative) | README states the raw "number of side effects" was converted to a maximization-friendly score. If a shared convention assumes "larger/positive is better," any sign or offset adjustment should be applied per this documented convention, not assumed blindly. |
| Function_04 | ≈ -32.626 to -4.026 (all negative, wider spread) | Order of magnitude larger (absolute terms) than Functions 01–03; would need per-function standardization before pooling. **No documentation explains why this function's outputs are all negative** (unlike Function_03/06) — a documentation gap, not an assumption to fill by analogy. |
| Function_05 | ≈ 0.1129 to 1088.86 | Strong candidate for log-transform or robust scaling given the ~4-orders-of-magnitude spread within its own data and its extreme scale relative to all other functions. **[RESOLVED — see Addendum]** The official course FAQ's own example (Function 5 output ≈ 6619.92) confirms this scale is expected by design, not an artifact — the transform choice is now purely a modelling decision (when the time comes), not something blocked on an unresolved legitimacy question. |
| Function_06 | ≈ -2.5712 to -0.7143 (all negative) | README explicitly documents this as an intentionally negative combined score where "best is closest to zero." Any sign-flip/offset for maximization should follow this documented semantics specifically. |
| Function_07 | ≈ 0.0027 to 1.3650 | Positive, moderate range; no strong transform indicated by the statistics alone. |
| Function_08 | ≈ 5.5922 to 9.5985 | Positive, moderate/compact range; no strong transform indicated. Note the previously documented (non-erroneous) discrepancy where the source `summary.json`'s `best_output` (9.8157) reflects one additional post-hoc recorded query not present in `initial_outputs.npy` — keep this in mind so comparisons don't mix the two figures. |

**Cross-function consideration:** Given the scale disparity across functions
(from ~1e-16 up to ~1088), any future shared or comparative GP surrogate would
need **per-function standardization**, not a single global scaler fit across
pooled outputs, to avoid one function's outputs dominating shared distance/kernel
computations purely due to scale rather than genuine structure.

---

## 4. Verified Facts

1. **[VERIFIED]** Function_05's max output (`1088.8596181962705`) is present
   identically in the source project's `initial_outputs.npy`,
   `verified_cumulative_outputs.npy`, `observations.csv`, and `summary.json`; it
   is not a rebuild-only artifact.
2. **[VERIFIED]** A second, distinct query (query 21) at a nearly identical input
   independently produced a nearly identical large output
   (`1088.8535114737463`), documented as "the strongest result obtained among all
   functions during Week 1."
3. **[VERIFIED]** Only Function_01 has a written numeric bounds specification
   anywhere in the source project (`[0, 1]^2`); Functions 02–08 have no numeric
   bounds file in either project.
4. **[VERIFIED]** Dimensionality per function (2, 2, 3, 4, 4, 5, 6, 8 for
   Functions 01–08) is consistent across `summary.json`, array shapes, and the
   data-quality reports.
5. **[VERIFIED]** No mathematical formula, generator code, or explicit
   output-scale/units statement exists anywhere in the source project for any of
   the 8 functions — only qualitative application-domain descriptions.
6. **[VERIFIED]** Function_03 and Function_06's all-negative outputs each have an
   explicit qualitative documentation basis in their `03_Data/README.md` files;
   Function_04's all-negative output range has no function-specific documented
   explanation, though it is now covered by the general project-wide rule in
   item 8 below.
7. **[VERIFIED — Addendum]** The official course FAQ confirms, for the whole
   project: input bounds are `[0.000000, 0.999999]` for every function; every
   function is a maximisation problem; outputs "may be negative by design"; and
   real-world minimisation/side-effect analogies are handled by negating the
   output so higher is always better.
8. **[VERIFIED — Addendum]** The FAQ's own worked example reports a Function 5
   output of `6619.923795891087`, corroborating that Function_05's large output
   scale (this dataset's max: `1088.8596181962705`) is expected by design.

## 5. Unresolved Issues / Open Questions

1. ~~**[UNRESOLVED]** Whether Function_05's ~1088 magnitude is a genuine feature
   of its true (undisclosed) underlying function...~~ **[RESOLVED — see
   Addendum]** The official course FAQ's own example output (≈6619.92 for
   Function 5) confirms this scale is expected by design for this function. The
   *exact* mathematical formula is still not available, so the precise value
   1088.8596181962705 cannot be independently re-derived, but its plausibility
   is no longer in question.
2. ~~**[UNRESOLVED]** Functions 02–08 have no confirmed numeric domain-bounds
   specification...~~ **[RESOLVED — see Addendum]** The official course FAQ
   confirms `[0.000000, 0.999999]` as the bound for all eight functions.
3. ~~**[UNRESOLVED]** Function_04's all-negative output convention has no
   documented rationale...~~ **[RESOLVED — see Addendum]** The official course
   FAQ's general negation-for-maximisation rule applies project-wide and covers
   Function_04, even without a function-specific README explanation.
4. **[UNRESOLVED — still open]** No independent way exists (within this
   read-only review) to confirm reproducibility controls (seeds, generator
   versions) for how any of the 8 functions' hidden objectives were originally
   generated, since no generator source code is present in the source
   repository for any function. The official course FAQ does not address this
   either — it describes the functions as "synthetic" but does not disclose
   generation methodology.

## 6. Recommendations (Not Implemented)

1. ~~**[RECOMMENDATION]** ...explicitly resolve or formally flag the Function_05
   magnitude question...~~ **[SUPERSEDED]** Resolved by the official course FAQ
   (Addendum); no further action needed on this point.
2. ~~**[RECOMMENDATION]** Add an explicit, written bounds/domain specification
   for Functions 02–08...~~ **[SUPERSEDED]** The official course FAQ now serves
   as this specification (`[0.000000, 0.999999]` for all functions); no
   project-internal spec file is strictly required, though copying the FAQ's
   bound statement into each function's own documentation folder would still be
   good practice for self-containedness.
3. **[RECOMMENDATION — still applies]** If a shared/comparative GP surrogate
   across functions is ever built, apply per-function standardization (not a
   single global scaler across pooled outputs); a log-transform specifically for
   Function_05 remains a reasonable option now that its scale is understood to
   be expected by design (previously this was gated on resolving the legitimacy
   question — that gate is now removed).
4. ~~**[RECOMMENDATION]** Document, or seek documentation for, why Function_04's
   outputs are uniformly negative...~~ **[SUPERSEDED]** Resolved by the official
   course FAQ's project-wide negation-for-maximisation rule (Addendum).
5. **[RECOMMENDATION — still applies]** Treat Function_01's near-machine-epsilon
   output values (down to ~7.7e-16) with care in any downstream numerical
   pipeline — verify they are not partially floating-point noise before relying
   on fine-grained differences at that scale.
6. **[RECOMMENDATION — still applies]** Any future report or model write-up
   should continue to clearly separate "verified from data/documentation" claims
   from "assumed by analogy" claims, consistent with the convention used in this
   report and in the existing data-quality reports.
7. **[RECOMMENDATION — new]** When implementing the search space for Bayesian
   optimisation (any future modelling step), use the FAQ-confirmed bound
   `[0.000000, 0.999999)` for every function's every input dimension, and use
   the FAQ-confirmed six-decimal, hyphen-separated submission format
   (`x1-x2-...-xn`) when preparing any query for submission.

---

## Files Reviewed (Read-Only; None Modified)

- `Week_01/DATA_QUALITY_SUMMARY.md`
- `Week_01/Function_0[1-8]/05_Documentation/data_quality_report.md`
- `My_Capstone_1_Imperial/Week_01/Function_05/03_Data/{initial_outputs.npy, verified_cumulative_outputs.npy, provenance.json, README.md}`
- `My_Capstone_1_Imperial/Week_01/Function_05/04_Results/{summary.json, observations.csv}`
- `My_Capstone_1_Imperial/Week_01/Function_05/{README.md, 06_Documentation/README.md, 07_Reflection/Reflection.md, 02_Code/README.md, 01_Notebook/Week_01_Function_05.ipynb}` (notebook checked via text search only)
- `My_Capstone_1_Imperial/Week_01/Function_01/06_Documentation/methodology.md`
- `My_Capstone_1_Imperial/Week_01/Function_0[1-8]/03_Data/README.md`
- `My_Capstone_1_Imperial/Week_01/Function_0[1-8]/04_Results/summary.json` (dimensions field, plus full recursive search across all `.md`/`.json` files in each function folder for bounds/domain/range/dimension terms)
- **(Addendum, 2026-09-16)** `~/Downloads/Capstone Project FAQs (1).pdf` — official
  Imperial College Executive Education course document, supplied directly by the
  user; read in full for this addendum.

No file anywhere, including in `My_Capstone_1_Imperial`, was modified, renamed,
moved, or deleted during this review.
