# CLAUDE.md

Guidance for AI assistants (and humans) working in this repository.

## What this project is

A notebook-based data-science study of how generative-AI usage patterns relate
to student learning outcomes. The scientific framing is the **mode-of-engagement
hypothesis**: outcomes are driven by *how* GenAI is used, not *how much*. Read
`README.md` for the full framing, research questions, and variable dictionary
before making analytical changes.

## Hard constraints

- **Never commit or push.** Only the repository owner (Filipe) commits. Do not
  run `git commit`, `git push`, `git add` with intent to commit, or any history-
  rewriting command. Staging for inspection is acceptable only if explicitly
  requested.
- **Notebooks only.** The deliverables are Jupyter notebooks. Do not introduce a
  `src/` package or move analysis logic into importable modules unless the owner
  asks. Shared setup is repeated in each notebook's first cells by design, so
  every notebook runs standalone (after Notebook 01).
- **The raw CSV is immutable.** Treat `data/ai_student_impact_dataset (1).csv`
  as read-only. All cleaned or derived tables go to `data/processed/`.

## Environment

- Python 3.14. Use the project virtual environment at `.venv`.
- Dependencies are pinned in `requirements.txt`. Note that `shap` and other
  `numba`-dependent packages do **not** build on Python 3.14; model explanations
  use scikit-learn permutation importance instead.
- Run a notebook headless with:
  ```bash
  .venv/bin/jupyter nbconvert --to notebook --execute --inplace \
      --ExecutePreprocessor.timeout=1200 notebooks/<name>.ipynb
  ```

## Conventions

- **Execution order.** Notebook 01 produces the processed analysis table that
  02-04 consume. Always run 01 first after changing feature engineering.
- **Reproducibility.** `RANDOM_STATE = 42` everywhere a seed is needed.
- **Visual system.** All figures use one colour-vision-safe palette defined in
  the setup cell of each notebook (categorical, sequential, and diverging
  roles). Do not introduce matplotlib default colour cycling or rainbow
  colormaps. Keep marks thin, grids recessive, and label series directly rather
  than relying on colour alone. Sequential encodings use a single-hue ramp;
  diverging encodings use blue↔red with a neutral midpoint.
- **Writing style.** Markdown narrative in notebooks and docs is scientific and
  plain — no emojis. State claims with the evidence that supports them.
- **Causal language.** The data are cross-sectional and self-reported. Use
  associational language ("is associated with", "predicts") and avoid causal
  verbs unless discussing the within-student GPA change measure explicitly, and
  even then with stated caveats.

## Notebook roles

| Notebook | Responsibility |
|---|---|
| `01_data_understanding_and_quality.ipynb` | Profiling, QA, feature engineering, writes `data/processed/` |
| `02_exploratory_data_analysis.ipynb` | Visual EDA on the processed table |
| `03_statistical_inference.ipynb` | Hypothesis tests, ANOVA, OLS regression |
| `04_predictive_modeling.ipynb` | ML classification and regression, model comparison, importance |

## When extending the work

- Add new engineered features in Notebook 01 and re-run downstream notebooks so
  the processed table and all analyses stay consistent.
- Keep each notebook's first two cells as the standard setup (imports + palette).
- Prefer adding a new numbered notebook over overloading an existing one when a
  distinct analytical stage is introduced.
