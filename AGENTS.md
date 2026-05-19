# Agentic AI instructions — MS Protein & Peptide Plotter (MSPP)

Goal: Onboard AI coding agents to be immediately productive in this repository.

## 1. Big picture
- **Backend:** Data processor and plotter at `programs/mspp_app/logic.py` & `programs/mspp_app/plot.py` — REST API that ingests DIA‑NN TSVs, uses pandas/numpy and matplotlib (Agg) to return base64 images or binary PNG/ZIP.
- **Frontend:** CustomTkinter GUI app in `programs/mspp_app/gui_app.py`. 
- **Other Tools:** Standalone GUI tools and notebooks live in `programs/`.

## 2. Agent Behavior & Rules of Engagement
- **Plan First:** For complex tasks, propose a concise strategy before modifying files.
- **Search First:** Do not guess paths or imports. Search the workspace to verify existing patterns.
- **Validate Autonomously:** Never assume a change works. Always run `tests/` or frontend linting before concluding your turn.
- **Dependencies:** Do not add new or `pip` packages without explicit user permission.
- **Security:** Never hardcode secrets or absolute paths. Use environment variables.
- **Communication:** Be extremely concise. Avoid conversational filler. Output the minimal amount of text needed to explain your action. 

## 3. Key implementation patterns (inspect these files)
- **Data parsing & caching:** look in `programs/mspp_app/logic.py` for DataProcessor — consensus protein logic (require presence in E25 & E100) is central.
- **Plot generation:** PlotGenerator in `programs/mspp_app/plot.py` exposes `_create_*_figure()` helpers and `_fig_to_base64()`; always close figures to avoid memory leaks.
- **Organism detection:** simple string heuristics (`sp\|` -> HeLa, `ECOLI` -> E.coli, `YEAST` -> Yeast). Update DataProcessor when adding species.

## 4. Developer workflows & Scripts
- **Setup:** Prefer using `python scripts/setup_dev.py` or platform-specific `.sh`/`.ps1` scripts to instantiate the environment. `pixi` is also supported (`pixi run start`).

## 5. Code Quality & Tooling (Strict Requirements)
- **Python Formatting & Linting:** Use `ruff` (`ruff format .` and `ruff check --fix .`). Line length is 100 characters. `mypy` is used for type checking.
- **Python Docstrings:** All new functions and classes must include clear docstrings.
- **Testing:** Backend tests are in `tests/`. Always run `pytest tests/` after modifications. Ensure you add tests for new features (`pytest-cov` is active).
- **Commits:** Follow conventional commits (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`).

## 6. Conventions & gotchas
- **Filename expectations:** `report.pg_matrix_E25_*` and `report.pg_matrix_E100_*` (parser tolerant to `E_25`, `E-25`).
- **Export settings:** PNG exports use dpi=300; reuse `_create_*_figure()` to keep exports consistent.
- **Memory:** always close figures after conversion (`fig.close()` or `plt.close(fig)`).

## 7. Extension examples (copy patterns)
- **Add parsing rule:** modify organism detection in DataProcessor and add a unit test under `tests/` using sample TSVs.

## 8. Files to inspect now
- `programs/mspp_app/gui_app.py` (GUI app launcher)
- `programs/mspp_app/logic.py` and `plot.py` (DataProcessor / PlotGenerator)
- `pyproject.toml` (for tooling configurations)
