# Week 5 Strategy Reflection — Advanced Neural Network Concepts and This Project

**Scope:** Reflection for Required Capstone Component 16.1.
**Honesty note on 16.1, consistent with every prior round's reflection**:
this component assumes a fifth real query has been submitted, with a
dataset of "14 data points." In this project, only **one** real query has
ever been generated for live submission — the Week 1 random-search
baseline (`Week_01/`). No second, third, fourth, or fifth live query
exists. What has actually progressed through five rounds is the
`Historical_Replay/` track, built on genuine historical data, explicitly
kept separate from live submissions (see every prior week's reflection for
the same caveat). Note also that "14 data points" does not describe this
project's actual state uniformly: `Historical_Replay/Week_05`'s per-function
cumulative row counts are 14, 14, 19, 34, 24, 24, 34, 44 for Functions
01–08 respectively — 14 matches only Functions 01 and 02, since each
function has its own independent starting dataset size and this project
never pads or truncates a function's real observation count to match
another's (`CLAUDE.md` Rule 2).

## This Project Has Never Used AlexNet, ImageNet-Style Training, or a Deep Learning Framework

Worth stating plainly before reflecting: this project has never used
AlexNet, any ImageNet-pretrained model, PyTorch, TensorFlow, or any deep
neural network. Every model built in `Week_01/` and `Historical_Replay/`
is a Gaussian Process, fit with `scikit-learn`. This reflection connects
general knowledge of these advanced deep-learning topics to the modelling
work actually done here, rather than reporting on experiments that don't
exist in this project.

## AlexNet and ImageNet — Why the Breakthrough Doesn't Transfer Directly Here

AlexNet's 2012 ImageNet result mattered because it showed a
sufficiently deep convolutional network, trained on a sufficiently large
labelled dataset with sufficient compute (GPUs), could learn useful
hierarchical feature representations **directly from raw pixels** —
replacing hand-engineered features with representations learned from data.

That breakthrough depended on conditions this project's eight black-box
functions do not have: ImageNet has roughly 1.2 million labelled training
images; this project's largest function-specific dataset
(`Historical_Replay/Week_05/Function_08`) has **44** observations. The
entire premise of a deep, many-layer network learning its own feature
hierarchy requires a data volume many orders of magnitude larger than
anything available here. This is the same small-sample caution already
raised in every prior week's reflection, now sharpened by a concrete
point of comparison: AlexNet's success is itself evidence that deep
learning's advantages emerge at large scale, and is not, on its own, a
reason to expect the same advantage at n=11–44.

## The Five Building Blocks of Deep Learning, Mapped Onto This Project's Actual GP Work

The five building blocks typically taught for a deep learning system —
data, model architecture, a loss/objective function, an optimisation
procedure, and an evaluation step — all have a direct, already-implemented
analogue in this project's GP pipeline, even though no neural network is
used:

| Deep learning building block | This project's actual GP analogue |
|---|---|
| **Data** | Each function's own cumulative `.npy` dataset, validated for shape, bounds, duplicates, and finiteness before every fit (`CLAUDE.md` Rules 1–4). |
| **Model architecture** | The kernel choice (RBF or Matérn(ν=2.5), each combined with `ConstantKernel` and `WhiteKernel`) plays the same structural role as choosing a network's layers — it defines the space of functions the model can represent. |
| **Loss/objective function** | Negative log-marginal-likelihood, maximised during kernel-hyperparameter fitting — functionally the same role as a neural network's loss function during training. |
| **Optimisation procedure** | `L-BFGS-B` with `n_restarts_optimizer=5` (6 total attempts) plays the same role as gradient descent/backpropagation in NN training — both are non-convex optimisation procedures where a single run can land in a poor solution, which is exactly why this project instruments every restart attempt. |
| **Evaluation** | The diagnostic plots (observed-vs-predicted, residuals) and, distinctively, this project's own convergence-aware kernel-selection rule (prefer fewer `ABNORMAL` terminations, then higher log-marginal-likelihood, else report both as exploratory) — a documented, disciplined evaluation step already applied identically to every function since Week 3. |

Seeing these five blocks mapped this way makes the comparison concrete
rather than abstract: this project already runs a version of the standard
deep-learning workflow, just with a different, more data-efficient model
family substituted at the "architecture" step.

## PyTorch/TensorFlow — Why Neither Has Been Adopted

Neither PyTorch nor TensorFlow has been used, and the reasoning is
directly about fit for purpose, not unfamiliarity with the tools:
`scikit-learn`'s `GaussianProcessRegressor` already provides everything
this project's actual model family needs (kernel construction,
multi-restart marginal-likelihood optimisation, posterior mean/variance)
without the added complexity of a full deep-learning framework — automatic
differentiation, GPU dispatch, and manual training-loop construction that
PyTorch/TensorFlow provide are built for training many-parameter networks
on large datasets, neither of which describes this project's current
scale. Introducing either framework now would add real engineering
overhead (environment/dependency management, a training loop, device
handling) without a corresponding model that could actually use it
responsibly at n=11–44 observations per function.

If an uncertainty-aware neural surrogate is ever adopted later — as
`Week_04/STRATEGY_REFLECTION.md` already frames as a **deferred**, not
current, option, pending substantially larger per-function sample sizes —
PyTorch would be the natural framework to introduce at that point
specifically because it would be needed for a Bayesian neural network or
deep ensemble surrogate, not for its own sake.

## How This Strengthens the Project's Strategy Going Forward

None of this changes the immediate next step already identified in
`Week_04/STRATEGY_REFLECTION.md`: introduce an acquisition function (EI or
UCB) on the existing GP posterior, optimised via multi-start gradient-based
search using the GP's own analytic posterior gradients. What this week's
reflection adds is a clearer articulation of *why* the GP remains the
right choice for now — the five-building-blocks comparison shows this
project already runs a disciplined, deep-learning-style workflow at every
stage (data validation, model/architecture choice, an objective function,
a multi-restart optimisation procedure, and a documented evaluation rule),
and the AlexNet/ImageNet comparison sharpens exactly how large a sample
size would need to grow before a deep architecture's advantages would
plausibly outweigh a GP's much greater data efficiency at this project's
current scale.

---

## Follow-Up: Feature Hierarchies, Architectural Trade-offs, and Framework Choices as Parallels to Iterative Strategy Design

This follow-up asks specifically how three deep-learning ideas —
hierarchical features, architectural trade-offs, and framework choice —
parallel the iterative, week-by-week strategy design already under way in
this project, and how that parallel should inform where to query next.

### Feature hierarchies ↔ coarse-to-fine information gain across weeks

A deep network's early layers learn coarse, low-level features (edges,
simple textures) and later layers compose these into progressively more
abstract representations — the hierarchy only becomes informative once
enough data has passed through it. This project's own week-by-week process
shows the same coarse-to-fine progression, just across time rather than
across layers: early weeks' sparse per-function datasets (11 observations
at Week 1) could only support coarse judgments — does any structure exist
at all, is the output roughly smooth or highly variable (the EDA scatter
and output-histogram steps every week). By Week 5, larger datasets
(up to 44 observations for Function_08) support a genuinely finer-grained
read: per-dimension length-scales that distinguish which input directions
the model can resolve confidently versus which remain pinned at the
search bound (Function_07's dimensions 1–3, Function_08's dimension 7).
That pinned/resolved distinction is this project's closest analogue to a
feature hierarchy revealing structure only once there's enough data to
support it.

The honest limit of this parallel matters too: a GP with an additive,
per-dimension length-scale kernel does not compose features
hierarchically the way a deep network's layers do — each input dimension
contributes its own independent length-scale rather than the model
learning higher-order combinations of dimensions on its own. This is a
genuine architectural limitation relative to a deep network, not just a
data-scale one, and it is one more reason (alongside the data-volume
argument in the section above) that a deep architecture's capacity
advantage is currently more theoretical than practical for these eight
functions.

### Architectural trade-offs ↔ this project's own kernel/capacity decisions

A deep network's depth/width choice trades representational capacity
against overfitting risk and training stability — the same trade-off this
project already navigates explicitly every week when choosing between RBF
and Matérn kernels (a smoothness-assumption trade-off) and when deciding
how many optimizer restarts to run (a search-thoroughness-versus-cost
trade-off). Function_05's persistent convergence instability (unresolved
across Weeks 2–5, 4/6 ABNORMAL for both kernels this week) is this
project's concrete evidence of what happens when a model-capacity choice
sits at the edge of what the available data can support — structurally
the same failure mode as a network architecture that is too complex (or
poorly conditioned) for its training set.

The parallel to "where to query next" is direct: an architectural
capacity decision in deep learning is a decision about how much of the
model's representational budget to spend where; an acquisition function
in Bayesian optimisation is the same kind of budget decision applied to
where to spend the *next observation*. Prioritising queries in
under-resolved dimensions (where length-scales are pinned at the bound)
is this project's analogue of directing a model's limited capacity toward
the regions of input space it currently understands least.

### Framework choice ↔ tooling matched to the actual model in use

PyTorch and TensorFlow exist to make training many-parameter networks
practical — automatic differentiation, GPU dispatch, mini-batching. The
same reasoning that motivates choosing a framework based on what the
model actually needs is why this project uses `scikit-learn` rather than
either: this project's GP posterior mean is already analytically
differentiable in closed form (no automatic differentiation required),
and its optimisation is a small, CPU-tractable multi-restart search, not
mini-batch gradient descent over a large parameter set. Framework choice,
in both settings, should follow from the model's actual computational
needs rather than from which tool is more prominent in the field — the
same principle articulated for the model-family choice itself earlier in
this reflection.

### Synthesis: what this means for the next query

Bringing these three parallels together sharpens, rather than changes,
the strategy already set: introduce an acquisition function next,
optimised via the GP's own analytic gradients (no new framework needed),
and let the current per-dimension length-scale picture — which dimensions
are resolved versus pinned at the bound, the closest thing this project
has to a "feature hierarchy" reading of its own data — directly inform
where that acquisition function should weight exploration most heavily,
particularly for the higher-dimensional functions (Function_07,
Function_08) where under-resolved dimensions have already been identified.

---

## Part 2: Critical Reflection on Strategy

**1. How did hierarchical feature learning influence structuring or
refining the optimisation strategy this round?**

It reframed how this week's per-dimension length-scale results were read.
Rather than treating "pinned at the search bound" as simply a fitting
detail to note, thinking in terms of a feature hierarchy suggested
reading it as a *level of understanding not yet reached* — the same way
an under-trained layer in a network hasn't yet resolved a useful feature.
That reframing is what motivated this week's synthesis (above): use the
resolved-versus-pinned distinction to directly weight where a future
acquisition function should explore, rather than treating every dimension
as equally well understood. No change was made to the actual GP fitting
procedure this round — the influence was on interpretation and forward
planning, not on this week's modelling code itself.

**2. What parallels do you see between AlexNet/ImageNet-style leaps and
this project's own incremental submissions?**

The honest parallel is more about contrast than similarity. AlexNet's
leap came from a genuine change in *method* — a deeper convolutional
architecture, trained at a data/compute scale not previously attempted —
producing a discontinuous jump in performance, not a gradual improvement.
This project has had one moment that resembles that structurally, if not
in scale: the Week 2 remediation that introduced the convergence-aware
kernel-selection rule (prefer fewer ABNORMAL terminations, then higher
log-marginal-likelihood, else report both as exploratory) was a genuine
methodological step-change, not an incremental tweak — before it, a
misattributed convergence finding went uncorrected; after it, every
week's diagnostics apply a disciplined, honest selection rule. That is
this project's closest analogue to a "breakthrough," and like AlexNet's,
it came from changing the *approach*, not just adding more data.

Where the parallel breaks down is instructive: AlexNet's story is usually
told as a clean, dramatic improvement. This project's actual week-to-week
results are not monotonic — Function_05 this week improved for RBF (5/6
→ 4/6 ABNORMAL) but simultaneously worsened for Matérn (3/6 → 4/6),
a genuinely mixed result. Real iterative work, even at this small scale,
is messier than the highlight-reel version of a breakthrough, and this
project's reflections have tried to represent that honestly rather than
narrating steady, uninterrupted progress.

**3. Did you encounter depth/complexity/training-efficiency-style
trade-offs in deciding whether to explore widely or exploit known
promising regions?**

Not yet at the level the question is really asking about: this project
has not run an acquisition function, so no query has actually been chosen
by weighing explore-versus-exploit — every query so far is either the
Week 1 random baseline or a genuine historical replay, not a query this
project's own strategy selected. The structurally similar trade-off that
*has* been made repeatedly is at the model-fitting level: choosing
`n_restarts_optimizer=5` is a direct trade-off between search
thoroughness (more restarts, better chance of finding the true
log-marginal-likelihood optimum) and computational cost, the same shape
of trade-off as depth/width decisions in a network trading capacity
against training cost. It is the right honest analogue available in this
project's actual work, but it should not be overstated as the
explore/exploit query-selection trade-off itself, which remains a future
step.

**4. Which building blocks (inputs, activations, loss, gradients, weight
updates) helped you think differently about how the model learns from
the accumulated data?**

**Loss and gradients** mapped most usefully. Treating log-marginal-
likelihood as a loss surface, and L-BFGS-B's restart search as gradient-
based descent over it, reframed Function_05's persistent `ABNORMAL`
terminations: not as "the wrong kernel was chosen," but as "the loss
landscape this particular data induces is genuinely hard to descend
reliably" — a more technically precise, less blame-the-model way to
describe the same observed instability. **Inputs** map straightforwardly
(the cumulative query vectors). **Activations** and **weight updates** do
not map cleanly onto a GP at all, and it would be misleading to force the
analogy: a GP has no layered forward pass and no per-example incremental
weight update — its kernel hyperparameters are fit once per call via a
batch quasi-Newton procedure, not updated step-by-step across many small
gradient steps the way a network's weights are. Being explicit about
where the analogy stops is as useful as the parts where it holds.

**5. If you framed your current approach as a "framework," would it be
closer to rapid prototyping/flexibility or structured, production-ready
design?**

Closer to **structured**, but not "production-ready" in the conventional
software-engineering sense either — it sits between the two, and if
forced to pick one, structured is the better fit. Every step in this
project (see `SOFTWARE_ARCHITECTURE.md`, Section 5) goes through a fixed
propose → independently verify → build → independently re-verify →
document loop, with recorded seeds, checksums recomputed (not just
re-read) at every stage, and dated permanent review artifacts — that is
real engineering discipline, well past "rapid prototyping." What keeps it
short of "production-ready" is the absence of automation: there is no
CI/CD pipeline, no automated test suite, and each week's verification is
performed by dispatching review steps manually rather than by a system
that runs unattended. It is best described as a rigorously audited
research pipeline — structured and disciplined in process, but not yet
engineered for unattended production use.

**6. How might reflecting on real-world deep learning use cases (e.g.
industry applications in sport, as discussed in the guest interview)
inform how you benchmark success in your own capstone challenge?**

Without claiming familiarity with specifics from that interview beyond
what this prompt describes, the generally-applicable principle from
real-world sport-analytics deep learning is that success is rarely
benchmarked by a single raw performance number in isolation — it is
benchmarked by whether a model's output is reliable enough, and its
uncertainty honest enough, to actually change a decision a coach or team
would make. That principle transfers directly to how this project should
benchmark its own success: not simply "did this week find a lower/higher
output value," but whether the *model itself* can be trusted at the
current sample size — which is exactly why this project's own
convergence-aware selection rule refuses to declare a kernel reliable
when the evidence doesn't support it (Function_05, honestly reported as
exploratory-only rather than force-selecting a "winner"). A model
selected or trusted purely because it produced the most impressive-looking
number, without checking whether it converged reliably, would be the
capstone-project equivalent of an unvalidated sport-analytics model that
looks good on paper but wouldn't survive contact with a real decision.
