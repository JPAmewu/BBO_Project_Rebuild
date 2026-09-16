# Historical Replay Inventory — Original Source Project

**Source audited:** `~/Documents/GitHub/My_Capstone_1_Imperial` (read-only inventory
by the Data Quality Agent). **Nothing was copied, edited, renamed, or deleted** —
this is a pure inspection report. This inventory is **not** associated in any way
with the new Week_01 random-search-baseline queries already generated in this
rebuild project (`Week_01/Function_0X/03_Queries/week_01_query.txt`) — those
remain separate, unevaluated queries and are not paired with any historical
output below.

**Legend:** **VERIFIED** = directly read/computed from files, cross-checked
where possible. **CORRECTION** = a documented case where a later record
supersedes an earlier one (both preserved). **ASSUMPTION/INFERENCE** =
inferred rather than directly confirmed. **UNRESOLVED** = could not be
determined from available files, or the files directly contradict each other.

---

## ⚠️ Most Important Finding — Read First

**The source project's own records directly contradict each other about
whether Week 12 has a genuine returned black-box output.** Two
internally-inconsistent layers exist in the same repository, and even a
single file (`Documentation/DATASET_DATASHEET.md`) contradicts itself within
about ten lines of its own text:

- **"Verified" layer** (claims Week 12 outputs ARE real): the canonical
  `Results/query_output_ledger.csv` (v1.2), `Week_12/Function_0X/04_Results/
  observations.csv` and `summary.json` (all 8 functions), and one table in
  `Documentation/DATASET_DATASHEET.md` all show a `recorded`/`verified` Week 12
  query-output pair per function, with `evidence_status =
  verified_cumulative_archive_pair_after_query_record_reconciliation`.
- **"Proposal only, no return" layer** (claims Week 12 was never evaluated):
  `Week_12/01_Queries/README.md` ("no verified Week 12 returned outputs are
  preserved"), `Week_12/Function_0X/06_Documentation/methodology.md` and
  `07_Reflection/README.md` (all 8 functions: "The Week 12 point is a
  proposal, not an observation"), `Results/bbo_query_ledger.csv` (status =
  `proposal_only_return_unavailable`, with a GP-*predicted* mean numerically
  nothing like the "verified" ledger's value), `FINAL_FINDINGS.md` ("no Week
  12 returns are available"), `Documentation/EVALUATION.md`, and a second
  table just above the contradicting one in `Documentation/DATASET_DATASHEET.md`
  itself ("Week 12 proposals | 8 | ... zero verified Week 12 returns").

**This is UNRESOLVED.** The Data Quality Agent could not determine which layer
is authoritative from the files alone, and did not treat either version as
ground truth. **No Week 12 "returned output" value should be treated as
confirmed, used, or replayed into any future rebuild week until a human
decision resolves this contradiction** — e.g. by consulting the original
capstone portal/platform records, or by an explicit decision from the user on
how to treat Week 12 (verified / disputed / excluded pending clarification).
Week 12 values are still recorded in full below, under a clearly marked
UNRESOLVED heading, for completeness — but they must not be treated as
confirmed history.

---

## Structure Discovered

The source repository (a git repo, separate from this rebuild) contains
**Week_01 through Week_13** (13 weeks), each with `Function_01` through
`Function_08` (dimensions 2, 2, 3, 4, 4, 5, 6, 8 for F1–F8 respectively).

Per-function structure (consistent across sampled weeks): `01_Notebook/`,
`02_Code/`, `03_Data/` (`provenance.json`, `verified_cumulative_inputs.npy`,
`verified_cumulative_outputs.npy`; only Week_01 additionally has
`initial_inputs.npy`/`initial_outputs.npy`; only Week_11 additionally has
raw, **quarantined** `function_N_inputs.npy`/`function_N_outputs.npy`),
`04_Results/` (`observations.csv`, `summary.json`), `05_Figures/`,
`06_Documentation/` (`methodology.md`), `07_Reflection/` (`README.md`).
Week-level folders also have `01_Queries/` and `02_Notebook/`. `Week_12/
01_Queries/` additionally has `archive/pre_reconciliation_week_12_query_points.txt`
(see Corrections below). `Week_13/` additionally has a week-level `04_Results/`
with acquisition-comparison/strategy-summary CSVs.

The single authoritative, append-only cross-week/cross-function registry is
`Results/query_output_ledger.csv` (96 rows = Weeks 1–12 × 8 functions),
referenced from every per-function `03_Data/provenance.json` via
`"recorded_pairs_registry": "Results/query_output_ledger.csv"`.

---

## VERIFIED — Exact Historical Query/Output Data, Weeks 1–11

**Source:** `Results/query_output_ledger.csv` (canonical
`verified-query-output-ledger-v1.2`, SHA-256
`303ff186313ea6885c0cf706f2c6c642ae0876cb489cbd950f0faa7e19154c7d`).
Cross-checked value-by-value against `Week_01/Function_0X/04_Results/
observations.csv` + `summary.json` for **all 8 functions** — exact match.
Weeks 2–11 were structurally (not value-by-value) cross-checked via the
cumulative-array prefix test below; full row-by-row cross-check against each
week's own `observations.csv`/`summary.json` was not repeated for every
function in Weeks 2–11 (time budget — see Scope Not Covered).

Format: `Week — date basis — notebook` then `Function: query → returned_output`
(full precision as stored).

### Week 1 — 2026-06-01 (source-file mtime) — `Week_01/02_Notebook/Week_1_Capstone.ipynb`
- F1 `[0.37454, 0.950714]` → `-1.560646704467778e-117`
- F2 `[0.37454, 0.950714]` → `-0.03182956281754251`
- F3 `[0.444444, 0.666666, 0.333333]` → `-0.04090761844901528`
- F4 `[0.555555, 0.444444, 0.222222, 0.111111]` → `-8.727516493155957`
- F5 `[0.224189, 0.84648, 0.879484, 0.878515]` → `1088.8535114737463`
- F6 `[0.728186, 0.154693, 0.732552, 0.693997, 0.564013]` → `-1.1520351120911565`
- F7 `[0.045091, 0.528666, 0.329265, 0.10535, 0.434667, 0.641164]` → `1.0510148516295004`
- F8 `[0.273673, 0.2604, 0.073937, 0.078562, 0.862321, 0.230729, 0.108688, 0.352588]` → `9.8157087929671`

### Week 2 — 2026-06-10 — `Week_02/02_Notebook/Week_2_Capstone.ipynb`
- F1 `[0.536429, 0.835362]` → `1.674933466363685e-36`
- F2 `[0.978752, 0.932731]` → `0.022666631114895516`
- F3 `[0.657452, 0.998464, 0.817253]` → `-0.08987474979637637`
- F4 `[0.00628, 0.28184, 0.932013, 0.960202]` → `-31.73535839431216`
- F5 `[0.255841, 0.841692, 0.888984, 0.86026]` → `1035.6341457754475`
- F6 `[0.433654, 0.486678, 0.282753, 0.972905, 0.323321]` → `-0.8782405650300305`
- F7 `[0.243848, 0.9001, 0.696978, 0.19363, 0.374149, 0.826324]` → `0.308765180253091`
- F8 `[0.16316, 0.184786, 0.152644, 0.083802, 0.999322, 0.544113, 0.184124, 0.123846]` → `9.9399041910574`

### Week 3 — 2026-06-15 — `Week_03/02_Notebook/Week_3_Capstone.ipynb`
- F1 `[0.614168, 0.334143]` → `-1.0755942664604116e-32`
- F2 `[6e-06, 0.334143]` → `0.04868370128566149`
- F3 `[0.670026, 0.057881, 0.658241]` → `-0.18323876643005035`
- F4 `[0.394519, 0.361122, 0.256803, 0.461856]` → `-1.9810750402526334`
- F5 `[0.255842, 0.841692, 0.888985, 0.860266]` → `1035.6647479285914`
- F6 `[0.49507, 0.097152, 0.672921, 7e-06, 0.351268]` → `-1.339542402620705`
- F7 `[0.143585, 0.302559, 0.571101, 0.194533, 0.395561, 0.815792]` → `2.149905456773691`
- F8 `[0.088894, 0.525132, 0.030986, 0.992538, 0.870819, 0.21706, 0.011239, 0.447691]` → `8.9636037557014`

### Week 4 — 2026-06-22 — `Week_04/02_Notebook/Week_4_Capstone.ipynb`
- F1 `[0.872626, 0.321598]` → `-2.4666412561217706e-107`
- F2 `[0.197553, 0.808683]` → `0.038774938576018964`
- F3 `[0.650132, 0.218327, 0.110542]` → `-0.08251349582236739`
- F4 `[0.715715, 0.504442, 0.275065, 0.552962]` → `-9.312811809021998`
- F5 `[0.0, 0.0, 0.0, 0.0]` → `163.1225`
- F6 `[0.973181, 0.022807, 0.714484, 0.145453, 0.97479]` → `-2.4375082517089726`
- F7 `[0.045103, 0.016167, 0.773003, 0.156031, 0.147073, 0.616114]` → `1.2087334449610816`
- F8 `[0.356502, 0.157574, 0.172997, 0.610561, 0.106477, 0.260297, 0.411421, 0.90474]` → `9.2540644925525`

### Week 5 — 2026-06-29 — `Week_05/02_Notebook/Week_5_Capstone.ipynb`
- F1 `[0.897714, 0.081699]` → `7.6512102255565565e-239`
- F2 `[0.555332, 0.360931]` → `0.22078072799826579`
- F3 `[0.522711, 0.090769, 0.451242]` → `-0.05739710166867316`
- F4 `[0.713766, 0.198601, 0.014095, 0.068347]` → `-17.92630659900679`
- F5 `[0.754614, 0.504566, 0.432986, 0.307039]` → `0.9401160531598542`
- F6 `[0.824244, 0.022732, 0.960907, 0.041715, 0.89756]` → `-2.5293691850310744`
- F7 `[0.025337, 0.910075, 0.72779, 0.075231, 0.386516, 0.492629]` → `0.19078262729854223`
- F8 `[0.110992, 0.022977, 0.255313, 0.963757, 0.895383, 0.716713, 0.030436, 0.083412]` → `9.1386093113071`

### Week 6 — 2026-07-06 — `Week_06/02_Notebook/Week_6_Capstone.ipynb`
- F1 `[0.222521, 0.999673]` → `6.131855211153796e-211`
- F2 `[0.473151, 0.950706]` → `0.16053152307795218`
- F3 `[0.418776, 0.420613, 0.695421]` → `-0.11700517654347184`
- F4 `[0.753422, 0.441826, 0.942545, 0.489846]` → `-20.879363209082445`
- F5 `[0.654981, 0.602894, 0.898891, 0.589622]` → `281.9115728306016`
- F6 `[0.78177, 0.192076, 0.805757, 0.695733, 0.56594]` → `-1.3125396070217887`
- F7 `[0.042873, 0.466831, 0.379623, 0.20709, 0.367754, 0.515646]` → `1.611967464642638`
- F8 `[0.115956, 0.821931, 0.587418, 0.022809, 0.44026, 0.96555, 0.368459, 0.301774]` → `8.5570343323444`

### Week 7 — 2026-07-14 — `Week_07/02_Notebook/Week_7_Capstone.ipynb`
- F1 `[0.35042, 0.999543]` → `-6.738616132275143e-151`
- F2 `[0.009675, 0.999881]` → `0.04871438177077586`
- F3 `[0.427731, 0.66981, 0.343678]` → `-0.047183135414436805`
- F4 `[0.096005, 0.028299, 0.033322, 0.077327]` → `-20.19585364168859`
- **F5 `[0.0, 0.0, 0.0, 0.0]` → `163.1225`** — flagged `duplicate_of: week:4` (see Corrections)
- F6 `[0.68383, 0.035248, 0.777884, 0.740721, 0.62931]` → `-1.2870894592671105`
- F7 `[0.121173, 0.544849, 0.282921, 0.037758, 0.374065, 0.591179]` → `0.8994987341856474`
- F8 `[0.006802, 0.833571, 0.147083, 0.461921, 0.26353, 0.887722, 0.724982, 0.315234]` → `8.5636390087854`

### Week 8 — 2026-07-21 — `Week_08/02_Notebook/Week_8_Capstone.ipynb`
- F1 `[0.397837, 0.919531]` → `-1.7723999829558996e-96`
- F2 `[0.314242, 0.973527]` → `0.07276416200318507`
- F3 `[0.457347, 0.664022, 0.295993]` → `-0.07741316335640898`
- F4 `[0.046009, 0.778273, 0.979511, 0.088488]` → `-28.736322947747187`
- F5 `[0.08123, 0.992582, 0.156202, 0.988421]` → `1465.5121917504077`
- F6 `[0.994993, 0.316819, 0.508801, 0.557706, 0.579403]` → `-1.4794522548883542`
- F7 `[0.016174, 0.558963, 0.348588, 0.050763, 0.417748, 0.64403]` → `0.8461801483768777`
- F8 `[0.712683, 0.197193, 0.129585, 0.337018, 0.771044, 0.106613, 0.229839, 0.008464]` → `9.0200908822654`

### Week 9 — 2026-07-25 — `Week_09/02_Notebook/Week_9_Capstone.ipynb`
- F1 `[0.42141, 0.935804]` → `-3.089423814911752e-96`
- F2 `[0.995366, 0.102732]` → `0.049406406222616564`
- F3 `[0.449035, 0.664262, 0.290384]` → `-0.08189344986506433`
- F4 `[0.889493, 0.002958, 0.445975, 0.774547]` → `-23.42280313862202`
- F5 `[0.319924, 0.675406, 0.431628, 1.0]` → `430.8031249775375`
- F6 `[0.711755, 0.17567, 0.738396, 0.689597, 0.562717]` → `-1.1717131510084098`
- F7 `[0.018603, 0.530937, 0.376818, 0.094048, 0.443567, 0.614974]` → `1.009894033457839`
- F8 `[0.321258, 0.232284, 0.021301, 0.0, 0.895967, 0.249315, 0.054463, 0.073967]` → `9.6998918101966`

### Week 10 — 2026-08-09 (see Finding B — lineage/quarantine note) — `Week_10/02_Notebook/Week_10_Capstone.ipynb`
- F1 `[0.379403, 0.071186]` → `-4.5921413403053304e-89`
- F2 `[0.329121, 0.99795]` → `0.02678836560203038`
- F3 `[0.447317, 0.65893, 0.324854]` → `-0.0463387992394308`
- F4 `[0.587928, 0.514452, 0.045793, 0.032616]` → `-15.125730966112929`
- F5 `[0.005552, 0.918769, 0.692571, 0.988052]` → `1424.6366011060773`
- F6 `[0.853051, 0.152023, 0.700695, 0.554516, 0.611033]` → `-1.4490886989479597`
- F7 `[0.107447, 0.625529, 0.309529, 0.130105, 0.44366, 0.667717]` → `0.8456374465318568`
- F8 `[0.546897, 0.163123, 0.136484, 0.078398, 0.771803, 0.070794, 0.119397, 0.111488]` → `9.3736675460081`

### Week 11 — 2026-08-09, same source_snapshot as Week 10 (see Finding B) — `Week_11/02_Notebook/Week_11_Capstone.ipynb`
- F1 `[0.353293, 0.956697]` → `8.159220183163013e-130`
- F2 `[0.384537, 0.996119]` → `0.06529972619341909`
- F3 `[0.450034, 0.667941, 0.342989]` → `-0.038446126253640946`
- F4 `[0.80766, 0.348421, 0.277162, 0.044827]` → `-14.992666345077428`
- F5 `[0.016444, 0.027486, 0.656999, 0.955172]` → `210.03831315192616`
- F6 `[0.67859, 0.144728, 0.730278, 0.715653, 0.604728]` → `-1.154424131709899`
- F7 `[0.280913, 0.512275, 0.321637, 0.188234, 0.420546, 0.641027]` → `1.478174096441659`
- F8 `[0.234325, 0.196964, 0.387716, 0.46245, 0.845967, 0.154645, 0.549698, 0.111557]` → `9.2760687615836`

---

## UNRESOLVED — Week 12 (contradicting sources — see top of document)

**Source (disputed):** `Week_12/01_Queries/week_12_query_points.txt`;
`Results/query_output_ledger.csv` labels these `evidence_status =
verified_cumulative_archive_pair_after_query_record_reconciliation`. This
label is directly contradicted elsewhere in the same repository (see the
finding at the top of this document). **Do not treat as confirmed.**

- F1 `[0.997915, 0.748997]` → `-1.623961943367607e-106`
- F2 `[0.692505, 0.619198]` → `0.6073819387620356`
- F3 `[0.737864, 0.713916, 0.373523]` → `-0.022629319369005918`
- F4 `[0.370439, 0.382409, 0.425133, 0.390888]` → `0.36997528143061276`
- F5 `[0.09922, 0.956523, 0.978678, 0.993262]` → `3546.6319692489096`
- F6 `[0.542094, 0.205654, 0.578843, 0.946832, 0.036709]` → `-0.5378218203873342`
- F7 `[0.108717, 0.301229, 0.470084, 0.22803, 0.331784, 0.836498]` → `2.26680177151999`
- F8 `[0.123374, 0.09797, 0.065526, 0.059094, 0.757317, 0.290599, 0.242285, 0.654268]` → `9.9268353584061`

## Week 13 — No Returned Output Exists Anywhere in the Audited Repository

All sources agree Week 13 is proposals only (no dispute here, unlike Week 12):
`Week_13/01_Queries/week_13_query_points.txt` (queries only),
`Week_13/Function_0X/04_Results/summary.json` (`evidence_gap`: "No verified
Week 13 return is present; confirmed cumulative evidence is available through
Week 12"), and `Documentation/DATASET_DATASHEET.md`. **Do not treat any Week
13 value as a confirmed return.**

**Incidental, out-of-scope observation:** an external, non-repository folder
on this machine (`~/Downloads/Capstone_Folder/week_13_data`) already contains
a `week_13_inputs.txt`/`week_13_outputs.txt` with a genuine 13th row per
function (e.g. Function_01: query `[0.98745, 0.502905]`, output
`-4.447270518867807e-102`), with a file modification date later than
everything inside the audited repository's own Week 13 materials. This data
has **not** been incorporated anywhere into `My_Capstone_1_Imperial` and was
not used, imported, or relied upon anywhere in this report — flagged purely
for awareness, since it lives entirely outside the audited repository and its
own authenticity/provenance was not assessed.

---

## Verification Status Summary

| Week | Verification status | Basis |
|---|---|---|
| 1 | **VERIFIED** — cross-file confirmed | Ledger matches `observations.csv`/`summary.json` exactly for all 8 functions |
| 2–9 | **VERIFIED** (ledger) / structurally cross-checked | Value-by-value per-function cross-check not repeated for every function (time budget); cumulative-array structure confirmed |
| 10 | **VERIFIED**, with a documented and independently-confirmed lineage defect | See Finding B below — raw Week 10/11 archive files were stuck at Week 9/10 content; ledger values were recovered from a later cumulative snapshot and are internally consistent |
| 11 | **VERIFIED** (recovered values), raw per-function arrays **quarantined** | See Finding B |
| 12 | **UNRESOLVED** — internally contradictory sources | See top-of-document finding |
| 13 | **VERIFIED as "no return exists"** (all sources agree) | Universally consistent across all documentation |

---

## Finding B — Data-Lineage Failure Around Weeks 10–11 (Resolved by the Project, Independently Re-Confirmed Here)

- `Results/quarantined_week11_arrays.csv` and `Results/query_output_source_manifest.csv`
  record that the *raw* Week 11 arrays
  (`Week_11/Function_0X/03_Data/function_N_inputs.npy`/`function_N_outputs.npy`)
  failed provenance reconciliation and are quarantined ("Failed prior
  provenance reconciliation; excluded from reconstructed evidence").
- **Independently re-confirmed** (not just taken on the project's word): loading
  `Week_11/Function_01/03_Data/function_1_inputs.npy` directly shows shape
  `(20,2)` — one row short of the corrected `verified_cumulative_inputs.npy`
  (shape `(21,2)`) — and its last row `[0.379403, 0.071186]` is actually
  **Week 10's** value, not a genuine new Week-11 point. The raw archived
  Week 11 file never actually advanced past Week 10.
- An external (non-repository) archive at `~/Downloads/Capstone_Folder`,
  referenced by the ledger's own `source_input`/`source_output` columns, was
  spot-checked (it lies outside the audited repository, so the ledger's own
  "source of truth" cannot be fully verified from the repo alone, but it
  happened to exist on this machine):
  - Week 1's archive file hashes **match exactly** the hashes recorded in
    `Results/query_output_source_manifest.csv` — genuine independent
    cross-file confirmation for at least one week.
  - The external archive's own "Week 10" input file is **byte-identical** to
    its "Week 9" file — independent, out-of-repo confirmation that the
    project's account of the lineage failure is accurate, not merely
    self-reported.
  - The ledger recovered both Week 10's and Week 11's "new" rows from the
    external archive's Week-11 cumulative snapshot (which correctly contains
    both points, one as the second-to-last row, one as the last).
- `Results/query_output_ledger_versions.json` documents this as a genuine,
  preserved correction history: **v1.0** (superseded) → **v1.1** (recovered
  from aligned cumulative source snapshots) → **v1.2** (canonical, Week 12
  rows appended). Superseded versions remain on disk at
  `Results/archive/query_output_ledger_v1.0.csv` and `v1.1.csv`, each with
  its own SHA-256 file.

## Corrections Log (Documented Supersessions, Both Versions Preserved)

1. **Ledger v1.0 → v1.1**: superseded after reconciliation against the
   original cumulative Downloads archive (see Finding B).
2. **Ledger v1.1 → v1.2**: appended 8 Week-12 rows after cumulative-prefix and
   query-record reconciliation (labelled "verified" — but see the top-of-document
   contradiction; this labelling is itself disputed elsewhere in the same
   repository).
3. **Week 12 query file**: an earlier, incorrect version
   (`Week_12/01_Queries/archive/pre_reconciliation_week_12_query_points.txt`)
   "repeated the Week 11 queries and matched none of the supplied Week 12
   queries" (per `Results/week_12_evidence_validation.json`); the corrected
   file is `Week_12/01_Queries/week_12_query_points.txt`.
4. **Week 11 raw arrays**: superseded by reconstructed
   `verified_cumulative_*.npy` files — quarantined, **not deleted**; raw
   files remain on disk, read-only, with their hashes recorded.

## Duplicate Record Found

**Exactly one** duplicate (query, output) pair exists in the entire 96-row
canonical ledger: **Week 7, Function 5** — query `[0.0, 0.0, 0.0, 0.0]`,
output `163.1225` — is identical to **Week 4, Function 5**, and is explicitly
flagged in the ledger itself as `duplicate_of: week:4`. An exhaustive
programmatic scan of all 96 rows for any (function, query-vector) collision
found no other unflagged duplicates.

## Cumulative-Dataset Status — Confirmed (for sampled functions)

`verified_cumulative_inputs.npy` was loaded directly for every week 1–13, for
Function_01 (2D) and Function_08 (8D) as representative cases:
- Row counts grow by exactly 1 per week from Week 1 to Week 12, then stay flat
  Week 12→13 (F1: 11→12→...→21→22→22; F8: 41→42→...→51→52→52) — matching
  `Results/submission_manifest.json`'s recorded canonical post-Week-11 counts
  and `Documentation/DATASET_DATASHEET.md`'s Week-12 table.
- Each week's array is confirmed (via `numpy.allclose`) to be an **exact
  row-for-row prefix** of the next week's array, for every transition 1→2
  through 12→13, for both functions checked.

**ASSUMPTION/INFERENCE:** This exact prefix check was not independently
repeated for Functions 2–7 (time budget). The project's own
`Results/recovery_validation_report.json` and
`Results/week_12_evidence_validation.json` report programmatic checks
("historical_prefix_mismatches": 0, "query_output_row_alignment": "passed")
across all 8 functions, giving moderate-to-high confidence this holds
project-wide, but this is inference from the project's own self-report for
Functions 2–7, not independently re-derived here.

## Chronological Order Evidence

- The `submission_date`/`date_basis` fields in the ledger explicitly
  self-disclose their own limitation: `"source_file_mtime; not platform
  submission timestamp"`. The underlying `recovery_validation_report.json`
  states: `"date_validation": {"status": "source_metadata_only", ...
  "limitation": "No authoritative platform submission timestamps were
  present in the archive."}`
- In-repo `observations.csv` file mtimes are clustered on two dates
  (2026-08-17 for Weeks 1–11, 2026-08-22 for Weeks 12–13) — these are
  **regeneration/freeze timestamps from the recovery process**, not original
  collection dates.
- The only evidence of original per-week chronology is the `submission_date`/
  `archive_date` field sourced from the external archive: 2026-06-01,
  06-10, 06-15, 06-22, 06-29, 07-06, 07-14, 07-21, 07-25, 08-09 (both Week 10
  & 11), 08-19 (Week 12).

**ASSUMPTION:** The ordering Week 1 → Week 12 by increasing archive date is
the best available chronological signal and is internally consistent
(monotonically increasing), but is explicitly **not** an authoritative
platform timestamp, per the project's own admission.

---

## Full Verified / Corrections / Assumptions / Unresolved Summary

**VERIFIED** (directly read/computed, cross-checked):
- Full 96-row canonical ledger content (Weeks 1–12 × F1–F8), reproduced above.
- Week 1 and Week 12 ledger rows cross-confirmed against per-function
  `observations.csv` + `summary.json` for all 8 functions.
- Cumulative-array prefix property (Function_01, Function_08, all 13 weeks).
- Quarantine of raw Week 11 arrays and the specific defect (stuck at Week-10
  content), confirmed by direct `.npy` loading, independent of the project's
  own claim.
- SHA-256 match between the source manifest's recorded hash and the actual
  external Week-1 archive file content.
- Byte-identity of the external archive's "Week 10" and "Week 9" files
  (independent confirmation of the lineage failure).
- Exactly one duplicate (query, output) pair in the ledger, confirmed by
  exhaustive programmatic scan of all 96 rows.
- Ledger version history v1.0 → v1.1 → v1.2, each preserved immutably with
  its own SHA-256.

**CORRECTIONS** (documented supersession, both versions preserved): see the
Corrections Log above (4 items).

**ASSUMPTIONS/INFERENCE:**
- Chronological order of Weeks 1–12 inferred from file-mtime-based date
  fields, not authoritative platform timestamps (self-disclosed limitation).
- Cumulative-prefix property assumed (not individually re-verified) for
  Functions 2–7, based on the project's own passed validation checks.

**UNRESOLVED:**
- Whether Week 12 has a genuine returned output at all (see top-of-document
  finding) — **the single most important open item in this inventory.**
- Whether the external `~/Downloads/Capstone_Folder` archive (outside the
  audited repository) is itself authoritative/current — only spot-checked
  for hash agreement, not fully audited.
- The true provenance of `Results/source_evidence/week_12/week_12_{inputs,outputs}.txt`
  — byte-identical to the external Downloads copy, but no explicit statement
  of how these numbers were obtained (genuine platform returns vs. a
  replay/re-evaluation); their authenticity is actively contested by the
  "no verified return" documentation elsewhere, not merely under-documented.

**Scope not covered in this pass** (time/budget — flagged for completeness,
not treated as a defect): no Jupyter notebooks were opened/executed (only
cited); Weeks 2–11 ledger rows were not value-by-value cross-checked against
every function's own `observations.csv`/`summary.json` (only Week 1 and
Week 12 fully cross-checked; structural checks done for all weeks); `Code/*.py`
scripts were not inspected in depth beyond locating the proposal-vs-verified
contradiction; `Results/gp_*`, sensitivity/performance-summary CSVs, and the
`Notebooks/` folder (derived analysis artifacts, not raw history) were not
examined in detail; no `.png` figures were opened; the external Downloads
archive was not audited beyond the specific hash/diff checks reported above.

No credentials, API keys, or personal/private data were encountered anywhere
in the source project during this audit.
