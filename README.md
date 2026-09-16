# Black-Box Optimisation (BBO) Capstone Project

**Status: work in progress, not a finished result.** This README is
organised around the course's four documentation prompts. Section 4 is
explicitly a living record and will keep changing as the project
continues.

---

## Section 1: Project Overview

This project tackles eight "black-box" optimisation problems — situations
where you can query a system for a result but never see its inner
workings, much like tuning a manufacturing process or a drug formulation
through trial and observation alone. Each week, one new query is submitted
per problem and a real result comes back, growing the dataset over time.

**Overall goal:** find, for each of the eight unknown functions, an input
that maximises its (unknown) output, using as few queries as possible,
without ever seeing the function's actual formula.

**Why this matters in real-world ML:** this setup mirrors a genuinely
common industrial problem — tuning something expensive to evaluate
(a manufacturing process, a drug formulation, a large model's
hyperparameters) where each "query" costs real time or money, and you
can't afford to try thousands of combinations. Bayesian optimisation, the
core technique this project builds toward, is the standard tool for
exactly this situation: it makes a principled trade-off between exploring
what's still unknown and exploiting what already looks promising, rather
than either guessing randomly or greedily chasing the best point seen so
far.

**High-level idea:** start with a small set of starting observations per
function, build a model of the unknown function informed by uncertainty
(not just a best guess), and use that model to choose the next query more
intelligently than random search — while keeping every step honest about
what the small amount of data can and can't yet support.

**Career relevance:** this project is a compact but complete rehearsal of
a workflow that shows up constantly in applied ML roles — deciding what to
try next under expensive, limited experimentation. Beyond the specific
Bayesian optimisation technique, the habits practiced here — validating
data before trusting it, distinguishing verified results from
interpretation, documenting model assumptions and known limitations
instead of hiding them, and independently re-checking work before calling
it final — are the same habits that matter in any ML role where a wrong
or overconfident conclusion has a real cost, not just an academic one.

---

## Section 2: Inputs and Outputs

**Inputs (query format):** each of the eight functions takes a vector of
real-valued coordinates, one per input dimension. Dimensionality is fixed
per function (see table below) and every coordinate must lie in the
official bound `[0.000000, 0.999999]` (i.e. ≥ 0 and strictly < 1).

| Function | Input dimensions |
|---|---|
| Function_01 | 2 |
| Function_02 | 2 |
| Function_03 | 3 |
| Function_04 | 4 |
| Function_05 | 4 |
| Function_06 | 5 |
| Function_07 | 6 |
| Function_08 | 8 |

**Submission format:** queries are submitted to the course portal as a
single string, one coordinate per dimension, each formatted to exactly six
decimal places, hyphen-separated, no brackets, no commas, no spaces:

```
0.652299-0.043775          <- Function_01 example (2 dimensions)
0.045091-0.528666-0.329265-0.105350-0.434667-0.641164   <- Function_07 example (6 dimensions)
```

**Output (response value):** a single real-valued scalar per query — the
function's "performance signal" for that input. All eight functions are
**maximisation** problems (see Section 3). Output scale varies enormously
across functions, from ~1e-16 for Function_01 up to over 1000 for
Function_05 — this is a genuine, verified property of the functions
themselves, not a data error (see `DATASHEET.md`).

**Example query → output pair** (from this project's own verified data,
Function_01, Week 1): query `0.374540-0.950714` → output
`-1.560646704467778e-117`.

---

## Section 3: Challenge Objectives

**Maximise, not minimise.** All eight functions are treated as
maximisation problems. Where a real-world analogy is naturally a
minimisation (e.g. minimising side effects) or involves a penalty, the
underlying score has already been transformed (typically by negation) so
that a higher returned value is always better — this is a confirmed course
rule, not an inference (see `CLAUDE.md` rules 17–18).

**Constraints and limitations to work within:**
- **Query budget**: only one new query per function, per week — evaluating
  the true function is treated as expensive, so a strategy that wastes
  queries on uninformative points is costly.
- **Response delay**: a submitted query's result is not available
  immediately; it returns at the end of the week/module cycle, which rules
  out any strategy relying on rapid iterative feedback within a single
  session.
- **Unknown function structure**: no formula, generator code, or explicit
  scale/units documentation exists for any of the eight functions in
  either this project or the original source project — every
  interpretation of a function's shape must come from observed data alone,
  never from ground truth.
- **Eight separate, independently-dimensioned problems**: the functions
  range from 2 to 8 input dimensions with different starting observation
  counts (10–40), and must never be mixed, pooled, or cross-referenced —
  each is modelled entirely separately (`CLAUDE.md` rule 2).
- **Small-sample regime throughout**: even after several rounds, sample
  sizes remain small relative to dimensionality for the higher-dimensional
  functions, which limits how much confidence any fitted model's
  uncertainty estimates should be given.

---

## Section 4: Technical Approach (Living Record)

**This section is explicitly a living record and will be updated as the
project's approach evolves.** As of this writing, it covers three rounds
of work — but an important clarification comes first.

### Important clarification on "three query submissions"

Only **one real query has ever been generated for live submission** in
this project: the Week 1 random-search baseline
(`Week_01/Function_0X/03_Queries/week_01_query.txt`). No second or third
live query has been produced from this repository. What has actually
progressed through three rounds is a separate, clearly-labelled
**retrospective learning track** (`Historical_Replay/`), built entirely on
genuine historical query-output data imported and independently verified
from the original source project — never fabricated, never conflated with
this project's own live submissions (`HISTORICAL_REPLAY_PROTOCOL.md` Rule
4). The strategy narrative below describes that track's real progression,
since it is the actual technical work completed so far.

### Round 1 (Week 1) — Random search baseline

- **Method**: uniform random sampling from `[0.000000, 0.999999]^d` via a
  seeded generator (`seed = 42 + function_number`), with redraw-on-collision
  for bounds violations or exact duplicates.
- **Rationale**: with no model fitted yet and only a small starter dataset
  available, this establishes the naive baseline that later,
  model-informed strategies are measured against — not a serious
  optimisation strategy in itself.
- Full detail: `Week_01/WEEK_01_METHOD.md`, `Week_01/REFLECTION.md`.

### Round 2 (Historical Replay Week 2) — First Gaussian Process surrogate

- **Method**: fit a `GaussianProcessRegressor` per function, comparing an
  RBF kernel against a Matérn kernel (nu=2.5), each combined with a
  `ConstantKernel` and a `WhiteKernel` for observation noise. Output
  standardised (zero mean, unit variance) before fitting; all
  reported/plotted values correctly inverse-transformed. No acquisition
  function applied — this round stopped at building and visualising the
  surrogate's posterior mean and uncertainty.
- **Why a GP and not, say, a linear or logistic regression, or an SVM**:
  these functions show non-uniform, non-linear structure (confirmed by the
  GP fits' own varying length-scales across dimensions), output scales
  spanning many orders of magnitude, and evidence of observation noise
  (Function_05's near-duplicate query pair with a non-zero output
  difference) — a plain linear model would violate the constant-effect and
  homoscedastic-noise assumptions visibly for several functions (see
  `Historical_Replay/Week_02/STRATEGY_REFLECTION.md` for the detailed
  case-by-case argument, including where a kernel SVM was considered as a
  complementary classifier for high/low regions, not a replacement for the
  GP's calibrated uncertainty).
- Full detail: `Historical_Replay/Week_02/STRATEGY_REFLECTION.md`,
  `Historical_Replay/Week_02/REMEDIATION_REPORT.md`.

### Round 3 (Historical Replay Week 3) — Convergence-aware model selection and diagnostics

- **Method**: same GP setup as Round 2, but the kernel-selection decision
  became a formal, programmatic rule instead of "whichever has higher
  likelihood" — applied to all eight functions, not just the one that had
  previously broken: *prefer the kernel with fewer failed optimizer
  restarts; if both converge cleanly, prefer higher log-marginal-likelihood;
  if neither converges reliably, do not select either as "the model."*
  New diagnostics were added: observed-vs-predicted plots (explicitly
  labelled in-sample only — no held-out validation exists anywhere in this
  project yet), residual plots, and a cumulative-best-observed-value
  tracker.
- **A genuine, not-hidden limitation surfaced this round**: Function_05's
  optimizer instability got *worse* with more data, not better — both
  kernels showed non-convergence this round (Week 2: only RBF had 1 of 6
  restarts fail; Week 3: RBF 3 of 6, Matérn 5 of 6). This is documented
  prominently, not smoothed over.
- Full detail: `Historical_Replay/Week_03/STRATEGY_REFLECTION.md`,
  `Historical_Replay/Week_03/WEEK_03_METHOD.md`,
  `Historical_Replay/Week_03/CODE_REVIEW_SUMMARY.md`.

### How exploration and exploitation are balanced

No acquisition function has been implemented yet, so this balance has not
been *enacted* in an actual query choice — but the diagnostics built so
far are explicitly aimed at supporting that decision honestly once it is
made:
- The cumulative-best-observed plots show which functions' running best is
  still moving (favouring exploration) versus plateaued (where exploiting
  a known-good region becomes more defensible).
- The GP's own posterior uncertainty remains large almost everywhere at
  current sample sizes, which is itself evidence that a genuinely balanced
  strategy should still lean toward exploration for most functions rather
  than aggressively exploiting the current best point — Function_05 is the
  clearest cautionary example, where the best-observed value is
  dramatically higher than anything else, but the model *around* that
  point is currently the least numerically reliable in the whole project.

### What makes this approach thoughtful, not just mechanical

Every model-fit result in this project has gone through an **independent
re-verification step before being treated as final**, and that process has
caught real problems that a single confident pass would have missed: a
missing reproducibility step (a notebook claimed to save its output but
didn't), a checksum-transcription error, and an initial misattribution of
which GP kernel caused a set of convergence warnings. The project has
consistently chosen to document what a result *cannot yet support* (no
generalisation claim without held-out validation, no "this dimension is
irrelevant" claim without more data, no "reliable model" claim for
Function_05) rather than rounding an uncertain result up to a confident
one.

---

## Repository Structure

```
BBO_Project_Rebuild/
├── README.md                          — this file
├── DATASHEET.md                       — data description, limitations, context
├── MODEL_CARD.md                      — model behaviour, assumptions, limitations
├── CLAUDE.md                          — permanent project rules
├── HISTORICAL_REPLAY_INVENTORY.md     — inventory of the original project's historical records
├── HISTORICAL_REPLAY_PROTOCOL.md      — rules governing use of historical data
├── Week_01/                           — Week 1: random-search baseline (real submission track)
│   └── Function_01..08/
│       ├── 01_Notebook/               — analysis notebooks
│       ├── 02_Data/                   — starter datasets
│       ├── 03_Queries/                — submitted queries
│       ├── 05_Documentation/          — data-quality reports
│       └── 06_Code_Review/            — code review records
└── Historical_Replay/                 — retrospective learning track (see Section 4)
    ├── Week_01/                       — genuine historical Week 1 pairs (verified imports)
    ├── Week_02/                       — cumulative data + first GP surrogate notebooks
    └── Week_03/                       — cumulative data + convergence-aware GP diagnostics
```

## Data Availability

Raw starter datasets (`.npy` files) are small (a few KB each) and are
included directly in this repository under each function's `02_Data/`
folder — no external data link is required.

## Methodology, Validation, and Documentation

Every substantive step in this project — data audits, methodology
verification, code review, remediation, and independent re-evaluation — is
recorded in its own dated document alongside the code it covers, following
this project's permanent rules in `CLAUDE.md`. See `DATASHEET.md` for data
details and `MODEL_CARD.md` for the current model's behaviour, assumptions,
and limitations.

## Current Best Observed Result, Per Function (Week 1 Starter Data)

These are the best values observed in each function's original starter
dataset (`Week_01/Function_0X/02_Data/initial_*.npy`) — not yet informed by
any model-guided query:

| Function | Dimensions | Best output observed | Best input observed |
|---|---|---|---|
| Function_01 | 2 | 7.710875e-16 | [0.731024, 0.733000] |
| Function_02 | 2 | 0.611205 | [0.702637, 0.926564] |
| Function_03 | 3 | -0.034835 | [0.492581, 0.611593, 0.340176] |
| Function_04 | 4 | -4.025542 | [0.577766, 0.428772, 0.425826, 0.249007] |
| Function_05 | 4 | 1088.859618 | [0.224189, 0.846480, 0.879484, 0.878516] |
| Function_06 | 5 | -0.714265 | [0.728186, 0.154693, 0.732552, 0.693997, 0.056401] |
| Function_07 | 6 | 1.364968 | [0.057896, 0.491672, 0.247422, 0.218118, 0.420428, 0.730970] |
| Function_08 | 8 | 9.598482 | [0.056447, 0.065956, 0.022929, 0.038786, 0.403935, 0.801055, 0.488307, 0.893085] |
