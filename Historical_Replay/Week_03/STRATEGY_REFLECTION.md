# Week 3 Strategy Reflection

**Scope:** Reflection on this week's work, answering the module's Part 2
prompts. **Honesty note, as with Week 2's reflection:** this module's
framing assumes three real queries have been submitted and returned. In
this project, only ONE real query has ever been generated for live
submission — the Week 1 random-search baseline
(`Week_01/Function_0X/03_Queries/week_01_query.txt`). No second or third
real query has been produced. What has actually advanced across three
"rounds" is the `Historical_Replay/` track's diagnostic pipeline, built
entirely on genuine historical data (imported and independently verified
from the original source project), explicitly kept separate from the live
submission track. The answers below are grounded in that real work — the
Week 1 → Week 2 → Week 3 progression of EDA, GP surrogate fitting, and
diagnostics — not in a fictional third live submission.

## How has the query strategy changed from earlier rounds? Model predictions or still exploring? Tuned hyperparameters or heuristics?

No query has actually been chosen or submitted in any of these three
rounds beyond Week 1's random draw, so there is no real answer to "which
point did you pick and why" for rounds 2 or 3 — that step has been
deliberately deferred every time. What *has* changed is the sophistication
of the modelling machinery being built toward that eventual decision:

- **Week 1**: no model at all — pure uniform random sampling, seeded and
  bounds-checked only.
- **Week 2**: first GP surrogate fit (RBF and Matérn, compared by
  log-marginal-likelihood), posterior visualised, but no formal rule for
  choosing between the two kernels beyond "whichever has higher
  likelihood" — applied loosely, and only Function_05 got a dedicated
  convergence check after remediation.
- **Week 3**: the SAME GP setup, but now with a formal, programmatic
  convergence-aware selection rule applied to *all eight* functions (not
  just the one that broke) — instrumenting every optimizer restart and
  preferring the kernel with fewer failed convergences over raw likelihood
  when the two disagree. New diagnostics (observed-vs-predicted, residuals,
  cumulative-best-observed tracking) were added specifically to build a
  more honest picture of model reliability before any acquisition step is
  trusted.

On hyperparameters specifically: the GP's own parameters (length-scales,
signal variance, noise level) are fit automatically via maximum marginal
likelihood — not hand-tuned. But several surrounding choices remain
heuristic and human-set, not learned: which two kernel families to compare
(RBF vs. Matérn nu=2.5, not e.g. a periodic or linear kernel), the search
bounds for each hyperparameter, and the convergence-aware selection rule
itself. None of this yet constitutes "relying on model predictions" for
query choice, since no query has been chosen this way.

## How is exploration balanced against exploitation? Known-good areas or still sampling untested regions?

Still no query has been chosen, so no real exploration/exploitation
trade-off has been enacted. But the Week 3 diagnostics sharpen what that
trade-off would look like if a query were chosen today:

- The **cumulative-best-observed plot** (new this week) shows, per
  function, how the running maximum has moved as data accumulated —
  useful for spotting which functions have plateaued (little recent
  improvement, suggesting the known-good region may already be
  well-characterised) versus which are still moving (suggesting untested
  regions may still hold better values). Function_08 is a clear case where
  the newest point *did* improve the running best (9.8157 → 9.9399),
  while several other functions' running best has been static for a while.
- The **per-dimension slice plots** continue to show large uncertainty
  bands almost everywhere except near a handful of dimensions where the
  length-scale saturated at its search bound — meaning, honestly, that
  "exploitation" of a currently-best point is still only weakly justified
  by the model's own uncertainty, and a genuinely balanced strategy would
  still lean toward exploration for most functions at this sample size.
- Function_05 is the clearest counter-example: its best-observed point
  (~1088.86) is dramatically higher than anything else in its dataset, but
  this week's diagnostics show the GP fit around it is *less* numerically
  reliable than last week (both kernels now show optimizer non-convergence,
  not just one), so "exploit near the best point" is a weaker
  recommendation here than the raw output value alone would suggest.

## How would SVMs change the approach? Soft-margin SVM for high/low classification? Kernel SVM for non-linear response surfaces?

A soft-margin SVM could plausibly serve a narrower, complementary role: if
the goal is reframed as a binary question ("is this region likely to be
high-output or low-output?") rather than "predict the exact output and its
uncertainty," an SVM classifier trained on a thresholded version of the
current data (e.g. above/below median) could draw a decision boundary
between promising and unpromising regions. Given the small sample sizes
here (12–42 points depending on function), a soft margin would be
important — a hard margin would almost certainly overfit noise or the
handful of extreme points (Function_05's ~1088.86 outlier, Function_08's
newly-improved best) rather than finding a boundary that generalises.

A **kernel SVM** (e.g. RBF kernel) would matter more than a linear one for
exactly the functions this project has already flagged as non-linear: the
GP fits' own length-scales show non-uniform, direction-dependent structure
for most functions, and Function_05 in particular — with output swinging
across roughly four orders of magnitude — has no plausible linear
separating boundary between "high" and "low" regions. A kernel SVM could
likely draw a much better boundary there than a linear one, echoing the
same conclusion already reached about logistic regression in Week 2's
reflection.

The key limitation an SVM wouldn't solve: it gives a decision boundary, not
a calibrated, continuous estimate of *how* good a point is expected to be
along with an honest uncertainty estimate — which is exactly what an
acquisition function (EI, UCB) needs to rank candidate query points. An
SVM could be a useful *filter* (e.g. "don't bother querying deep inside a
region classified as reliably low") layered on top of the GP, but it isn't
a substitute for the GP surrogate as the core decision-making tool in this
project.

## What limitations of the current model become apparent as data grows? Overfitting? Irrelevant features?

Several, and this week's diagnostics made at least one of them worse, not
better:

- **Function_05's optimizer instability got worse, not better, with more
  data.** In Week 2 (21 observations) only RBF showed convergence problems
  (1 of 6 restarts). In Week 3 (22 observations, one more point) *both*
  kernels show real instability (RBF 3/6, Matérn 5/6 ABNORMAL
  terminations). More data should generally make a GP's likelihood surface
  easier to optimize, not harder — this is a real, documented limitation
  worth flagging rather than assuming will simply resolve itself with yet
  more data.
- **No overfitting check currently exists**, because no held-out or
  cross-validated evaluation has been implemented anywhere in this
  project — every observed-vs-predicted and residual plot is explicitly
  labelled in-sample only. This means the project genuinely cannot yet
  distinguish "the GP fits well" from "the GP is fitting noise it has
  already seen." That is a limitation of the current pipeline, not
  something resolved by having more data available.
- **Several dimensions across multiple functions repeatedly show
  length-scales pinned at the search-space upper bound** (Function_01's
  2nd dimension, Function_03's 1st and 2nd, Function_07's 2nd and 3rd,
  Function_08's 8th, consistently across both Week 2 and Week 3). This
  could mean those dimensions are genuinely close to irrelevant — or it
  could simply mean there still isn't enough data to detect a real
  dependence. The two explanations are not yet distinguishable, and this
  project has consistently avoided asserting the stronger claim
  ("the function doesn't depend on this input") without more evidence.
- **Sample-to-dimension ratio remains tight for the higher-dimensional
  functions** even after this week's append (Function_07: 32 points for
  6 dimensions; Function_08: 42 points for 8 dimensions) — enough to fit a
  GP, but not enough to be confident the fitted length-scales represent
  the true underlying structure rather than an artefact of this particular
  small sample.

## How does this prepare you to think like a data scientist facing incomplete knowledge elsewhere?

The most transferable habit this project has reinforced is treating
"the model says X" and "X is actually true" as different claims that need
to be kept separate, and building that separation into the process itself
rather than trusting it to memory. Concretely: every model-fit result in
this project has gone through an independent re-verification step before
being treated as final, and that process has genuinely caught real
problems — a missing reproducibility step, a checksum-transcription error,
an initial mis-attribution of which kernel caused a set of convergence
warnings. None of those would have surfaced from a single confident pass.
The other transferable lesson is honesty about the boundary of what
small, incomplete data can support: this project's diagnostics repeatedly
say what they *cannot* yet conclude (no generalisation claim without
held-out validation, no "irrelevant dimension" claim without more data, no
"reliable model" claim for Function_05 given its convergence instability)
rather than rounding an uncertain result up to a confident one. That
discipline — verify before trusting, and state the limits of what the
current evidence actually supports — is exactly what's needed when facing
incomplete knowledge in any other project, not just this one.
