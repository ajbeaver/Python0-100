# Local setup — Python 0–100

## Environment

Use Python **3.12 or newer** with the standard-library `sqlite3` module available. The new lessons avoid third-party dependencies and use the explicit SQLite `autocommit` argument introduced in 3.12. Record your actual interpreter and SQLite versions rather than assuming every machine is identical.

Run these commands from a terminal:

```sh
python3 --version
python3 -c "import sqlite3; print(sqlite3.sqlite_version)"
```

If `python3` points to an older interpreter, select an installed 3.12+ interpreter explicitly. Do not replace the operating system's Python installation. On Windows, select a suitable interpreter with the `py` launcher if needed.

## Create one learning environment

From the course root on macOS/Linux:

```sh
python3 -m venv .venv
source .venv/bin/activate
python --version
```

In Windows PowerShell, activate with `.venv\Scripts\Activate.ps1`, or invoke the environment's Python executable directly if local policy prevents activation. Activation changes interpreter selection; it does not grant or restrict OS permissions. No `pip install` is needed for the required coursework.

Existing chapters may show `python3`; the added chapters use `python` after activation. In either case, verify that the chosen command invokes the intended environment.

## Working directories and source

Each exercise specification lives under `moduleNN-.../exercises/lessonNN/`. Create source and written answers there unless the specification names a separate project root. Run `python -m package` from the directory containing that package. Do not run a relative-import package's `__main__.py` directly.

Keep tests named `test_*.py`. From a project root with a `tests/` directory:

```sh
python -m unittest discover -s tests -v
```

Check the number of collected tests. A command that runs zero tests does not validate an implementation. Before Module 07, use the requested scripts and recorded observations; the formal test workflow is introduced in that module.

## Data and evidence

Use the explicit fixed data in each exercise. Modules 08–10 add file input and SQLite. Read and write only paths you deliberately supply for the lab. Use disposable temporary directories for failure tests and database experiments. Required Mother commands do not collect live metrics or access a network.

- Written predictions, commands, observations, reasoning: `lessonNN-answers.md`.
- Assessment responses: `assessment-answers.md` plus the specified assignments.
- Capstone source and fixtures: a separate `mother` project with its own README.
- Capstone generated output and demonstration transcripts: `evidence/`.

Do not overwrite existing learner answers. Keep `.venv/`, `__pycache__/`, generated databases, and local model/report files out of source control. Add ignore rules in each implementation project as appropriate; retain small declared fixture files needed to reproduce results.

## Recovery and reproducibility

A fresh virtual environment does not clear a SQLite database. A fresh process does not remove a saved model. For a clean demonstration, choose a new disposable directory, absent database and model paths, and explicitly copy only the submitted source and fixtures. Record interpreter version, commands, statuses, row counts, and expected/observed metrics.

## References

- [Python tutorial](https://docs.python.org/3/tutorial/)
- [Virtual environments](https://docs.python.org/3/library/venv.html)
- [Unit testing](https://docs.python.org/3/library/unittest.html)
- [SQLite transaction control](https://docs.python.org/3/library/sqlite3.html#transaction-control)
