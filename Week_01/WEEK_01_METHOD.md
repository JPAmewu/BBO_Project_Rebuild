# Week 1 Method — Random-Search Baseline (Functions 01–08)

**Scope:** This document describes the methodology used to produce the Week 1
random-search baseline query for each of the eight black-box functions
(`Function_01` through `Function_08`). It accompanies the eight per-function
notebooks at `Function_XX/01_Notebook/week_01_random_baseline.ipynb` and the
eight query files at `Function_XX/03_Queries/week_01_query.txt`.

---

## 1. Why a random-search baseline

All eight functions are treated as maximisation problems over black-box
objectives with no available closed-form expression, gradient, or generating
code (confirmed by `Week_01/PRE_MODELLING_VERIFICATION.md`, item 4.5 — no
formula or generator exists in either project repository for any of the
eight functions). Before any surrogate-model-based search (e.g. Gaussian
Process-based Bayesian optimisation, expected in later weeks) can reasonably
be introduced and evaluated, a naive, assumption-free baseline is needed as
a point of comparison. Uniform random sampling within the confirmed input
domain is the simplest such baseline: it uses no information from the
already-observed data beyond confirming the domain and avoiding exact
duplicate points, so any later Bayesian optimisation method can be judged
against how much it improves over pure random search on the same functions.
This is standard practice in the black-box/Bayesian optimisation literature,
where random search is the conventional "no-model" reference point.

## 2. Seed policy

- Each function's single new query for Week 1 is generated from its own
  independent `numpy.random.default_rng` instance.
- The seed is deterministic and derived from the function number:
  `seed = 42 + function_number`. Concretely:

  | Function | Seed |
  |---|---|
  | Function_01 | 43 |
  | Function_02 | 44 |
  | Function_03 | 45 |
  | Function_04 | 46 |
  | Function_05 | 47 |
  | Function_06 | 48 |
  | Function_07 | 49 |
  | Function_08 | 50 |

- Exactly one `Generator` instance is created per function. If a candidate
  draw is rejected (see Section 5), the **same** generator instance is used
  to draw the next candidate — the generator is never re-seeded or
  recreated mid-search. This keeps the entire draw sequence for a given
  function fully reproducible from its single seed: re-running the
  notebook with the same seed reproduces the same sequence of candidates
  and therefore the same final accepted query, regardless of how many
  candidates were rejected along the way.

## 3. Bounds policy

Every input coordinate, for every function, is constrained to
`[0.000000, 0.999999]`, i.e. each coordinate must be `>= 0` and strictly
`< 1`. This is the officially confirmed, course-wide bound documented in
`Week_01/PRE_MODELLING_VERIFICATION.md` (Section 2 and its Addendum), which
cites the official Imperial College Executive Education "Capstone Project
FAQs" document: "Input values must always lie in the range 0.000000 to
0.999999 (i.e. greater than or equal to 0 and strictly less than 1)." This
bound applies uniformly to all eight functions regardless of their
dimensionality (2 through 8), superseding the earlier per-repository finding
that only Function_01 had an explicit written bound.

Each notebook independently verifies that the existing `initial_inputs.npy`
data for its function satisfies this bound (see Section 6), and the new
query candidate is checked against the same bound before being accepted.

## 4. Leakage prevention

Only two files were read for each function: its own
`Function_XX/02_Data/initial_inputs.npy` and `initial_outputs.npy`. No other
data source was read or used to generate these queries. In particular:

- No Week_02 (or later) data was read — Week_02 does not exist in this
  project and was not created.
- No file from the original source project
  (`~/Documents/GitHub/My_Capstone_1_Imperial`) was read or referenced.
- No file under any function's own `04_Results` folder was opened or
  written to.

This ensures the Week 1 random baseline is generated purely from
information legitimately available at this stage (the domain bound and the
starter observations), with no contamination from later weeks, the original
source project, or previously recorded results.

## 5. Duplicate- and out-of-bounds-avoidance procedure

For each function, a candidate query is generated as follows:

1. Draw a raw vector `candidate = rng.random(d)` (uniform on `[0, 1)`),
   where `d` is the function's input dimensionality.
2. Round `candidate` to six decimal places.
3. Reject and redraw (from the same generator instance, without reseeding)
   if either:
   - **Out of bounds:** any rounded coordinate is `>= 1.0` (this can occur
     because a raw draw very close to 1, e.g. `0.9999996`, rounds up to
     `1.000000`, which violates the strict `< 1` requirement), or any
     coordinate is `< 0.0`; or
   - **Duplicate:** the rounded candidate vector exactly matches an
     existing row of `initial_inputs.npy`, after rounding those existing
     rows to six decimal places as well (for a fair, consistent
     comparison).
4. Repeat steps 1–3 until a candidate is accepted.

Across all eight functions, this procedure was run and the number of draws
required to reach an accepted query is recorded in each notebook (Section 5
of each notebook, cell reporting "Number of draws needed"). In this run, all
eight functions reached a valid, unique, in-bounds query on the **first**
draw (no rejections occurred for any function) — this is expected given
that each function's starter dataset has only 10–40 points scattered in a
continuous `[0,1)^d` space, so the probability of an exact collision (to six
decimal places) or of a raw draw rounding to exactly `1.0` is very small,
but the notebooks implement and would correctly apply the redraw loop had
either event occurred.

## 6. Validation checks performed in every notebook

Each of the eight `week_01_random_baseline.ipynb` notebooks performs, and
prints the actual computed result of, the following checks before any query
is generated:

- Input array shape and output array shape, cross-checked against the
  expected dimensionality for that function.
- Input and output array dtypes.
- Input dimensionality (`d`), asserted against the expected value.
- NaN and Inf counts in both the input and output arrays.
- Duplicate input row count (existing rows compared after rounding to six
  decimal places).
- Bounds compliance: confirmation that all existing input values satisfy
  `>= 0.0` and `< 1.0`, together with the observed min/max input values.

All eight notebooks were executed (not merely written) using a local Jupyter
kernel, so the printed validation results, summary statistics, and plots in
each notebook reflect real, computed values from each function's actual
`initial_inputs.npy` / `initial_outputs.npy` data — none of these results
were fabricated or estimated by inspection.

## 7. Limitations of a pure random-search baseline

- **No exploitation of observed structure.** The random query is drawn
  uniformly over the whole domain and does not use the existing
  `initial_outputs.npy` values to bias sampling toward promising regions.
- **No uncertainty modelling.** There is no surrogate model (e.g. a
  Gaussian Process) and therefore no notion of predictive uncertainty to
  guide exploration versus exploitation.
- **No detection of multi-modal structure.** A single uniform draw (or even
  a longer sequence of them) provides no mechanism to identify or
  distinguish multiple local optima; it can only ever provide a naive
  reference point against which a structure-aware method's superiority (or
  lack thereof) can later be measured.
- **One query per function this week.** Week 1's random baseline consists
  of exactly one new query per function; it is not an iterative search and
  makes no attempt to converge toward the optimum.
- **Sample-inefficiency.** Precisely because it ignores prior observations,
  random search is expected to require many more evaluations than
  Bayesian optimisation to reach a comparable output value — this is the
  intended contrast for later weeks, not a flaw specific to this
  implementation.
- **Curse of dimensionality.** For the higher-dimensional functions
  (Function_07 at 6D, Function_08 at 8D), uniform random coverage of the
  domain becomes sparser per unit hypervolume as dimensionality increases,
  further limiting how informative a single random draw can be.

These limitations are expected and intentional: Week 1's purpose is only to
establish a naive, transparent, fully reproducible comparison point, not to
approach optimality.
