# Local setup — Python 0–100

Start here even if you have never installed a programming language. Choose the instructions for your operating system; you do not need to complete every platform section. Installation needs an internet connection. Python and all required course tools are free.

## 1. Open a terminal

A terminal is an application where you type commands and press Enter to run them.

| System | Terminal to use |
|---|---|
| macOS | Open **Terminal** from Applications → Utilities, or search for it with Spotlight. |
| Linux | Open your distribution's **Terminal** application. |
| Windows | Open **PowerShell** from the Start menu. |
| Windows using WSL | Open **Ubuntu** from the Start menu and follow the Linux instructions below. |

Copy commands without adding a `$`, `%`, or `>` prompt. Run one line at a time unless a block explicitly creates a file. If you see Python's `>>>` prompt, enter `exit()` to return to the terminal before running shell commands.

## 2. Install Python

The course requires **Python 3.12 or newer**, including the standard-library `sqlite3` module. Choose a supported stable Python 3 release, rather than a preview. No third-party Python packages are required.

### macOS

1. Open the [official Python downloads page](https://www.python.org/downloads/macos/).
2. Select a stable Python 3 release with a macOS installer, download its `.pkg`, and open it. Follow the installer prompts.
3. In Applications, open the newly installed **Python 3.x** folder and run **Install Certificates.command** to complete certificate setup.
4. Close and reopen Terminal, then check:

```sh
python3 --version
```

Expect `Python 3.12.x` or a newer Python 3 version. Keep Apple's system Python in place. See [Python's macOS guide](https://docs.python.org/3/using/mac.html) if installation or command discovery fails.

### Linux, including Ubuntu in WSL

For **Ubuntu 24.04 or newer**, and Debian-based systems whose repositories provide Python 3.12+, run:

```sh
sudo apt update
sudo apt install python3 python3-venv
python3 --version
```

`sudo` may ask for your account password; the terminal usually shows no characters while you type it. Press Enter when finished. Package installation may ask you to confirm before continuing.

On other distributions, use the distribution's package manager to install Python 3 and its virtual-environment support. Package names vary; the commands above are for apt-based systems only. Check the version afterward. If it is below 3.12, use a newer supported distribution or its documented method for installing a newer Python alongside the system interpreter. Do not replace `/usr/bin/python3` manually. See [Ubuntu's Python setup guide](https://ubuntu.com/developers/docs/howto/python-setup/) for distribution-specific guidance.

### Windows with PowerShell

1. Open the [official Python downloads page](https://www.python.org/downloads/windows/) and install the **Python install manager** using the offered installer or its Microsoft Store link.
2. Close and reopen PowerShell, then install a runtime and check it:

```powershell
pymanager install default
py --version
```

Confirm Python 3.12 or newer. If you already have a working supported Python installed, you can keep it and skip installation. The manager command is `pymanager`; an older `py` launcher may not understand install commands. See [Python's Windows guide](https://docs.python.org/3/using/windows.html) for command/alias troubleshooting and installation options when the manager is unavailable.

**Using WSL for the Ethereum course?** Install Python inside Ubuntu using the Linux section above. Windows Python and WSL Python are separate installations; installing one does not set up the other.

## 3. Get the course and open its folder

Open the [Python course repository](https://github.com/ajbeaver/Python0-100), choose **Code → Download ZIP**, and extract it. A GitHub account and Git installation are not needed for this route. If you already use Git, cloning the repository is also fine.

In your terminal, type `cd ` (including the space), followed by the extracted folder's full path in quotes, then press Enter. Replace the example path with your own:

macOS/Linux:

```sh
cd "/path/to/Python0-100-main"
ls
```

Windows PowerShell:

```powershell
cd "C:\path\to\Python0-100-main"
Get-ChildItem
```

You are in the **course root** when the listing includes `README.md`, `setup.md`, and the module folders. Keep extracted files in an ordinary writable folder. Use a text editor to create `.py` files, not a word processor. Windows editors must save the filename as `.py`, rather than `.py.txt`.

## 4. Create a learning environment

A virtual environment gives the course its own Python environment. Run the following from the course root.

### macOS/Linux/WSL

```sh
python3 -m venv .venv
source .venv/bin/activate
python --version
```

### Windows PowerShell

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python --version
```

If PowerShell blocks activation, you can use the environment directly without changing execution policy:

```powershell
.\.venv\Scripts\python.exe --version
.\.venv\Scripts\python.exe -c "print('Python is ready')"
```

In that case, substitute the full path to this executable for `python` in later commands. From deeper exercise directories, use its absolute path. Activation is a convenience, not a requirement.

Activation applies to the current terminal. After opening a new terminal, return to the course root and run the activation command again; you do not need to recreate `.venv`. Run `deactivate` to leave an activated environment.

Early chapters use `python3`; later chapters use `python`. On Windows, use `python` after activation wherever a lesson says `python3`. On Unix-like systems either should select the environment after activation; check the version. A virtual environment isolates dependencies, not operating-system permissions. No `pip install` is needed.

## 5. Verify you are ready

Using the activated environment (or its executable directly), run:

```sh
python -c "import sys, sqlite3; print(sys.version); print('SQLite', sqlite3.sqlite_version); print('Python is ready')"
```

Expect a Python version at least 3.12, a SQLite version, and `Python is ready`. SQLite is accessed through Python; a separate database server or SQLite command-line installation is not required. These checks do not create a database.

If a command is not found, reopen the terminal and check the installation section for your platform. If `venv` is unavailable on an apt-based Linux system, install `python3-venv` for your selected interpreter and retry. If importing `sqlite3` fails, use a Python distribution that includes that module.

You are ready for [Lesson 1.1](module01-foundations/lesson01-execution.md). Return to this guide's coursework conventions below as needed.

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
