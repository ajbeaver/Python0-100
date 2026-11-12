# Python 0–100 — Course Edition 2

A terminal-first course in Python programming and software engineering. Start with execution, values, and control flow; finish with **Mother**, a tested local application that stores supplied observations and learns a small predictive baseline from their history.

## Course components

1. **Textbook.** Execution models, terminology, annotated examples, and engineering decisions.
2. **Lesson exercises.** Predict behavior, investigate it, then implement from written requirements.
3. **Module assessments.** Knowledge checks, independent programming assignments, and design reasoning, totaling 100 points each.
4. **Capstone.** Develop the skills for Mother across the course, then combine them in Module 10.

## Start here

Follow [local setup](setup.md), then begin [Lesson 1.1](module01-foundations/lesson01-execution.md). Read before running examples, write predictions first, and keep your implementation and answers beside each exercise specification. Create source files empty; the course provides requirements rather than completed exercise solutions.

The existing Modules 01–04 and saved learner work are retained. Modules 05–10 extend the original roadmap with three lessons, three exercises, and a 100-point assessment per module. Early lessons remain self-contained; their Mother connection describes preparation rather than a requirement to maintain one evolving program from day one.

## Course textbook structure

| Module | Subject | Mother checkpoint |
|---|---|---|
| 01 | [Foundations](module01-foundations/README.md) | Explain values and represent a supplied observation. |
| 02 | [Control Flow and Functions](module02-control-flow/README.md) | Compute a current warning from explicit inputs. |
| 03 | [Scope, References, and State](module03-scope-state/README.md) | Separate current state from intentional history. |
| 04 | [Modules, Packages, and Imports](module04-modules-packages-imports/README.md) | Build import-safe modules and a package entry point. |
| 05 | [Object-Oriented Programming](module05-object-oriented-programming/README.md) | Represent validated observations and give history a clear owner. |
| 06 | [Software Architecture](module06-software-architecture/README.md) | Define a deterministic pipeline with replaceable input and persistence boundaries. |
| 07 | [Testing, Debugging, and Reliability](module07-testing-debugging-reliability/README.md) | Prove classification, isolation, and failure behavior before adding persistence. |
| 08 | [Files, Persistence, and System Interfaces](module08-files-persistence-system-interfaces/README.md) | Import explicit local observations into a transactional, replayable SQLite history. |
| 09 | [Advanced Python and Machine Learning](module09-advanced-python-machine-learning/README.md) | Fit a small predictive baseline and evaluate it against unseen chronological data. |
| 10 | [Mother — Final Capstone](module10-mother-capstone/README.md) | Deliver a tested local observation application, fitted model, and reproducible evaluation report. |

## Study workflow

Complete lessons in order, then take the module assessment. Save written work in `lessonNN-answers.md` or `assessment-answers.md`, keeping it separate from source and instructions. Mark work complete only when you can reproduce the evidence and explain the results. See [the capstone specification](module10-mother-capstone/lesson01-specification-and-domain.md) for the final authoritative contract.

Mother's required input is explicit local sample data. Its learned component is a fitted per-device mean predictor evaluated against held-out chronological observations and a persistence baseline. Fixed CPU warnings, residual flags, and predictive error are separate concepts. An honest comparison matters more than forcing a model to win.
