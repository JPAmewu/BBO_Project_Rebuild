# Software Architecture — BBO_Project_Rebuild

**Scope:** Required Capstone Component 16.2 — documents the software
architecture actually in use for this GitHub repository
(`https://github.com/JPAmewu/BBO_Project_Rebuild`), and the reasoning
behind it. This describes the architecture as it exists today; it is not
a future-scope design document, and it is updated as the project's actual
structure changes rather than describing an aspirational state.

## 1. Repository-Level Architecture: Two Deliberately Separated Tracks

The repository root contains exactly two top-level work areas, plus
governance/documentation files:

```
BBO_Project_Rebuild/
├── CLAUDE.md                        # permanent project rules (governs all work)
├── README.md, DATASHEET.md,
│   MODEL_CARD.md                    # project-level documentation
├── HISTORICAL_REPLAY_INVENTORY.md,
│   HISTORICAL_REPLAY_PROTOCOL.md    # governance for the retrospective track
├── SOFTWARE_ARCHITECTURE.md         # this file
├── Week_01/                         # the real, live-submission track
│   └── Function_01/ .. Function_08/
└── Historical_Replay/               # retrospective learning track
    ├── Week_01/ .. Week_05/
    │   └── Function_01/ .. Function_08/
```

This two-track split is the single most important architectural decision
in the repository, and it is deliberate rather than incidental:

- **`Week_01/`** holds the one real, live-submitted query this project has
  ever generated (a seeded random-search baseline). It exists to preserve
  that submission exactly as it was made.
- **`Historical_Replay/`** is a separate, explicitly-labelled retrospective
  track that replays genuine historical query-output pairs from an
  original source project, week by week, to practise and validate the
  full Gaussian Process modelling pipeline without ever fabricating a
  query or conflating retrospective work with a live submission.

Keeping these as physically separate directory trees — rather than, say,
a single `Week_0X/` sequence with a flag distinguishing "real" from
"replayed" — makes the distinction structurally unambiguous: it is
impossible to accidentally treat a `Historical_Replay/` artifact as a live
submission, or vice versa, without an explicit path change. This directly
supports `CLAUDE.md` Rule 12 (never commit/push without instruction) and
the project's broader principle of never fabricating or misrepresenting
what has actually been submitted.

## 2. Per-Function, Per-Week Module Structure

Both tracks share the same internal module boundary: **function** and
**week** are the two axes of decomposition, and every function's data,
model, and documentation are kept fully isolated from every other
function's (`CLAUDE.md` Rule 2 — never mix functions' inputs/outputs).

Each `Function_0X/` folder (both tracks) follows the same standard
subfolder contract defined in `CLAUDE.md`:

| Subfolder | Purpose |
|---|---|
| `01_Notebook/` | The week's analysis/modelling notebook for this function only |
| `02_Data/` | That function's own cumulative validated dataset (`.npy`) |
| `03_Queries/` | Proposed/submitted query points (`Week_01/` only, so far) |
| `04_Results/` or `04_Figures/` | Observed outputs, or diagnostic plots, depending on track |
| `05_Documentation/` | Model assumptions, kernel/hyperparameter records |
| `06_Code_Review/` | Independent code-review reports |
| `07_Reflection/` | Weekly reflections |

This is a deliberately repetitive, uniform structure rather than a shared
"utils" module or common library: every function's pipeline is
self-contained within its own folder, so a change or defect confined to
one function's notebook cannot silently affect another's. Some
duplication across 8 near-identical notebooks per week is the accepted
cost of this isolation guarantee.

## 3. Language, Libraries, and Environment Pinning

- **Language**: Python (currently 3.14.3 across all weeks).
- **Core libraries**: `numpy`, `scipy`, `scikit-learn` (the actual modelling
  engine — `GaussianProcessRegressor` with RBF/Matérn kernels), `matplotlib`,
  `seaborn` for plotting, and `jupyter`/`nbformat`/`nbclient`/`nbconvert`/
  `ipykernel` to build and execute notebooks programmatically.
- **No deep-learning framework** (PyTorch, TensorFlow) is currently a
  dependency — see `Historical_Replay/Week_05/STRATEGY_REFLECTION.md` for
  the explicit reasoning: the project's actual model family (GP surrogates)
  is fully served by `scikit-learn`, and introducing a deep-learning
  framework now would add real complexity with no model in place that
  could use it responsibly at this project's current per-function sample
  sizes (11–44 observations).
- **Environment pinning**: each `Historical_Replay/Week_0X/requirements-lock.txt`
  records the exact package versions used to execute that week's
  notebooks, checked before reuse in later weeks (an unchanged environment
  is confirmed via direct `pip freeze` inspection before copying a lock
  file forward, rather than assumed).

## 4. Data Architecture

- **Storage format**: raw NumPy `.npy` arrays for cumulative inputs/outputs
  (not CSV/Parquet/a database) — chosen for exact round-trip fidelity with
  `scikit-learn`'s expected array inputs and for trivial, dependency-free
  loading in every notebook.
- **Cumulativeness**: each week's dataset is built by appending exactly one
  new validated row to the previous week's array (`CLAUDE.md` Rules 3, 20)
  — never regenerated from scratch, so a function's full observation
  history is always reconstructable from its weekly dataset lineage.
- **Provenance**: every appended row is traceable to a specific source file
  and SHA-256 checksum in the original source project, recorded in a
  per-function `pair_provenance.md` and a per-week `VERIFIED_PAIRS.csv`
  index.
- **Integrity verification**: SHA-256 checksums are computed and recorded
  at build time for every dataset file, and independently recomputed
  (never merely re-read) during every subsequent review step.

## 5. Verification Architecture — the Build/Verify Loop

Every substantive change to data or models in this repository goes
through the same fixed sequence, which is itself an architectural
decision (a fixed pipeline shape, not ad hoc review):

1. **Propose** (a specific, scoped operation — e.g. "append this one row").
2. **Independently verify the proposal** before it is built, using a
   read-only review agent with no write access.
3. **Build** the actual artifact (dataset, notebook) directly, rather than
   trusting a large-array transcription.
4. **Independently re-derive and re-verify the built artifact from
   scratch** (a second, differently-scoped read-only review), including
   recomputing checksums rather than re-reading recorded ones.
5. **Document** both the build and both review passes as permanent,
   dated markdown records alongside the artifact.

This loop is why the repository contains as many `*_VALIDATION.md`,
`INDEPENDENT_*_REVIEW.md`, and `code_review_report.md` files as it does
data/notebook files — verification artifacts are treated as first-class,
permanent parts of the architecture, not disposable process, because they
are what make every dataset and model in the repository independently
auditable after the fact.

## 6. Version Control Architecture

- A single git repository at the project root (`BBO_Project_Rebuild`),
  pushed to a single public GitHub remote
  (`https://github.com/JPAmewu/BBO_Project_Rebuild`) — not split into
  multiple repositories per track or per week, so the full project history
  and both tracks are visible in one place.
- The original source project (`My_Capstone_1_Imperial`) is a separate,
  independent git repository, never touched by this repository's own git
  operations — read-only access only, enforced procedurally (`CLAUDE.md`)
  rather than by any technical repository-linking mechanism.
- `.gitignore` excludes local notebook-checkpoint artifacts
  (`.ipynb_checkpoints/`), OS metadata (`.DS_Store`), Python bytecode
  caches, and one specifically-named stray file left by an earlier session
  — kept on disk but excluded from version control by explicit user
  instruction, rather than deleted.
- Commits are created only on explicit instruction (`CLAUDE.md` Rule 12),
  and are scoped to a specific, identifiable body of work (e.g. "Week_04
  complete") rather than broad, unscoped snapshots.

## 7. Why This Architecture, Not an Alternative

- **A shared library/package instead of per-function duplication** was
  considered and rejected for now: with only 8 functions and weekly,
  incremental changes, a shared abstraction would add indirection without
  a corresponding reduction in real complexity, and would risk a change
  intended for one function silently affecting another — directly against
  Rule 2. This may be revisited if the notebooks' shared logic grows
  substantially before the project's later weeks.
- **A database (e.g. SQLite) instead of per-function `.npy` files** was
  not adopted: the datasets are small (currently ≤44 rows per function),
  and `.npy` files integrate directly with `scikit-learn` with no
  serialisation layer, at the cost of losing query-language flexibility
  that isn't currently needed.
- **No CI/CD pipeline** currently runs notebook execution automatically;
  execution is performed and independently reviewed manually as part of
  the build/verify loop described above. This is a reasonable manual
  substitute at the project's current scale, but would be a natural
  candidate for automation if the project's cadence or contributor count
  grew.

## 8. Anticipated Future Architectural Change

If a neural-network surrogate is ever adopted (already framed in
`Historical_Replay/Week_04/STRATEGY_REFLECTION.md` and
`Week_05/STRATEGY_REFLECTION.md` as a deferred option, pending
substantially larger per-function sample sizes), the anticipated
architectural change would be: introduce PyTorch as an additional,
clearly-scoped dependency (recorded in that week's own
`requirements-lock.txt`), and keep any NN-surrogate notebook logic
physically separate from the existing, already-verified `scikit-learn`-based
GP notebooks — following the same per-function, per-week isolation
principle already established, rather than replacing or modifying the
existing GP pipeline in place.
