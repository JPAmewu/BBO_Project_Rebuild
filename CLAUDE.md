# Black-Box Optimisation (BBO) Project — Rebuild

## Project Overview

This is a clean rebuild of an academic Black-Box Optimisation project. The
project optimises **eight expensive black-box functions** using **sequential
weekly observations**. It compares four query-selection strategies:

- Manual search
- Random search
- Grid search
- Bayesian optimisation

Bayesian optimisation uses **Gaussian Process surrogate models**, including
**RBF** and **Matérn** kernels, with acquisition functions including:

- Upper Confidence Bound (UCB)
- Expected Improvement (EI)
- Probability of Improvement (PI)

No experiments have been performed yet. This file describes intended scope,
structure and permanent rules only — it does not claim any results exist.

## Principal Tools

- Python
- NumPy
- pandas
- scikit-learn
- matplotlib
- seaborn

## Permanent Project Rules

These rules apply to all work in this project, in every session, and remain
in force unless the user explicitly changes this file.

1. Preserve all original datasets and never overwrite, delete or fabricate
   observations.
2. Keep each of the eight functions separate and never mix their inputs or
   outputs.
3. Treat weekly datasets as cumulative: validate new observations before
   appending them.
4. Check input/output shapes, dimensionality, bounds, missing values,
   duplicates and repeated query points.
5. Clearly distinguish exploration from exploitation and document
   acquisition-function parameters.
6. Set and record random seeds wherever randomness is used.
7. Avoid data leakage and do not use future weekly outputs when generating an
   earlier query.
8. Before recommending a query, verify that it is within bounds and has not
   already been evaluated.
9. Record model assumptions, kernel settings, hyperparameters, uncertainty
   and limitations.
10. Save code, data, queries, results, documentation and reflections in their
    designated folders.
11. Do not modify historical results merely to improve performance.
12. Do not commit or push anything to GitHub unless expressly instructed.
13. Ask before deleting, renaming, moving or overwriting any file.
14. Use clear academic explanations and distinguish verified results from
    interpretation.
15. Use the global **Data Quality**, **Data Scientist**, **ML Reviewer**,
    **Evaluation**, **Documentation**, **Code Reviewer** and **Code
    Debugger** agents when their specialist review is required.

### Rules from the Official Course FAQ

The following additional permanent rules are sourced directly from the
course's official **Capstone Project FAQs** (Imperial College Executive
Education) and carry the same force as Rules 1–15 above.

16. Every coordinate for all eight functions must lie within
    [0.000000, 0.999999]; 1.000000 is not permitted.
17. All eight functions are maximisation problems.
18. Negative outputs may be valid and expected.
19. Query submissions must use exactly six decimal places, leading zeros and
    hyphens between coordinates, with no brackets, spaces or commas.
20. Weekly datasets are cumulative: validate and append new observations
    without modifying historical observations.
21. Each function must be modelled separately.
22. Do not claim that the unknown function formulas or output scales are
    known.

## Project Structure

The project is organised by week, then by function, then by standard
subfolder. Structure begins at `Week_01` and continues for each subsequent
week. Each week contains all eight functions, `Function_01` through
`Function_08`. Each function contains the same seven standard subfolders.

```
BBO_Project_Rebuild/
├── CLAUDE.md
├── Week_01/
│   ├── Function_01/
│   │   ├── 01_Notebook/
│   │   ├── 02_Data/
│   │   ├── 03_Queries/
│   │   ├── 04_Results/
│   │   ├── 05_Documentation/
│   │   ├── 06_Code_Review/
│   │   └── 07_Reflection/
│   ├── Function_02/
│   │   ├── 01_Notebook/
│   │   ├── 02_Data/
│   │   ├── 03_Queries/
│   │   ├── 04_Results/
│   │   ├── 05_Documentation/
│   │   ├── 06_Code_Review/
│   │   └── 07_Reflection/
│   ├── Function_03/  ... (same 7 subfolders)
│   ├── Function_04/  ... (same 7 subfolders)
│   ├── Function_05/  ... (same 7 subfolders)
│   ├── Function_06/  ... (same 7 subfolders)
│   ├── Function_07/  ... (same 7 subfolders)
│   └── Function_08/  ... (same 7 subfolders)
├── Week_02/
│   └── ... (same structure as Week_01)
└── ...
```

### Standard Subfolders (per function, per week)

1. `01_Notebook` — analysis and experiment notebooks
2. `02_Data` — cumulative validated datasets for that function
3. `03_Queries` — proposed and submitted query points
4. `04_Results` — observed outputs and results
5. `05_Documentation` — model assumptions, kernel/hyperparameter records,
   methodology notes
6. `06_Code_Review` — review outputs from specialist agents
7. `07_Reflection` — weekly reflections and lessons learned

This structure has not yet been created on disk beyond this file; folders
are added as each week/function's work actually begins.
