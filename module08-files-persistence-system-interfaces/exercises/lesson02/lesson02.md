# Exercise 8.2 — SQLite transactions and replay

**Textbook:** [SQLite transactions and replay](../../lesson02-sqlite-transactions-and-replay.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Existing rows contain (node01, 0, 20). A batch adds (node02, 0, 30), then conflicts with (node01, 0, 21). Predict the committed row count after rollback.

## Part B — Investigate and build

Create `storage.py` and `test_storage.py`. Implement schema initialization, `import_batch`, and ordered readback using the stated identity policy. In temporary databases test first import, identical replay, new rows, a conflicting row late in a batch, and persistence after close/reopen. Verify row counts and actual values before and after rollback. Use placeholders even for a device name containing an apostrophe. Do not treat malformed input as a duplicate.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Why does a primary key alone not define replay policy? What does a connection context manage, and what must still be closed?

## Submission

Keep written work in `lesson02-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
