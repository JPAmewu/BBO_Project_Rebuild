# Model Card — BBO Capstone Project

Following the spirit of Mitchell et al.'s "Model Cards for Model
Reporting," this document describes the models used or explored in this
project so far, their intended use, assumptions, limitations, and
interpretability considerations.

## Model Details

Two distinct "strategies" have been implemented and independently
verified so far. Neither is a single fixed artefact — each function
(Function_01–08) has its own separate baseline and, where applicable, its
own separately-fitted surrogate model. Models are never shared or mixed
across functions (`CLAUDE.md` rule 2).

### 1. Random-search baseline (used for the live Week 1 submission)

- **Method**: uniform random sampling from the official input domain
  `[0.000000, 0.999999]^d`, via `numpy.random.default_rng(42 +
  function_number)`.
- **Constraints enforced**: every candidate is rejected and redrawn (from
  the same seeded generator, not a fresh one) if it would round to `1.0`
  or exactly duplicate an existing observation.
- **Status**: this is the strategy actually used for this project's real
  Week 1 portal submission. It is not a "model" in the statistical sense —
  it encodes no belief about the function's shape — and is retained
  specifically as the naive baseline that later, model-informed strategies
  should be compared against.

### 2. Gaussian Process surrogate (exploratory, retrospective-learning track only)

- **Method**: `sklearn.gaussian_process.GaussianProcessRegressor`, fit
  separately per function on that function's cumulative data (original
  starter set plus, in this exploratory track, one appended historical
  observation).
- **Kernel**: two variants fit and compared per function —
  `ConstantKernel * RBF + WhiteKernel` and
  `ConstantKernel * Matern(nu=2.5) + WhiteKernel`.
- **Output handling**: the output is standardised (zero mean, unit
  variance) before fitting, since raw output scales vary by many orders of
  magnitude across functions; all reported/plotted values are correctly
  inverse-transformed back to the original scale.
- **Hyperparameter fitting**: maximum marginal likelihood, `random_state =
  42 + function_number`, `n_restarts_optimizer = 5`.
- **Status**: this model has been fit and its posterior (mean and
  uncertainty) visualised, but **has not yet been used to generate or
  submit any query** — no acquisition function has been applied. It exists
  only within the separate `Historical_Replay/Week_02/` retrospective
  track, fit on genuine historical data, not on live portal feedback.

## Intended Use

The random-search baseline is intended only as a naive comparison point,
not a serious optimisation strategy. The GP surrogate is intended as the
foundation for future acquisition-function-driven querying (Expected
Improvement or Upper Confidence Bound, not yet implemented) once
sufficient data exists per function to trust its uncertainty estimates.
Neither model is intended for use outside this specific project's eight
functions, and neither should be treated as validated for production or
high-stakes decisions — this is an academic, small-sample exploratory
exercise.

## Training/Fitting Data

Described fully in `DATASHEET.md`. In brief: 10–40 starter observations
per function (varies by function), plus, for the GP surrogate work only,
one additional genuine historical observation per function drawn from the
retrospective learning track. The random-search baseline uses no training
data at all beyond checking for duplicates and bounds.

## Evaluation

No formal quantitative evaluation (e.g. held-out validation, calibration
check, or cross-validation) has been performed on the GP surrogate models
yet — this is explicitly disclosed in every Week 2 notebook's own
interpretation text: kernel comparisons (RBF vs. Matérn, by
log-marginal-likelihood) are described as "exploratory evidence, not a
validated model-selection result," given the very small sample sizes
involved (11–41 points across 2–8 dimensions).

## Assumptions and Limitations

- **Noiseless-vs-noisy assumption**: a `WhiteKernel` noise term is
  included in every GP fit as a deliberate, documented choice, motivated by
  direct evidence of possible observation noise (Function_05's
  near-duplicate query pair with a non-zero output difference) — not
  assumed away, but also not proven to be homoscedastic or well-calibrated
  with this little data.
- **Small-sample instability**: Function_05's RBF-kernel fit showed
  genuine L-BFGS-B optimizer non-convergence (1 of 6 restarts for RBF; 3 of
  6 for Matérn) — documented transparently, with a convergence-aware
  selection rule applied (prefer the kernel with fewer non-converged
  restarts) rather than defaulting to raw log-marginal-likelihood, which
  would have picked the less numerically reliable fit for this function.
  This is the clearest example in the project so far of a model result that
  should not be trusted with the same confidence as the other seven
  functions' fits.
- **ARD length-scale saturation**: several functions had at least one
  input dimension's fitted length-scale converge to the search-space upper
  bound (e.g. Function_01's 2nd dimension, Function_07's 2nd and 3rd,
  Function_08's 8th) — indicating the current data doesn't constrain the
  model's belief about that dimension much, which should not be
  over-interpreted as "the function truly doesn't depend on that
  dimension."
- **No acquisition function implemented yet** — this model card describes
  a surrogate, not yet a complete optimisation strategy.
- **Reproducibility**: seeds are fixed and recorded throughout, and
  library versions used for the GP work are pinned in
  `Historical_Replay/Week_02/requirements-lock.txt`; exact bit-for-bit
  reproducibility across different environments is not otherwise
  guaranteed.

## Interpretability Considerations

Each function's GP notebook includes per-dimension "1D slice" plots —
varying one input dimension at a time while holding the others fixed at
the current best-observed point, and showing the posterior mean with an
uncertainty band. This serves a similar interpretive role to individual
coefficient effects in a linear model: it shows, dimension by dimension,
where the model currently believes the function is sensitive versus flat,
and — critically — where that flatness is a genuine property of the data
versus simply a lack of information so far (see
`Historical_Replay/Week_02/STRATEGY_REFLECTION.md` for a fuller discussion,
including a comparison against what a simple linear/logistic model would
assume and where those assumptions would likely be violated for these
functions).

## Ethical and Safety Considerations

The functions used in this project are synthetic academic exercises with
no direct real-world deployment; there is no personal data involved and no
plausible harm pathway from misuse of these specific models. The main
"safety" consideration is intellectual honesty in reporting: this project
has deliberately avoided overstating model reliability (e.g. flagging
Function_05's convergence issue prominently rather than hiding it,
disclosing when a review's own earlier finding was itself wrong and
correcting it) — a norm this model card continues.
