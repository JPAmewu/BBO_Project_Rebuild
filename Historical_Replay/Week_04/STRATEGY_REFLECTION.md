# Week 4 Strategy Reflection — Neural Network Hyperparameters and This Project

**Scope:** Reflection for Required Capstone Components 15.1 and 15.2.
**Honesty note on 15.1, consistent with every prior round's reflection**:
this component assumes a fourth real query has been submitted. In this
project, only **one** real query has ever been generated for live
submission — the Week 1 random-search baseline. No second, third, or
fourth live query exists. What has actually progressed through four rounds
is the `Historical_Replay/` track, built on genuine historical data,
explicitly kept separate from live submissions (see every prior week's
reflection for the same caveat). Component 15.2's topic — neural network
hyperparameters — is addressed below on its own terms, since it doesn't
depend on that distinction.

## This Project Has Never Used a Neural Network

Worth stating plainly before reflecting: every model built in this project
so far is a **Gaussian Process**, not a neural network. No NN has been
trained, tuned, or even considered as a candidate model anywhere in
`Week_01/` or `Historical_Replay/`. So this reflection connects general
knowledge of NN hyperparameters to the modelling work actually done here,
rather than reporting on NN experiments that don't exist in this project.

## How Neural Network Hyperparameters Affect Model Behaviour

A few of the most consequential NN hyperparameters, and what they control:

- **Learning rate**: too high and training diverges or oscillates; too low
  and training crawls or gets stuck in a poor local minimum before the
  budget runs out. This is fundamentally a trade-off between optimisation
  speed and stability.
- **Network depth/width**: more capacity lets the model fit more complex
  functions, but with a small dataset it also increases the risk of
  memorising noise rather than learning genuine structure — the classic
  bias-variance trade-off.
- **Regularisation (weight decay, dropout)**: deliberately limits how much
  the model can rely on any single feature or unit, trading some training
  accuracy for better generalisation.
- **Batch size and number of epochs**: control how much and how often the
  model sees the data before a hyperparameter or convergence decision is
  made; too few epochs risks stopping before the model has actually
  learned anything useful, too many risks overfitting.
- **Random initialisation / seed**: neural network training is
  non-convex, so different random starts can converge to meaningfully
  different results — which is why multiple restarts (or at least a fixed,
  recorded seed) matter for reproducibility and for trusting any single
  training run.

## How These Insights Connect to This Project's Own Modelling Work

Even though this project uses a Gaussian Process rather than a neural
network, several of the same underlying concerns show up in the GP
hyperparameter-fitting work already done in `Historical_Replay/Week_02`,
`Week_03`, and `Week_04`, and reasoning about NN hyperparameters
sharpens how to think about them:

- **The GP's own hyperparameters (kernel length-scales, signal variance,
  noise level) play the same role as an NN's weights and regularisation
  strength** — they control how flexible the fitted model is allowed to
  be, and they are fit automatically (via maximum marginal likelihood)
  rather than hand-set, in the same way NN weights are fit via gradient
  descent rather than hand-set. The `WhiteKernel` noise term is a close
  analogue to NN regularisation: both exist specifically to stop the model
  from treating every data point as noise-free ground truth, which matters
  more, not less, with a small dataset.
- **Convergence instability is not unique to neural networks.** Training a
  deep network with a bad learning rate can fail to converge in exactly
  the same qualitative way this project's own GP fits failed for
  Function_05: this week's notebooks show its L-BFGS-B optimizer producing
  genuine `ABNORMAL` terminations on both the RBF and Matérn kernel
  (5 of 6 and 3 of 6 restart attempts respectively) — the underlying
  hyperparameter-optimisation landscape is difficult, the same failure
  mode that motivates using multiple random restarts (or early-stopping
  checks, or learning-rate schedules) in NN training. The
  convergence-aware selection rule this project already applies (prefer
  the fit with fewer failed optimizer attempts, and say plainly when
  neither fit is reliable) is the same discipline that would matter for
  choosing between multiple NN training runs — trusting a result because
  it has the best loss value alone, without checking whether the
  optimizer actually converged cleanly to get there, is a mistake in
  either setting.
- **Multiple restarts matter for the same reason in both settings.** This
  project's `n_restarts_optimizer=5` policy for GP fitting is directly
  analogous to running NN training from several random initialisations
  and comparing outcomes rather than trusting a single run — both exist
  because the underlying optimisation is non-convex and a single attempt
  can land in a poor solution purely by chance.
- **Bayesian optimisation itself is a standard technique for tuning NN
  hyperparameters** — this is worth noting explicitly as the most direct
  connection between the two topics: the exact GP-surrogate-plus-
  acquisition-function approach this project is building toward (random
  search → GP surrogate → convergence-aware model comparison → eventually
  an acquisition function) is one of the most common real-world methods
  for tuning a neural network's own learning rate, depth, or
  regularisation strength when training each candidate configuration is
  itself expensive. In that sense, this project's black-box functions
  could just as easily represent "validation accuracy as a function of
  hyperparameters" for some neural network, rather than the manufacturing-
  or design-style problems the actual eight functions represent — the
  methodology transfers directly.
- **Small-sample caution applies to both.** Just as this project has
  repeatedly flagged that its GP fits (11–43 observations across 2–8
  dimensions) are too small to support strong claims about which input
  dimensions are irrelevant, the same caution applies to reading too much
  into NN hyperparameter effects observed from only a handful of training
  runs — a hyperparameter that looks unimportant with 5 trials might
  simply not have been explored broadly enough yet, exactly the ambiguity
  this project has already flagged for its own length-scale-pinned-at-
  bound dimensions.

## What This Does Not Change

This reflection does not imply a neural network will be introduced into
this project — the eight functions are being modelled with Gaussian
Process surrogates, and that remains the approach. The value of this
comparison is conceptual: the same underlying ideas (capacity vs.
overfitting, optimisation convergence and instability, the value of
multiple restarts, and the appropriateness of Bayesian optimisation itself
as a hyperparameter-tuning method) show up whether the model being fit is
a GP or a neural network, and reasoning explicitly about one sharpens the
reasoning about the other.

---

## Follow-Up: Could a Neural Network Be a More Expressive Surrogate, and Could Backpropagation Steer Queries?

This follow-up activity asks two direct questions. Answering them honestly
means giving a real recommendation and its trade-off, not just listing
possibilities — and then translating that into a concrete next step for
this project, grounded in what's actually been observed so far.

### Could a neural network act as a more expressive surrogate in high-dimensional search spaces?

**In principle, yes — but not yet, for this project's actual data.** A
neural network surrogate (or a deep kernel / Bayesian neural network) can
represent non-stationary, sharply-varying response surfaces that a
stationary RBF/Matérn kernel genuinely struggles with, and unlike a GP
with one ARD length-scale per input dimension, its capacity doesn't scale
in quite the same rigid way as dimensionality grows. That expressiveness
is exactly what would help most for the higher-dimensional functions —
Function_07 (d=6, 33 observations) and Function_08 (d=8, 43 observations)
— where a GP's per-dimension length-scales are already showing the
"pinned at the search bound" pattern flagged repeatedly in this project's
own reports (Function_07's dimensions 1–3, Function_08's dimension 7),
which is at least partly a symptom of not having enough data to resolve
every direction confidently.

The trade-off that makes this **not yet appropriate** is exactly what
makes it tempting: neural networks are typically far more data-hungry than
GPs, and this project's per-function sample sizes (11–43 observations) are
already stretching what a *GP* can reliably fit — Function_05's genuine
optimizer non-convergence this week (both RBF and Matérn showing ABNORMAL
L-BFGS-B terminations) is direct evidence that even the current, lower-
capacity model is at the edge of what this little data can support. A
plain neural network with more parameters than a GP has hyperparameters,
fit to 43 points in 8 dimensions, would almost certainly overfit far more
severely, not less. There's also a structural gap, not just a data one: a
point-estimate neural network doesn't give the calibrated posterior
uncertainty that Expected Improvement or UCB depend on — getting that back
would require a Bayesian neural network, deep ensemble, or MC-dropout,
which is real added complexity, not a drop-in replacement.

**Recommendation**: keep the GP surrogate for now; revisit an NN-based
surrogate specifically for the higher-dimensional functions once their
per-function sample sizes grow substantially (realistically, many tens to
low hundreds of observations) — and even then, prefer a
uncertainty-aware variant (Bayesian NN / deep ensemble) over a plain
point-estimate network, so the acquisition-function machinery this
project is building toward still has something principled to reason
about.

### Could backpropagation help estimate gradients to steer queries?

**Backpropagation itself isn't the missing piece here — but gradient-based
query steering is, and it's available right now without introducing a
neural network at all.** Backpropagation is specifically how gradients are
computed *through a neural network's layers*; it has no direct meaning for
this project's black-box functions, which are not differentiable and
cannot be backpropagated through directly. The gradient that would
actually help — the gradient of the *surrogate's* predicted output (or an
acquisition function built on it) with respect to the input — is already
available in closed form for a GP with a standard RBF or Matérn kernel,
with no automatic differentiation or neural network required. This
project's notebooks so far only visualise the GP posterior via grids (2D
functions) and 1D slices (all functions) — they don't yet use the
posterior's analytic gradient to actually steer a query.

The genuine trade-off here is the same one that motivates why acquisition
functions balance mean and uncertainty rather than just climbing the
surrogate's predicted mean: gradient ascent purely on the posterior mean
would happily walk toward a region the model merely *believes* is good
based on very little data, exactly the kind of premature exploitation this
project's own reflections have repeatedly cautioned against given how
large the posterior uncertainty still is almost everywhere at this sample
size. Any gradient-based step needs to be on the *acquisition function*
(which already incorporates uncertainty), not the raw predicted mean.
There's also a curse-of-dimensionality argument specifically in favour of
doing this soon: a dense grid search over 6–8 dimensions (Function_07,
Function_08) becomes impractical quickly, which is exactly why real
Bayesian optimisation libraries use multi-start gradient-based
optimisation (e.g. L-BFGS from several random starting points) to maximise
the acquisition function in higher dimensions, rather than evaluating it
on a grid.

**Recommendation**: once an acquisition function is introduced (the
already-planned next step), optimise it via multi-start gradient-based
local search using the GP's analytic posterior gradients — not by
introducing backpropagation or a neural network. Apply the same
convergence-aware discipline already used for GP kernel fitting (multiple
restarts, checking whether the optimizer actually converged) to this
acquisition-optimisation step too, rather than trusting a single run's
result.

## A Stronger Strategy for the Next Submission

Bringing this together into concrete next steps, grounded in what this
project has actually built and found so far:

1. **Keep the Gaussian Process surrogate** for all eight functions at
   current sample sizes — a neural network surrogate is a documented,
   deferred option, not a near-term change, pending substantially more
   observations per function.
2. **Introduce acquisition-function-driven querying** (the step every
   prior week's reflection has flagged as deferred) using Expected
   Improvement or UCB computed on the existing GP posterior — this is the
   natural next concrete step, not gradient methods or NNs.
3. **Optimise that acquisition function via multi-start gradient-based
   local search** (using the GP's analytic posterior gradients, no
   backpropagation needed) rather than a grid — necessary in practice for
   Function_07 and Function_08, where a dense grid is infeasible, and
   consistent with standard practice in real Bayesian optimisation
   libraries.
4. **Apply the same convergence-aware checking already used for kernel
   fitting to this new optimisation step** — record how many restarts
   were needed and whether they agree, rather than trusting a single
   gradient-ascent run, especially for Function_05 given its already-
   documented instability.
5. **Revisit neural-network surrogates later, not now** — specifically
   once per-function sample sizes grow enough to support them, and only
   with an uncertainty-aware variant (Bayesian NN or deep ensemble), so
   the exploration/exploitation trade-off this project already relies on
   doesn't lose its principled uncertainty estimate.

---

## Part 2: Critical Reflection on Strategy

These prompts ask about a neural network surrogate directly. As stated
above, **no neural network has been trained anywhere in this project** —
every model built in `Week_01/` and `Historical_Replay/` is a Gaussian
Process. The answers below are honest about that gap throughout: where a
question assumes an NN result that doesn't exist, the answer says so
plainly and gives the closest genuine analogue from this project's actual
GP work, rather than describing an experiment that was never run.

**1. Which inputs seemed to act like support vectors — near a decision
boundary or region of rapid change?**

Two concrete candidates stand out from the genuine historical data
imported so far, not from any classifier:

- **Function_05's all-zero query, `[0,0,0,0]` → `163.1225`** (the Week 4
  pair), sits at a corner of the input domain and returns a value on a
  completely different scale from this function's other observations —
  the kind of sharp jump that would sit right at a decision boundary if
  Function_05's outputs were thresholded into "good" vs "bad."
- **Function_02 and Function_06's near-zero outputs** encountered in the
  Week 3 import (`6e-06` and `7e-06` respectively) sit extremely close to
  a sign change in output — genuinely support-vector-like in the sense
  that a tiny perturbation in the query could plausibly flip which side
  of zero the output lands on.

Recognising these should guide the next query toward **finer sampling
around these specific points**, not away from them — if the goal is to
map where the function's behaviour changes character, these are exactly
the regions where one more nearby query is most informative, the same
logic that makes points near an SVM's margin the most informative points
to a classifier.

**2. Did you train a neural network or surrogate model — did you explore
how outputs change with inputs, and how gradients might reduce the
function value? If not, why not?**

A surrogate model **was** trained — a Gaussian Process, not a neural
network — and its predicted mean is exactly as differentiable with
respect to the input as a trained NN's output would be. That gradient
has **not yet been computed or used** in this project: every notebook so
far only visualises the posterior via 2D grids or 1D slices, not its
analytic gradient. A neural network specifically was not trained for the
reason detailed above: at 11–43 observations per function, an NN would be
far more prone to overfitting than the GP already fit, and it would not
natively provide the calibrated uncertainty that this project's planned
acquisition-function step depends on.

**3. Framing this as a classification task (good vs. bad outputs) — how
could logistic regression, SVMs, or neural networks capture the decision
boundary, and what trade-offs exist between misclassification and
exploration?**

Any of the three could, in principle, fit a boundary separating queries
whose output is "good" (low, since these functions are being minimised)
from "bad" (high) — logistic regression giving a linear boundary in the
input space (or in a hand-engineered feature space), an SVM giving a
maximum-margin boundary usable with a kernel for non-linear separation,
and a neural network giving an arbitrarily flexible boundary. The core
trade-off is that **collapsing a continuous output into a binary label
throws away exactly the magnitude information Bayesian optimisation
needs** — knowing a point is "bad" says nothing about *how* bad, which
matters for deciding whether it's worth exploring nearby. There's also an
exploration cost specific to this project's tiny sample sizes: with only
11–43 labelled points, any of these three classifiers would have a highly
uncertain boundary, and treating that boundary as reliable enough to
prune future queries risks discarding regions that are only mislabelled
"bad" for lack of data — the same small-sample caution raised throughout
this project's GP work.

**4. Which model — linear regression, SVM, or neural network — felt most
appropriate for guiding the search, and how was interpretability balanced
against flexibility?**

None of these three is what this project actually used — the Gaussian
Process sits deliberately between them. A GP with an RBF/Matérn kernel is,
mechanically, Bayesian **linear regression carried out in an
infinite-dimensional feature space defined by the kernel** — so it keeps
a linear model's interpretability (a small number of hyperparameters:
signal variance, per-dimension length-scales, noise level, each with a
direct physical reading) while gaining an SVM- or NN-like ability to
represent non-linear structure. If forced to choose only among the three
listed, an **SVM (as a regressor, i.e. SVR)** would be the closest
compromise: kernelised like the GP, but without the native predictive
uncertainty that made the GP the right choice for an *optimisation* task
specifically, as opposed to a one-off prediction task.

**5. Which input variables showed the steepest gradients / greatest
influence in "your neural network surrogate," and how could this
prioritise future queries?**

No neural network was trained, so there are no NN gradients to report.
The closest genuine signal from this project's actual GP fits is each
kernel's **per-dimension length-scale**: a shorter length-scale means the
predicted output changes faster along that dimension for a given input
step, i.e. greater local sensitivity, while a length-scale pinned at the
upper search bound (already flagged for Function_07's dimensions 1–3 and
Function_08's dimension 7) indicates the fit could not resolve any
sensitivity in that dimension from the data available. The exact fitted
length-scale values are recorded per function in each week's GP
diagnostics notebook rather than restated here to avoid transcribing
numbers from memory; prioritising future queries should favour dimensions
with short, well-converged length-scales (genuine sensitivity worth
refining) over those pinned at the bound (not yet resolvable with this
much data).

**6. Framing this as classification — how well did "your neural network"
approximate the decision boundary, and how did backpropagation help
interpret or visualise it?**

Since no neural network or classifier of any kind was trained in this
project, there is no fitted decision boundary and no backpropagation-based
interpretation to report. Had this been attempted, backpropagation would
only have entered as the mechanism for computing gradients *of that
classifier's own loss with respect to its weights* during training — it
would not, by itself, have made the resulting boundary any easier to
visualise; visualising a learned boundary in more than 2–3 dimensions
still requires the same grid/slice techniques already used for the GP
posterior in this project's notebooks.

**7. Compared to linear/logistic regression, how well did "your neural
network" capture non-linear patterns, and was the added flexibility worth
the tuning/interpretation cost?**

Again, no neural network was trained, so this project has no NN result to
compare. The genuine comparison available is the GP itself against a
plain linear model: the RBF/Matérn kernel lets the GP capture non-linear
structure a linear regression structurally cannot, and the added tuning
cost was real and directly observed — Function_05's kernel
hyperparameter fitting produced genuine `ABNORMAL` optimizer terminations
in both Week 3 and Week 4, and kernel choice for Function_05 and
Function_07 flipped between weeks as more data arrived. That cost was
judged worth paying because the flexibility bought two things a linear
model can't offer: a plausible fit to genuinely non-linear black-box
functions, and calibrated uncertainty essential to the acquisition-driven
querying this project is building toward — the same justification that
would apply to choosing a neural network over linear regression, but with
the additional benefit, specific to the GP, of not needing a separate
mechanism bolted on afterward to recover uncertainty.

---

## Follow-Up: Hyperparameter Effects and Application to the Capstone

**Honesty note, consistent with every section above**: no neural network
has been trained in this project, so nothing below reports an actual NN
training outcome. Where the prompts ask what was "observed," the answer
draws on this project's real, observed GP hyperparameter behaviour — the
closest genuine analogue available — and says so explicitly.

### 1. Key hyperparameters observed, and their effect on convergence, stability, or performance

No NN hyperparameter has been tuned in this project. The hyperparameters
actually fit and observed are this project's **GP kernel hyperparameters**
— signal variance, per-dimension length-scale(s), and noise level — plus
one **procedural** hyperparameter, `n_restarts_optimizer=5`, and one
**model-family choice**, RBF vs. Matérn(ν=2.5). Their effects on
convergence and stability were concretely observed, not hypothetical:

- **Kernel family choice measurably changed convergence reliability.**
  For Function_05, the RBF and Matérn fits showed genuinely different
  numbers of `ABNORMAL` L-BFGS-B terminations out of 6 restart attempts
  across weeks (Week 2: RBF 1/6; Week 3: RBF 3/6, Matérn 5/6; Week 4:
  RBF 5/6, Matérn 3/6) — the *same* underlying data-fitting problem became
  more or less numerically stable purely as a function of which kernel
  family was used, and which kernel was "more stable" even flipped
  between weeks as more data arrived.
- **Restart count directly affected whether an unstable fit was caught at
  all.** With only 1 optimizer attempt (no restarts), Function_05's
  instability could easily have gone unnoticed; instrumenting all 6
  restarts is what surfaced the ABNORMAL-termination pattern in the first
  place, the same reason multiple random initialisations matter for NN
  training.
- **Length-scale fitting outcomes affected interpretability, not just
  fit quality.** Several dimensions (Function_07's dimensions 1–3,
  Function_08's dimension 7) had their length-scale fit pinned at the
  upper search bound rather than converging to an interior value — a
  direct, observed consequence of too little data to resolve sensitivity
  in those directions, not a hyperparameter setting that was chosen.

### 2. Discrete vs. continuous hyperparameters, and how that shapes tuning method

Categorising the hyperparameters actually involved in this project's own
modelling (plus the general NN hyperparameters named in the source
reflection above):

| Hyperparameter | Type | Tuning method used / appropriate |
|---|---|---|
| Kernel signal variance | Continuous | Continuous optimisation (L-BFGS-B, maximising log-marginal-likelihood) |
| Kernel length-scale(s) | Continuous | Same — continuous optimisation |
| Noise level (`WhiteKernel`) | Continuous | Same — continuous optimisation |
| Kernel family (RBF vs. Matérn) | **Discrete / categorical** | Not amenable to gradient-based tuning at all — handled by fitting each candidate separately and comparing via the convergence-aware selection rule (fewer ABNORMAL terminations, then higher log-marginal-likelihood) |
| `n_restarts_optimizer` | Discrete | Set by convention/judgement (5), not tuned by search |
| NN learning rate (general) | Continuous | Grid/random search, or itself treated as a black-box variable for Bayesian optimisation |
| NN depth / width (general) | Discrete | Grid/random search, or a categorical dimension in Bayesian optimisation |
| NN batch size / epochs (general) | Discrete | Grid/random search |
| NN weight initialisation seed (general) | Discrete | Fixed and recorded for reproducibility, not usually "tuned" for performance |

The type of hyperparameter genuinely changes what tuning method is
appropriate — this project's own kernel-family choice is the clearest
example: it is a categorical decision with no gradient to follow, so it
was resolved the same way a categorical NN hyperparameter (e.g. choice of
activation function or optimiser) would be — fit multiple candidates
independently and compare them on a fixed, principled criterion — rather
than by any continuous optimisation method. This is exactly why standard
Bayesian-optimisation-for-hyperparameter-tuning setups mix search
strategies: continuous kernels/parameters via gradient-based or
GP-acquisition search, discrete/categorical choices via grid search,
random search, or specialised categorical kernels within the same
Bayesian-optimisation framework.

### 3. Application to the capstone: would understanding hyperparameter tuning change NN-surrogate decisions, and could this project's BO approach tune an NN directly?

If a neural network surrogate were adopted later (as this reflection's
earlier section already frames as a **deferred**, not current, option),
the discrete/continuous distinction above would directly shape how its
hyperparameters get tuned: continuous ones (learning rate, weight decay)
would be natural candidates for the same GP-surrogate-plus-acquisition
machinery this project is already building for the eight black-box
functions, while discrete/categorical ones (depth, width, architecture
family) would need the categorical-comparison approach already used here
for kernel-family selection — fit multiple candidates, compare on a fixed
criterion, rather than assume a gradient exists.

More directly: **yes, this project's own Bayesian optimisation approach
could be applied to tune a neural network's hyperparameters**, and this is
not speculative — it is one of the most common real-world uses of exactly
this GP-surrogate-plus-acquisition-function methodology, as already noted
above. Concretely, this would mean treating a neural network's validation
performance as the black-box output, its hyperparameters (learning rate,
depth, regularisation strength) as the query coordinates, and reusing the
same convergence-aware discipline already established here — instrument
every optimiser restart, prefer fewer failed terminations, and don't trust
a single run's result — when fitting the GP surrogate over the NN's own
hyperparameter space. That reuse is available immediately, without waiting
for the "more data" precondition already set for adopting an NN as *this
project's* black-box-function surrogate — because in that scenario, the
GP would be surrogate-modelling the neural network's hyperparameters, not
replaced by the neural network itself.
