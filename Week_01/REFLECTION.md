# Week 1 Reflection — First Formal Submission

**Scope:** Reflection on the strategy behind Week 1's query submission for
all eight functions (Function_01–Function_08). This accompanies the
already-generated, already-verified queries in each function's
`03_Queries/week_01_query.txt`, and the methodology in `WEEK_01_METHOD.md`.

## Portal Reflection Questions — Direct Answers

### What was the main principle or heuristic you used to decide on each query point?

Honestly: **none of exploitation, uncertainty-guided exploration, or
deliberate diversity-maximisation** — because none of those are available
yet. Exploitation requires a model of where the output is likely to be high;
uncertainty-guided exploration requires a surrogate's predictive variance;
both require a fitted model, and at Week 1 there is no model, only each
function's small, fixed starter dataset. Building a heuristic on top of that
data this early would have meant either overfitting to 10–40 points with no
uncertainty quantification, or dressing up a guess as a strategy.

So the actual principle was **unbiased uniform coverage of the declared
search space, with a hard duplicate/bounds guarantee and full
reproducibility** — closest to "diversity of samples" in spirit, but without
any explicit diversity *objective* (like maximin distance from existing
points); it's simple uniform random sampling via a seeded generator, with
the sole non-random logic being: reject and redraw (from the same seeded
sequence, not a fresh one) if a draw would violate the `[0.000000, 0.999999]`
bound or exactly duplicate an existing observation. The deliberate choice was
to treat Week 1 as the **naive baseline** the rest of the project measures
itself against, rather than to smuggle in an ungrounded heuristic and call
it informed.

### Which function(s) were most challenging to query, and why? What additional information would have helped?

**Function_05** was the hardest to reason about, though not to actually
query. Its output scale (up to ~1088 in the starter data, later corroborated
by the course FAQ's own worked example showing an even larger value) is 2–3
orders of magnitude beyond every other function, and a near-duplicate point
surfaced later in the historical record with a ~0.006 output difference at
what appears to be essentially the same input. Without the function's actual
formula, there's no way to tell whether that gap is genuine local curvature,
noise, or an artifact — a random query into that space is easy to generate,
but hard to interpret.

**Function_07 and Function_08** (6D and 8D respectively) were the hardest in
a different sense: a single uniform random point in a high-dimensional unit
cube covers almost none of the space, and with only 30–40 starter
observations, the sparsity is severe. Nothing about the query generation
itself was difficult, but it's clear that random search will scale worst on
exactly these two functions once compared against a model-based strategy.

**What would have helped most:** a documented bounds specification for every
function up front — only Function_01 had one in the original source
project; the shared `[0, 1)` bound for the rest had to be confirmed later
against the external course FAQ rather than being available at the point of
first querying. The actual mathematical formulas (available for none of the
eight functions) would obviously help most of all, but that defeats the
point of a black-box exercise, so the more realistic gap is a stated scale/
units expectation per function, so a value like Function_05's isn't
ambiguous between "correct" and "erroneous" on sight.

### How do you plan to adjust your strategy in future rounds based on current performance or uncertainty levels?

Once each function has two or more real observations, the plan is to fit a
Gaussian Process surrogate (RBF first, Matérn as a comparison) per function
and switch from uniform random draws to an acquisition-function-driven
choice — starting with Expected Improvement or Upper Confidence Bound, since
both give an explicit, tunable exploration/exploitation trade-off that
random search structurally cannot. Concretely:

- **High-uncertainty regions** (wherever the GP's posterior variance is
  large, which at this stage is nearly everywhere) will be favoured early,
  since there's almost no informed way to exploit yet.
- **As the dataset grows**, especially for Function_07/Function_08 where
  coverage is sparsest, the acquisition function's exploration weight (e.g.
  UCB's `kappa` or EI's `xi`) will need to be set deliberately high enough to
  compensate for the curse-of-dimensionality sparsity, rather than reusing
  the same setting across all eight functions regardless of dimensionality.
- **For Function_05 specifically**, given the unresolved output-scale
  question, standardising or log-transforming its output before fitting a
  GP is a live option — but only after checking whether that near-duplicate
  point's ~0.006 gap is noise (in which case a GP with an explicit noise
  term is the right fix) or real curvature (in which case a transform could
  hide genuine structure). That decision is deferred, not resolved, until
  more observations exist.

## What Guided the Decision

For this first submission, the strategy was **pure random search**, not
Bayesian optimisation. This was a deliberate choice, not a default: with only
each function's small starter dataset available (10–40 points depending on
the function) and no model fitted yet, there was nothing for a surrogate
model to meaningfully learn from that a single random draw wouldn't already
cover. Random search here serves as the **naive baseline** that later weeks'
Bayesian optimisation results can be compared against — without it, it would
be impossible to tell whether a GP-guided acquisition strategy in Week 2
onward is actually adding value, or just getting lucky.

Three things guided how the random query was generated, specifically:

1. **Reproducibility.** Each function's query was drawn from
   `numpy.random.default_rng(42 + function_number)` — a fixed, documented
   seed per function, never reseeded mid-generation. Anyone re-running the
   notebook gets the identical query back. This mattered more than it might
   seem: without a recorded seed, "why did you query this point" has no
   defensible answer beyond "the random number generator said so," which is
   a poor basis for a submission that's supposed to demonstrate a reasoned
   strategy, even a baseline one.
2. **Compliance with the official bounds.** The course's own FAQ document
   confirms every coordinate must lie in `[0.000000, 0.999999]` for all
   eight functions, and the submission format requires exactly six decimal
   places, hyphen-separated. Both were enforced programmatically (not just
   asserted) before any query was accepted, including a redraw-from-the-same-generator
   loop if a rounded draw ever landed on `1.000000` or exactly
   reproduced an existing observation.
3. **No leakage.** Only that function's own `initial_inputs.npy`/
   `initial_outputs.npy` were used to check for duplicates — nothing from
   later weeks, nothing from the original source project's historical
   record, even though that record was independently inventoried in
   parallel this week for a separate purpose (see `HISTORICAL_REPLAY_INVENTORY.md`).
   Keeping those two efforts strictly separate was a conscious decision, not
   an oversight.

## What Proved Difficult

A few things did not go smoothly on the first attempt, and each one changed
the approach:

- **Assuming uniform structure across functions.** Early in the process it
  was tempting to assume all eight functions shared the same starter dataset
  size (10 observations) and the same input-domain bounds. Neither held.
  Functions 3–8 have native starter counts of 15–40, and only Function_01
  originally had a written bounds specification anywhere in the source
  project — the `[0, 1)` bound for the other seven had to be confirmed
  later against the official course FAQ rather than assumed by analogy.
  Building in an explicit stop-and-ask step (rather than silently forcing a
  uniform assumption) caught this before it produced incorrect files.
- **Reproducibility gaps that weren't obvious until reviewed.** A code
  review found that every one of the eight notebooks printed its final
  query string but never actually wrote it to disk — the saved `.txt` files
  were correct, but only because they'd been populated out-of-band. Running
  the notebook top-to-bottom would not have regenerated its own claimed
  output. This is an easy mistake to make when a script "prints the right
  answer" and it's tempting to consider the job done; the fix (an explicit
  file-write cell, then a full re-execution and byte-for-byte before/after
  checksum comparison) was straightforward once caught, but wouldn't have
  been caught without a dedicated review pass looking specifically for it.
- **Interpreting scale differences across functions.** Function outputs span
  from ~1e-16 (Function_01) to over 1000 (Function_05) to compact positive
  ranges (Function_08). Early on it was unclear whether Function_05's large
  magnitude was a data-quality problem or a genuine feature of that
  function. It turned out to be genuine — confirmed by the FAQ's own worked
  example showing a similarly large Function 5 value — but resolving that
  took an explicit investigation rather than being obvious from the data
  alone. This is a reminder that a "weird-looking" value isn't automatically
  a bug.

## What I Plan to Adjust Next

- **Introduce an actual surrogate model.** Week 1's queries are
  intentionally naive. From Week 2 onward, once a second real observation
  per function exists, a Gaussian Process surrogate (RBF and Matérn kernels)
  with an acquisition function (starting with Expected Improvement or UCB)
  should replace pure random search, and the random-search results should
  be kept as the ongoing comparison baseline, not discarded.
- **Watch the near-duplicate issue.** While preparing separate historical
  cumulative datasets this week, two functions (Function_05, Function_06)
  showed queries extremely close to existing points — in Function_06's
  case, one coordinate differed by almost exactly a factor of 10, which
  looks like a decimal-place issue rather than a genuinely new point. That
  was in historical data, not this week's own submission, but it's a
  reminder to sanity-check future submitted queries against existing points
  more carefully than a simple exact-match duplicate filter, since a
  near-duplicate can slip past rounding-based checks.
- **Keep the validation habit, not just the validation.** The heaviest time
  cost this week wasn't generating the queries — it was independently
  verifying them (data quality audits, code review, regression checks) after
  the fact. That discipline caught real issues (the missing file-write step,
  the checksum-transcription error in one of my own reports) that would
  otherwise have gone unnoticed. The plan is to keep that same
  build-then-independently-verify pattern for every future week, even once
  the novelty of a first submission wears off.

## Distinguishing Verified Facts from Interpretation

Everything above describing what was actually done, what checks passed or
failed, and what the code review/audits found is **verified** — traceable to
this project's own audit trail (`Week_01/CODE_REVIEW_SUMMARY.md`,
`Week_01/INDEPENDENT_EVALUATION.md`, `Week_01/PRE_MODELLING_VERIFICATION.md`).
The assessment of *why* certain functions behave the way they do (e.g.
whether Function_05's near-duplicate output gap reflects genuine curvature
vs. noise) remains **interpretation** — explicitly flagged as unverifiable
without querying the black-box function directly, and should not be read as
established fact.
