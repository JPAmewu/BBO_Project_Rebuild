# Historical Replay Protocol

## Purpose

This document governs how, at some future explicitly-instructed point in this
rebuild project, historical query-output data already inventoried in
`HISTORICAL_REPLAY_INVENTORY.md` may be replayed or imported into this
project's own weekly structure (`Week_01`, `Week_02`, ...). It establishes
binding rules for that later work.

**This document does not itself perform any replay or import.** It records
no new historical values, copies no files, and does not act on any of the
data catalogued in `HISTORICAL_REPLAY_INVENTORY.md`. It is a rulebook to be
consulted and applied only when the user explicitly instructs that historical
replay/import work begin, consistent with `CLAUDE.md` Rule 13 ("Ask before
deleting, renaming, moving or overwriting any file") and the project's general
practice of treating documentation and execution as separate steps.

## Status as of This Document's Creation

As of the creation of this document, **no historical data from the original
source project (`~/Documents/GitHub/My_Capstone_1_Imperial`) has been copied
into this rebuild project**, and **no existing Week_01 work or source-project
file has been modified, renamed, moved, or deleted**. `HISTORICAL_REPLAY_INVENTORY.md`
remains a pure, read-only inspection report, and this protocol document only
establishes the rules that will apply *if and when* the user later instructs
that replay/import work be carried out. Nothing described below should be
read as already having happened.

## Legend

This document reuses the verification terminology already established in
`HISTORICAL_REPLAY_INVENTORY.md`, so that every rule below is traceable back
to a specific, labelled finding rather than asserted independently:

- **VERIFIED** — directly read/computed from files, cross-checked where
  possible (per the inventory's own definition).
- **CORRECTION** — a documented case where a later record supersedes an
  earlier one, with both preserved.
- **ASSUMPTION/INFERENCE** — inferred rather than directly confirmed.
- **UNRESOLVED** — could not be determined from available files, or the
  files directly contradict each other.

## Rules Governing Future Historical Replay/Import

### 1. This is a retrospective learning reconstruction, not a live Imperial submission.

Any future replay of historical query-output pairs into this rebuild
project is for retrospective learning, methodology demonstration, and
internal analysis only. It is not, and must not be represented as, a live
submission to the Imperial College Executive Education capstone platform.
No replayed query should be treated as if it were about to be, or had been,
submitted to that platform for a genuine new evaluation. This distinction
must be stated explicitly in any documentation produced during replay, so
that a future reader cannot mistake reconstructed historical work for an
active submission record.

### 2. Historical outputs may be used only with the exact queries that produced them.

A historical output value may never be detached from the exact query vector
that the inventory records as having produced it. Query and output must
always be imported and used together, as the single paired row they appear
as in `HISTORICAL_REPLAY_INVENTORY.md`'s VERIFIED section (sourced from the
canonical `Results/query_output_ledger.csv`, v1.2). Do not substitute a
rounded, reformatted, or approximated query for the exact recorded query
when attaching a historical output value — full precision as stored in the
inventory must be preserved, since query and output are only meaningful as a
matched pair.

### 3. Replay one week at a time and reveal no future-week information.

Historical data must be replayed into this rebuild's week-by-week structure
strictly in order, one week at a time, and no information from a later
historical week may be surfaced, referenced, or used while a query for an
earlier rebuild week is being generated or evaluated. This directly extends
`CLAUDE.md` Rule 7 ("Avoid data leakage and do not use future weekly outputs
when generating an earlier query") to the replay context: even though the
full historical record already exists in the inventory, replay must proceed
as if future weeks were still unknown at the time each earlier week's query
would have been generated, to faithfully reconstruct the original sequential
decision process.

### 4. Preserve the new Week_01 random-baseline queries as unevaluated counterfactual recommendations.

The Week_01 random-search-baseline queries already generated in this rebuild
project (`Week_01/Function_0X/03_Queries/week_01_query.txt`, documented in
`Week_01/WEEK_01_METHOD.md`) are explicitly **not** associated with, and must
not be paired with, any historical output from the source project. As
`HISTORICAL_REPLAY_INVENTORY.md` states at the outset, this inventory "is
**not** associated in any way with the new Week_01 random-search-baseline
queries already generated in this rebuild project... those remain separate,
unevaluated queries and are not paired with any historical output below."
These queries must be preserved exactly as they are: unevaluated
counterfactual recommendations, standing apart from the historical replay
track, never retroactively assigned a historical output value, and never
overwritten or deleted to make room for a replayed historical Week 1 query.

### 5. Weeks 01-09 may be replayed only from verified query-output pairs.

Weeks 1 through 9 may be replayed only using the pairs the inventory labels
**VERIFIED** in its Verification Status Summary table: Week 1 is VERIFIED by
direct cross-file confirmation against `observations.csv`/`summary.json` for
all 8 functions; Weeks 2–9 are VERIFIED via the canonical ledger together
with structural cross-checks (the cumulative-array prefix property). No
value from these weeks may be replayed if it cannot be traced back to the
inventory's VERIFIED section for that week and function. Where the inventory
itself flags a limitation for this range (e.g. that value-by-value
cross-checking was not repeated for every function in Weeks 2–9, "time
budget — see Scope Not Covered"), that limitation must be carried into any
replay documentation, not silently dropped.

### 6. For Weeks 10-11, use only the corrected and traceable recovery/version-history records; do not rely on the stale archived Week_11 arrays.

Weeks 10 and 11 must be replayed only from the corrected, recovered values
documented under "Finding B — Data-Lineage Failure Around Weeks 10–11" in
`HISTORICAL_REPLAY_INVENTORY.md`, and only via the traceable version history
in `Results/query_output_ledger_versions.json` (v1.0 → v1.1 → v1.2). The
inventory independently re-confirmed that the *raw* archived Week 11 arrays
(`Week_11/Function_0X/03_Data/function_N_inputs.npy`/`function_N_outputs.npy`)
are defective: `Week_11/Function_01/03_Data/function_1_inputs.npy` has shape
`(20,2)`, one row short of the corrected `verified_cumulative_inputs.npy`
`(21,2)`, and its last row `[0.379403, 0.071186]` is actually Week 10's value,
not a genuine new Week-11 point — i.e. the raw Week 11 archive never actually
advanced past Week 10. These raw arrays are formally quarantined in the
source project itself (`Results/quarantined_week11_arrays.csv`,
`Results/query_output_source_manifest.csv`) and must remain quarantined in
any future replay: never read from, never used as a source for a Week
10/11 query-output pair, and never presented as if equivalent to the
recovered, corrected values. The values that may be replayed are those
listed under the inventory's "Week 10" and "Week 11" VERIFIED headings,
which were recovered from the external archive's aligned cumulative
snapshots, not from the stale in-repository raw files.

### 7. Quarantine Week_12 completely because its source records contradict one another. Do not treat it as confirmed history without new authoritative evidence.

Week 12 must not be replayed into this rebuild project under any
circumstance until an explicit, separate human decision resolves the
contradiction documented at the top of `HISTORICAL_REPLAY_INVENTORY.md`. The
inventory found that the source project's own records directly contradict
each other about whether Week 12 has a genuine returned output: a "verified"
layer (the canonical ledger v1.2, `Week_12/Function_0X/04_Results/
observations.csv`/`summary.json`, and one table in
`Documentation/DATASET_DATASHEET.md`) claims Week 12 outputs are real, while a
"proposal only, no return" layer (`Week_12/01_Queries/README.md`,
`Week_12/Function_0X/06_Documentation/methodology.md` and
`07_Reflection/README.md`, `Results/bbo_query_ledger.csv` with a
GP-*predicted* mean numerically nothing like the "verified" ledger's value,
`FINAL_FINDINGS.md`, `Documentation/EVALUATION.md`, and a second,
contradicting table in `DATASET_DATASHEET.md` itself) claims Week 12 was
never evaluated. The inventory explicitly marks this **UNRESOLVED** and
states that "No Week 12 'returned output' value should be treated as
confirmed, used, or replayed into any future rebuild week until a human
decision resolves this contradiction." This protocol adopts that conclusion
directly: Week 12 is quarantined in its entirety for replay purposes. It may
only be replayed later if new authoritative evidence (e.g. the original
capstone portal/platform records, or an explicit user decision on how to
treat Week 12) resolves the contradiction — and even then, that resolution
and its basis must be documented before any Week 12 value is used.

### 8. Treat Week_13 as proposal-only because no returned outputs exist.

Week 13 may be replayed only as a set of proposed (not yet evaluated) query
points, never as a week with a returned output. The inventory found that all
sources within the audited source repository agree on this, with no
contradiction (unlike Week 12): `Week_13/01_Queries/week_13_query_points.txt`
contains queries only, each function's `04_Results/summary.json` reports an
`evidence_gap` stating "No verified Week 13 return is present; confirmed
cumulative evidence is available through Week 12," and
`Documentation/DATASET_DATASHEET.md` concurs. Separately, the inventory
flagged — purely for awareness, as an out-of-scope observation — that an
external, non-repository folder (`~/Downloads/Capstone_Folder/week_13_data`)
happens to contain a `week_13_inputs.txt`/`week_13_outputs.txt` with what
appears to be a genuine 13th row per function, with a later file
modification date than everything inside the audited repository's own Week
13 materials. This external data was explicitly **not** incorporated
anywhere into the source project and its authenticity/provenance was not
assessed by the inventory. Consistent with that scope limitation, this
protocol does not authorize treating that external file as a confirmed
Week 13 return either: any future use of it must first be independently
verified and explicitly authorized, not assumed valid merely because it
exists on this machine.

### 9. Record the Week_07 Function_05 repetition of the Week_04 query as a historical duplicate and do not append it twice as a new observation.

The inventory's exhaustive programmatic scan of all 96 rows of the
canonical ledger found exactly one duplicate (query, output) pair in the
entire historical record: **Week 7, Function 5** — query
`[0.0, 0.0, 0.0, 0.0]`, output `163.1225` — is identical to **Week 4,
Function 5**'s query `[0.0, 0.0, 0.0, 0.0]` → `163.1225`, and is explicitly
flagged in the ledger itself as `duplicate_of: week:4`. When replaying
history, this pair must be recorded once, as a single historical
observation attributed to Week 4, with an explicit note that Week 7's
Function 5 query repeated it rather than producing a new observation. It
must **not** be appended a second time as if it were an independent new
data point for Week 7 — doing so would silently double-count a single
underlying observation in any cumulative dataset, count, or model fit built
from the replayed history.

### 10. Preserve all source files and record provenance for every imported pair.

No file in the original source project (`~/Documents/GitHub/My_Capstone_1_Imperial`)
may be modified, renamed, moved, or deleted as part of any future replay
work, consistent with `HISTORICAL_REPLAY_INVENTORY.md`'s own framing ("Nothing
was copied, edited, renamed, or deleted — this is a pure inspection report")
and with `CLAUDE.md` Rule 1 ("Preserve all original datasets and never
overwrite, delete or fabricate observations"). Every query-output pair
imported into this rebuild project must carry a recorded provenance note
identifying: which inventory section and week/function it came from, its
verification status (VERIFIED, CORRECTION, ASSUMPTION/INFERENCE, or
UNRESOLVED, per the legend above), and the underlying source file(s) and
version (e.g. `query_output_ledger.csv` v1.2, per the version history in
`Results/query_output_ledger_versions.json`). This mirrors the recovery
project's own practice of preserving every superseded ledger version on
disk (`Results/archive/query_output_ledger_v1.0.csv`, `v1.1.csv`, each with
its own SHA-256) rather than discarding earlier records — the rebuild
project's own replayed copies must be equally traceable back to a specific,
named source.

### 11. Never fabricate, interpolate, or guess a historical output.

If a historical output value is not present, exactly, in a source file the
inventory has already identified and labelled (VERIFIED or an explicitly
documented CORRECTION), it must not be estimated, interpolated,
back-calculated from a model, or guessed for replay purposes. This applies
with particular force to the weeks the inventory already flags as
UNRESOLVED or as having no genuine return (Week 12, Week 13): the absence of
a confirmed value is itself the correct finding to preserve, not a gap to be
filled by inference. This rule extends `CLAUDE.md` Rule 1 ("...never
overwrite, delete or fabricate observations") directly into the replay
context, and is reinforced by the inventory's own repeated instruction not
to treat disputed or predicted values (e.g. the Week 12 "proposal-only"
layer's GP-predicted mean) as if they were genuine returned outputs.

## Relationship to `CLAUDE.md`

This protocol is additive to, and fully consistent with, the permanent
project rules already recorded in `CLAUDE.md`; it does not override or
conflict with any of them. In particular:

- `CLAUDE.md` Rule 1 ("Preserve all original datasets and never overwrite,
  delete or fabricate observations") directly supports Rules 10 and 11
  above.
- `CLAUDE.md` Rule 7 ("Avoid data leakage and do not use future weekly
  outputs when generating an earlier query") directly supports Rule 3
  above.
- `CLAUDE.md` Rule 11 ("Do not modify historical results merely to improve
  performance") directly supports Rules 6, 7, 9, and 10 above — none of the
  corrections, quarantines, or duplicate-handling described in this
  protocol involve altering a historical result; they only govern which
  already-recorded values may be treated as confirmed for replay.
- `CLAUDE.md` Rule 13 ("Ask before deleting, renaming, moving or
  overwriting any file") and Rule 20 ("Weekly datasets are cumulative:
  validate and append new observations without modifying historical
  observations") together support the requirement, stated in the Purpose
  section above, that this document only takes effect for future,
  explicitly-instructed replay work.

No rule in this protocol authorizes any action that `CLAUDE.md` does not
already permit; where `CLAUDE.md` is silent on a replay-specific detail
(e.g. how to handle a documented internal contradiction like Week 12), this
protocol supplies the missing operational detail without changing any of
`CLAUDE.md`'s permanent rules.
