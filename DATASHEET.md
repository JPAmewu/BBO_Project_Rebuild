# Datasheet — BBO Capstone Project

Following the spirit of Gebru et al.'s "Datasheets for Datasets," this
document describes the data used in this project: its motivation,
composition, collection process, known limitations, and recommended uses.

## Motivation

**For what purpose was the dataset created?** To support an academic
capstone project on Bayesian black-box optimisation: eight synthetic
functions, each representing a different real-world-style optimisation
problem (e.g. manufacturing yield, hyperparameter tuning, multi-criteria
design), where the true mathematical formula is deliberately withheld from
the participant. The dataset consists of an initial set of "starter"
input-output observations per function, later grown by one new
query-output pair per week.

**Who created it, and on whose behalf?** The starter data and the
week-by-week returned outputs originate from the course's capstone
platform (Imperial College Executive Education), not from this project.
This repository only consumes that data; it does not generate or control
the underlying functions.

## Composition

**What does the dataset consist of, and how many instances?** For each of
eight functions (`Function_01`–`Function_08`), a starter set of input
vectors and their corresponding output values:

| Function | Dimensionality | Starter observations |
|---|---|---|
| Function_01 | 2 | 10 |
| Function_02 | 2 | 10 |
| Function_03 | 3 | 15 |
| Function_04 | 4 | 30 |
| Function_05 | 4 | 20 |
| Function_06 | 5 | 20 |
| Function_07 | 6 | 30 |
| Function_08 | 8 | 40 |

Each input vector's coordinates lie in `[0.000000, 0.999999]` (confirmed
official course-wide bound; see `Week_01/PRE_MODELLING_VERIFICATION.md`).
Outputs are single real-valued scalars, all functions being maximisation
problems.

**Does the dataset contain sensitive information?** No. All values are
numeric optimisation inputs/outputs; no personal, confidential, or
identifying information is present in any file (confirmed during every
data-quality audit conducted for this project).

## Collection Process

**How was the data collected?** The starter datasets were provided
directly by the course platform as `.npy` files. Weekly additional
observations are obtained by submitting one query per function through the
capstone portal and receiving back the corresponding output.

**Is there a retrospective/historical component?** Yes — this repository
also includes a separate, clearly-labelled `Historical_Replay/` track that
imports genuine historical query-output pairs from an earlier run of this
same capstone project (found in the original, pre-rebuild source
repository), used only for retrospective learning and methodology
validation, never conflated with this project's own live weekly
submissions. See `HISTORICAL_REPLAY_PROTOCOL.md` for the full rule set
governing this track, including why certain historical weeks (Week 12) are
quarantined due to internally-contradictory source records, and why
Week 13 is treated as proposal-only.

## Preprocessing / Cleaning / Labelling

**Was any preprocessing done?** No values were altered, imputed, or
removed. All data-quality checks performed (shape, dtype, bounds,
duplicates, NaN/Inf, finiteness) were verification-only — see each
function's `05_Documentation/data_quality_report.md` for the full audit
trail. The only "processing" applied anywhere is presentational: query
values are formatted to exactly six decimal places, hyphen-separated, for
submission — a text-formatting step that never changes the underlying
numeric value (verified explicitly wherever it was applied, e.g.
`Historical_Replay/Week_01/Function_0X/pair_provenance.md`).

## Uses

**What other tasks could this dataset be used for?** This dataset is
purpose-specific to this capstone's eight optimisation problems; it is not
designed or recommended for any other task, since the functions' true
definitions are unknown and the dataset is deliberately small.

**Are there risks or limitations a future user should be aware of?**
- **The true mathematical formula for every function is unknown.** No
  generator code or explicit formula exists anywhere in either this
  project or the original source project. All interpretation of function
  behaviour is based purely on observed data, never on ground truth.
- **Output scales vary by many orders of magnitude across functions**
  (from ~1e-16 for Function_01 to over 1000 for Function_05) — this is
  confirmed genuine (not a data error), corroborated by the official
  course FAQ's own worked example, but should be accounted for explicitly
  in any cross-function analysis (e.g. via per-function standardisation).
- **At least one function shows evidence of observation noise.**
  Function_05 has a near-duplicate query pair with a small but non-zero
  output difference (~0.006) at essentially the same input — consistent
  with either genuine stochastic noise or high local curvature; this
  cannot be distinguished without querying the true function again.
- **Sample sizes remain small relative to dimensionality** for several
  functions. As of `Historical_Replay/Week_05` (the current furthest
  point), per-function cumulative counts range from 14 (Functions 01–02,
  d=2) to 44 (Function_08, d=8) — e.g. 24 observations for a
  4-dimensional function (Function_05), 44 for an 8-dimensional one
  (Function_08) — which limits the reliability of any fitted model's
  uncertainty estimates, especially for the higher-dimensional functions.
  This range grows by one observation per function each completed
  `Historical_Replay` week; see that week's own `CODE_REVIEW_SUMMARY.md`
  for the exact current counts.
- **Historical data quarantine**: within the historical retrospective
  track only, Week 12's source records directly contradict each other
  about whether a genuine return exists — this week is explicitly
  excluded from use until independently resolved (see
  `HISTORICAL_REPLAY_INVENTORY.md` and `HISTORICAL_REPLAY_PROTOCOL.md`
  Rule 7).

## Distribution

**Will the dataset be distributed?** This repository (once pushed to
GitHub, at the user's discretion) will make the starter `.npy` files
available directly, since they are small. No separate license or
distribution restriction beyond the course's own terms applies to this
repository's use of the data.

## Maintenance

**Who maintains the dataset going forward?** The starter data is static and
provided by the course platform; it is never modified in this repository
(a permanent project rule — see `CLAUDE.md` rule 1). Weekly additional
observations are appended only after validation, and historical
observations are never altered to "correct" or improve results (`CLAUDE.md`
rule 11).
