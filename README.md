# Black-Box Optimisation (BBO) Capstone Project

## Summary (Non-Technical)

This project tackles eight "black-box" optimisation problems — situations
where you can query a system for a result but never see its inner workings,
much like tuning a manufacturing process or a drug formulation through trial
and observation alone. Each week, one new query is submitted per problem and
a real result comes back, growing the dataset over time. The project starts
with random search as a simple baseline, then introduces Gaussian Process
models — a way of learning the shape of an unknown function from limited
data while honestly tracking uncertainty — to make smarter, better-justified
choices about where to query next. Every step, including data checks, model
assumptions, and known limitations, is documented and independently
reviewed rather than taken on faith.

## Status

**This is a work in progress, not a finished result.** As of this writing:
random-search baseline queries have been generated and validated for
Week 1 for all eight functions (see `Week_01/`). A first Gaussian Process
surrogate model (comparing RBF and Matérn kernels) has been built and
independently reviewed as a preparatory exploratory step (see
`Historical_Replay/Week_02/`), using genuine historical data from a
retrospective learning track (see "Historical Replay Track" below) rather
than live portal feedback, since this repository does not yet have a
completed second live submission cycle. No acquisition-function-driven
query has been generated yet — that is the explicit next step.

## Current Best Observed Result, Per Function (Week 1 Starter Data)

All eight problems are **maximisation** problems. These are the best
values observed in each function's original starter dataset
(`Week_01/Function_0X/02_Data/initial_*.npy`) — not yet informed by any
model-guided query:

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

Negative values are expected by design for several functions (the
underlying real-world analogies involve minimisation or penalties,
transformed so higher is always better) — see `CLAUDE.md` rules 17–18 and
`HISTORICAL_REPLAY_INVENTORY.md` for the verification trail behind this.

## Why Bayesian Optimisation

Evaluating each function is expensive — one query per week — and each
function may be non-linear, noisy, or have multiple local optima. Bayesian
optimisation is well-suited to this setting because it builds a
probabilistic model of the function (a surrogate, here a Gaussian Process)
that predicts both an expected value and an uncertainty estimate, and uses
that uncertainty to explicitly balance exploring unknown regions against
exploiting regions that already look promising — rather than either
guessing randomly or greedily chasing the best point seen so far. Random
search (Week 1) serves as the naive baseline this approach is measured
against.

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
└── Historical_Replay/                 — retrospective learning track (see below)
    ├── Week_01/                       — genuine historical Week 1 pairs (verified imports)
    └── Week_02/                       — cumulative data + first GP surrogate notebooks
```

## Historical Replay Track

Because live portal feedback accumulates only one new point per week,
`Historical_Replay/` is a **separate, clearly-labelled retrospective
learning track** that imports genuine historical query-output pairs from
the original source project's own records (never fabricated, always
independently cross-checked against multiple primary source files) to
practice and validate the modelling pipeline against real, larger datasets
sooner. It is governed by its own rule set
(`HISTORICAL_REPLAY_PROTOCOL.md`) and is never conflated with, or used to
generate, this project's own live query submissions — see
`HISTORICAL_REPLAY_PROTOCOL.md` Rule 4.

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

## Challenges and Key Insights (So Far)

- **No documented bounds existed for seven of the eight functions** at the
  start of this project; the shared `[0.000000, 0.999999]` domain had to be
  confirmed against the official course FAQ before it could be relied on
  with confidence (see `Week_01/PRE_MODELLING_VERIFICATION.md`).
- **Output scales vary enormously across functions** (from ~1e-16 to
  over 1000), which affects both interpretation and any future
  cross-function modelling decisions (e.g. per-function standardisation).
- **Small-sample Gaussian Process fitting is genuinely fragile** for at
  least one function (Function_05), whose RBF kernel fit showed real
  optimizer non-convergence — documented transparently rather than
  papered over (see `Historical_Replay/Week_02/REMEDIATION_REPORT.md`).
- **Every deliverable in this project has been independently re-verified**
  by a separate review pass before being treated as final, catching real
  issues (a missing file-write step, a checksum-transcription error, an
  incorrect kernel attribution in an earlier review — a warning count was
  initially attributed entirely to the wrong GP kernel) that a single pass
  would likely have missed.
