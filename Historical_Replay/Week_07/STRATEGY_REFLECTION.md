# Week 7 Strategy Reflection

**Scope:** A self-directed strategy reflection for this week's Historical
Replay round, consolidating what Week_07's GP diagnostics and historical
pair import actually found — most notably, the confirmed arrival of the
Function_05/Week_04 duplicate long anticipated since Week_04 — and
updating the project's ongoing roadmap toward acquisition-function-driven
querying. No external course prompt accompanied this round; this
reflection follows the same practice used in every prior week regardless.

**Honesty note, consistent with every prior week's reflection**: only
**one** real query has ever been generated for live submission in this
project — the Week 1 random-search baseline. No live Week_02 through
Week_07 query exists. What has actually progressed through seven rounds
is the `Historical_Replay/` track, built on genuine historical data, kept
explicitly separate from any live submission.

## What This Week Actually Found

**Function_05's instability deepened for a sixth consecutive week.**
Across all seven weeks of GP fitting to date, this function has never
once shown both kernels converge cleanly:

| Round | RBF ABNORMAL | Matérn ABNORMAL | Outcome |
|---|---|---|---|
| Week 2 | 1/6 | 3/6 | RBF selected |
| Week 3 | 3/6 | 5/6 | RBF selected (fewer failures) |
| Week 4 | 5/6 | 3/6 | Matérn selected (fewer failures) |
| Week 5 | 4/6 | 4/6 | Neither selected — exact tie |
| Week 6 | 4/6 | 3/6 | Matérn selected (fewer failures) |
| Week 7 | 5/6 | 4/6 | Matérn selected (fewer failures) — **both kernels worse than Week 6** |

This week reversed Week_06's partial improvement: RBF's failure count
rose back to 5/6 (matching its Week_04 peak) and Matérn rose to 4/6.
Seven weeks in, there is still no consistent direction to this
instability — it oscillates rather than trending toward resolution,
which is itself informative: more data alone has not been enough to
stabilise this function's kernel-hyperparameter fitting.

**Function_07 held its kernel selection steady** at RBF, unchanged from
Week_06. Both kernels converged cleanly in both weeks — a stable,
ordinary LML outcome, in contrast to the earlier flip pattern
(Week_03 RBF → Week_04 Matérn → Week_05 Matérn → Week_06 RBF → Week_07
RBF). Two consecutive weeks of agreement suggests this function's kernel
preference may be settling, though two data points is not yet strong
evidence either way.

**All other 6 functions continue to converge cleanly** for both kernels,
with selection resolved purely by likelihood comparison.

**The long-anticipated Function_05/Week_04 duplicate has now arrived and
was handled correctly.** This week's genuine historical pair import
confirmed, via the source ledger's own `duplicate_of: week:4` field
(cross-checked in both directions, plus a third independent source),
that Function_05's Week_07 query-output pair (`[0,0,0,0] → 163.1225`) is
a re-submission of the exact same observation already recorded in
Week_04. Per `HISTORICAL_REPLAY_PROTOCOL.md` Rule 9, this pair is
documented (its own `pair_provenance.md`, historical_query.txt/output.txt
exist for audit-trail completeness) but was explicitly **not** appended
to any cumulative dataset, and never will be for this specific pair —
confirmed independently, including direct inspection that the cumulative
`.npy` files remained genuinely untouched by this import step. This is
the first time in this project that a genuine data-integrity subtlety
flagged four weeks in advance was actually encountered and resolved,
rather than remaining a hypothetical to plan for.

## What This Means for the Roadmap

Nothing here changes the next concrete step already identified across
Weeks 04-06's reflections: **introduce an acquisition function (EI or
UCB) on the current GP posterior**, optimised via multi-start
gradient-based search using the GP's own analytic posterior gradients.
This week reinforces that step from two angles:

1. **Function_05's oscillating, unresolved instability across seven
   weeks is the strongest cumulative argument yet for acquisition-driven
   querying.** Passive replay of whatever historical point comes next has
   given this function seven data points' worth of opportunity to
   stabilise on its own, and it has not. A principled acquisition
   function would let a future query deliberately target the region
   responsible for this instability, rather than continuing to
   accumulate whatever point the historical record happens to provide
   next.
2. **The Function_05 duplicate handling demonstrates why the project's
   audit-trail discipline matters in practice, not just in principle.**
   Rule 9 was written speculatively in Week_04, re-confirmed at every
   subsequent cumulative build, and only now put to its actual test. That
   it held up under a completely independent re-verification this week —
   without needing correction — is a concrete example of why this
   project's build → verify → re-verify loop is worth the overhead: a
   rule written four weeks before it mattered was still correct when it
   finally did.

## What Remains Unresolved

- Function_05's genuine convergence instability, unresolved across all
  seven weeks to date, oscillating rather than trending in either
  direction.
- No acquisition function has been implemented as part of this project's
  actual (non-retrospective) weekly pipeline — the diagnostics remain
  visualisation-only, as in every prior week.
- Function_05's Week 9 out-of-bounds coordinate (flagged during the
  Week_06 import) remains unactioned, correctly deferred until Week 9 is
  reached.
- The minor documentation-wording inconsistency in `Week_07/01_Queries/README.md`
  (boilerplate language superficially resembling the genuine Week 12/13
  dispute, confirmed this week to be leftover copy-paste text, not a real
  dispute) has been noted but not corrected in the source project, since
  this rebuild project does not modify the source project's own files.
