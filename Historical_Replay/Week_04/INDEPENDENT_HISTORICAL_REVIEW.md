# Independent Historical Review — Week_04 Genuine Historical Pair Import

**Scope:** Independent re-verification, by the ML Reviewer Agent, of the
eight genuine historical Week 4 query-output pairs imported into
`Historical_Replay/Week_04/Function_0X/` and documented in
`HISTORICAL_IMPORT_VALIDATION.md` and `VERIFIED_PAIRS.csv`. This mirrors
the same independent-review step already completed for the Week 1–3
imports, per the project's build → independently verify loop.

**Method:** every value was independently re-derived from the source
project's own primary records — not read back from this rebuild
project's own claims. This included raw byte inspection (`od -c`) of each
`historical_query.txt`/`historical_output.txt`, independent recomputation
of every SHA-256 checksum (`shasum -a 256`), direct extraction of ledger,
`observations.csv`, and `summary.json` rows, direct JSON parsing of all
eight `week_04_gp_diagnostics.ipynb` notebooks (not just a grep), and a
`git status` check of the source project's own working tree.

## Per-Function Results (all 14 required checks)

| Function | d | Query | Output | Result |
|---|---|---|---|---|
| Function_01 | 2 | `0.872626-0.321598` | `-2.4666412561217706e-107` | **PASS** (14/14) |
| Function_02 | 2 | `0.197553-0.808683` | `0.038774938576018964` | **PASS** (14/14) |
| Function_03 | 3 | `0.650132-0.218327-0.110542` | `-0.08251349582236739` | **PASS** (14/14) |
| Function_04 | 4 | `0.715715-0.504442-0.275065-0.552962` | `-9.312811809021998` | **PASS** (14/14) |
| Function_05 | 4 | `0.000000-0.000000-0.000000-0.000000` | `163.1225` | **PASS** (14/14) |
| Function_06 | 5 | `0.973181-0.022807-0.714484-0.145453-0.974790` | `-2.4375082517089726` | **PASS** (14/14) |
| Function_07 | 6 | `0.045103-0.016167-0.773003-0.156031-0.147073-0.616114` | `1.2087334449610816` | **PASS** (14/14) |
| Function_08 | 8 | `0.356502-0.157574-0.172997-0.610561-0.106477-0.260297-0.411421-0.904740` | `9.2540644925525` | **PASS** (14/14) |

Every value above, and every recorded SHA-256 checksum in each function's
`pair_provenance.md`, was matched exactly against an independently
recomputed value — not merely re-read from this project's own prior
documentation.

Formatting-only normalisation was independently confirmed as
value-preserving in two cases: Function_06's fifth coordinate
(source `0.97479` → stored `0.974790`) and Function_08's eighth
coordinate (source `0.90474` → stored `0.904740`) — both trailing-zero
padding to the six-decimal-place convention, not a numeric change.

## Function_05 Duplicate-of-Week_04 Relationship (Check 12)

Independently confirmed in **both directions** directly from the source
ledger:
- The Week 4 row for Function 5 (`[0.0,0.0,0.0,0.0]` → `163.1225`) has an
  empty `duplicate_of` field, confirming it as the original occurrence.
- The Week 7 row for Function 5 in the same ledger carries the identical
  query/output pair and is explicitly flagged `duplicate_of=week:4`.

This confirms `HISTORICAL_REPLAY_PROTOCOL.md` Rule 9 is correctly
documented and ready to be honoured (not double-counted) when Week 7 is
eventually processed.

## Project-Wide Checks

- **VERIFIED_PAIRS.csv agreement (Check 10):** every row matches its
  function's own `historical_query.txt`/`historical_output.txt`
  byte-for-byte. **PASS** for all 8.
- **No GP-notebook origin (Check 11):** direct JSON parsing (not just
  grep) of all eight `week_04_gp_diagnostics.ipynb` notebooks confirmed
  none loads, computes, or references any Week 4 historical query/output
  value, and each notebook's own markdown explicitly states it does not
  run an acquisition function or propose a query. **PASS.**
  - *Minor, non-blocking documentation note*: the notebooks' "data
    boundary" text describes building the Week 4 cumulative dataset by
    "appending... the verified Week 3 pair," phrasing that reads
    ambiguously on a first pass. The reviewer independently confirmed via
    the actual row counts (13,13,18,33,23,23,33,43 — one row short of each
    function's full Week 4 total) that the notebooks correctly use only
    through-Week-3 data and never touch the Week 4 historical pair. This
    is a wording clarity nitpick only, with no effect on any of the 14
    required checks — no correction to the dataset or notebooks is
    needed.
- **No unauthorized modification (Check 13):** all 16
  `cumulative_inputs.npy`/`cumulative_outputs.npy` checksums recomputed
  independently and matched exactly against `CUMULATIVE_DATA_VALIDATION.md`;
  `git status` in the source project (`My_Capstone_1_Imperial`) confirmed
  clean, with no source-project file modified. **PASS.**
- **No premature next-step artifacts (Check 14):** no `03_Queries`
  directory anywhere under `Historical_Replay/Week_04/`, and no `Week_05`
  directory anywhere in the project. **PASS.**

## Open Items Noted by the Reviewer (Not Failures)

- The GP diagnostics notebooks were reviewed statically (JSON/text
  inspection), not executed, during this review — consistent with a
  read-only review and with the notebooks' own stated design (no
  acquisition function, no query proposal).
- Only the source project's current working-tree status was checked
  (`git status`), not its full commit history; the working tree is clean
  and all recomputed checksums match values already on record from before
  this review.

Neither item affects the verdict below; both are scope notes standard to
a read-only, non-executing review.

## Final Verdict

# **PASS — Week_04 historical pair import independently re-verified, 14/14 checks across all 8 functions, all project-wide checks PASS**

No target leakage, no split/version discrepancy, no checksum mismatch, no
fabricated or altered numeric value, no premature acquisition-driven
query, and no unauthorized modification to the source project or to prior
Week 1–3 / cumulative-dataset artifacts was found.
