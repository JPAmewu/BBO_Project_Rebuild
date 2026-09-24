# Week 6 Strategy Reflection

**Scope:** A self-directed strategy reflection for this week's Historical
Replay round, consolidating what Week_06's GP diagnostics and historical
pair import actually found, and updating the project's ongoing roadmap
toward acquisition-function-driven querying. No external course prompt
accompanied this round; this reflection follows the same practice used in
every prior week regardless.

**Honesty note, consistent with every prior week's reflection**: only
**one** real query has ever been generated for live submission in this
project — the Week 1 random-search baseline. No live Week_02 through
Week_06 query exists. What has actually progressed through six rounds is
the `Historical_Replay/` track, built on genuine historical data, kept
explicitly separate from any live submission.

## What This Week Actually Found

**Function_05's instability shifted, but did not resolve.** Across six
weeks of GP fitting, this function has never once shown both kernels
converge cleanly:

| Round | RBF ABNORMAL | Matérn ABNORMAL | Outcome |
|---|---|---|---|
| Week 2 | 1/6 | 3/6 | RBF selected |
| Week 3 | 3/6 | 5/6 | RBF selected (fewer failures) |
| Week 4 | 5/6 | 3/6 | Matérn selected (fewer failures) |
| Week 5 | 4/6 | 4/6 | **Neither selected — exact tie** |
| Week 6 | 4/6 | 3/6 | Matérn selected (fewer failures) |

The move from Week 5's exact tie to Week 6's unequal split is a genuine
change, not a resolution — RBF's failure count is unchanged from last
week (4/6 both times), while Matérn improved by one restart (4/6 → 3/6).
Six weeks in, this function's optimizer instability remains the clearest,
most persistent unresolved issue in the entire project.

**Function_07 flipped kernel preference again**, from Matérn (selected in
both Week_04 and Week_05) back to RBF this week. Both kernels converged
cleanly in all three weeks — this is an ordinary log-marginal-likelihood
tie-break responding to new data, not an instability finding, and the
notebook's own dynamically-computed caveat correctly confirms no
instability applies here. Worth noting only because it shows kernel
selection can be genuinely sensitive to which single new observation
arrives, even when both candidate models are numerically well-behaved.

**All other 6 functions continue to converge cleanly** for both kernels,
with selection resolved purely by likelihood comparison — this remains
the normal case, and Function_05 remains the outlier.

**Data-quality note carried forward, not yet actioned**: while
independently verifying Function_05's historical pairs this week, a
reviewer incidentally found that Function_05's **Week 9** query in the
source ledger has a coordinate exactly equal to `1.0` — a genuine
out-of-bounds value relative to the official `[0.000000, 0.999999]`
domain. This has no bearing on Week_06 and was not acted on, but it is
now recorded (in `HISTORICAL_IMPORT_VALIDATION.md` and
`Function_05/pair_provenance.md`) so it is not lost before Week 9 is
processed — at that point it will need explicit handling (most likely
quarantine, pending how the rest of that week's records read).

## What This Means for the Roadmap

Nothing here changes the next concrete step already identified across
Weeks 04-05's reflections: **introduce an acquisition function (EI or
UCB) on the current GP posterior**, optimised via multi-start
gradient-based search using the GP's own analytic posterior gradients.
This week reinforces two specific reasons that step matters:

1. **Function_05's persistent instability is itself the strongest
   argument for acquisition-driven querying over continued passive
   replay.** Six weeks of watching this function's optimizer struggle
   without a corresponding query-selection response is the clearest
   illustration in this project of *why* Bayesian optimisation exists —
   a principled acquisition function would let future queries deliberately
   target the region driving this instability, rather than simply
   accumulating whatever historical point happens to be next in sequence.
2. **Function_07's kernel-preference sensitivity to individual new
   points** is a reminder that any acquisition function built on top of
   these GPs should be recomputed fresh each week from the currently
   selected kernel, not cached — a design constraint the eventual
   acquisition-function notebook should build in from the start, since
   the "correct" kernel visibly changes with each additional observation.

Separately, the retrospective/counterfactual EI/UCB/PI analysis already
built for Weeks 02-05 (`08_Retrospective_Acquisition/`) has not yet been
extended to Week_06 — that remains available as a follow-on task if
wanted, but was not requested this round and so was not built.

## What Remains Unresolved

- Function_05's genuine convergence instability, unresolved across all
  six weeks to date, in no consistent direction.
- No acquisition function has been implemented as part of this project's
  actual (non-retrospective) weekly pipeline — the diagnostics remain
  visualisation-only, as in every prior week.
- Function_05's Week 9 out-of-bounds coordinate, flagged but deliberately
  not acted on until Week 9 is reached.
