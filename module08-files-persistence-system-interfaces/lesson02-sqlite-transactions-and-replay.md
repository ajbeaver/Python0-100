# Lesson 8.2 — SQLite transactions and replay

**Course:** Python 0–100, Edition 2  
**Module:** [08 — Files, Persistence, and System Interfaces](README.md)  
**Prerequisite:** Lesson 8.1  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Use keys and parameterized SQL.
2. Make batch writes atomic.
3. Distinguish identical replay from conflicting identity.

### Identity makes replay precise

A row in Mother's history is identified by `(device, sequence)`. Reimporting exactly the same identity and CPU is an idempotent replay: it adds nothing. The same identity with a different CPU is a conflict and must reject the entire batch. Avoid a blanket `INSERT OR IGNORE`, which can conceal that conflict.

```sql
CREATE TABLE observations (
    device TEXT NOT NULL,
    sequence INTEGER NOT NULL CHECK (sequence >= 0),
    cpu REAL NOT NULL CHECK (cpu >= 0 AND cpu <= 100),
    PRIMARY KEY (device, sequence)
);
```

SQL defines tables, columns, and constraints. `SELECT` reads rows; `INSERT` adds them. `WHERE` filters, `ORDER BY` establishes report order, and `COUNT(*)` counts rows. Application validation still enforces types, finite values, and the nonempty-device rule.

### Bind values, do not build SQL from input

```python
row = connection.execute(
    "SELECT cpu FROM observations WHERE device = ? AND sequence = ?",
    (device, sequence),
).fetchone()
```

Question marks are placeholders for values. They are not placeholders for table names. `fetchone()` returns a row tuple or `None`. Compare an existing CPU with the incoming validated numeric value; use exact equality for this course's stored replay contract.

### One transaction owns one batch

With Python 3.12+, `sqlite3.connect(path, autocommit=False)` selects explicit transaction behavior. Use `with connection:` around the complete batch: success commits; an exception leaving the block rolls back. Close the connection in `finally`; its transaction context does not close it. Create and commit the schema separately before import so rollback demonstrations focus on row changes.

The resource and transaction structure looks like this. `import_validated_rows` represents your own batch implementation, and must let conflicts escape the transaction block:

```python
import sqlite3

def store_batch(path, rows, import_validated_rows):
    connection = sqlite3.connect(path, autocommit=False)
    try:
        with connection:
            return import_validated_rows(connection, rows)
    finally:
        connection.close()
```

This fragment assumes schema initialization has already committed. A `return` from a successful `with` block still runs its context exit before returning to the caller. Do not catch and suppress a conflict inside that block: normal exit would commit instead of rolling back.

Validate the batch before opening the write operation. Within the transaction, check each existing identity and either skip an identical row, insert a new row, or raise a conflict. This course assumes a single writer; concurrent ingestion needs additional locking and retry design.

A failed batch can leave an initialized empty database file. The required guarantee is unchanged observation rows, not disappearance of the file. On readback, use `ORDER BY device, sequence`; insertion order is not a database promise.

## Self-review

Why does a primary key alone not define replay policy? What does a connection context manage, and what must still be closed?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/sqlite3.html)
