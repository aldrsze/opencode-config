---
name: db-normalize
description: Inspect a live relational schema, find 1NF to BCNF violations and their anomalies, then propose the migration SQL and apply it only after I approve. Use on /db-normalize or when I say "normalize this database" / "fix this table".
---

# Normalize Tables

This skill changes the database. It has no standing authority: inspect freely, but every write needs my explicit yes.

## First
Ask, all at once:
1. Engine and version (Postgres, MySQL, SQLite, SQL Server).
2. How to connect and run SQL (psql, mysql, sqlite3, GUI, or I paste output).
3. Which schema or tables in scope.
4. OLTP or OLAP workload.

Then read only. No DDL until I approve.

## Inspect (read-only)
- Table list, columns, types, PK, FKs, unique constraints, indexes, row counts.
- Sample 5-20 rows per table to see repeating groups and real values.
- Views, triggers, app code, reports that read these tables. A split breaks them; say what breaks.

## Why (the three anomalies)
Tie each violation to the anomaly it actually causes, with a real row from my data:
- **Insertion**: can't add a fact without placeholder or unrelated data.
- **Update**: same value in many rows; miss one row, two truths.
- **Deletion**: deleting one row wipes a fact about something else.

No concrete anomaly, no split. Say so and leave the table alone.

## Order
Sequential. 2NF assumes 1NF, 3NF assumes 2NF, BCNF assumes 3NF. Stop at the first failing form; don't grade higher forms on a table that already fails.

## 1NF (atomic)
Rule: one atomic value per cell. No arrays, no comma lists, no `Phone1` / `Phone2` repeating groups.
Check: each row uniquely identifiable. No PK = fail.
Fix: new table for the repeating values, or a real column per attribute.

## 2NF (no partial dependency)
Rule: needs 1NF. Every non-prime attribute depends on the whole PK.
Check: composite key only. Test each non-prime attribute against each part. Depends on one part alone = partial dependency.
Fix: move that attribute plus the key part it needs into their own table.

## 3NF (no transitive dependency)
Rule: needs 2NF. A non-prime attribute cannot depend on another non-prime attribute.
Check: key, the whole key, nothing but the key. Find `X -> Y` where both are non-prime.
Fix: split `X` and `Y` into their own table, keep `X` as FK.

## BCNF (superkey determinants)
Rule: needs 3NF. For every FD `X -> Y`, `X` is a superkey.
Check: 3NF still allows `X` non-superkey when `Y` is prime. Any `X -> Y` with `X` not a superkey = fail.
Fix: split so the determinant becomes a key.

## Report (before touching anything)
Per table, per form: pass/fail, the FD or column that breaks it, one line why. Use `table.column` and `X -> Y`. No FDs stated: ask, or mark them guesses.
Then the anomalies the current shape actually causes.
Then the plan, numbered, one table at a time:
- Exact SQL per step (CREATE, INSERT from SELECT, ALTER, DROP).
- Row counts you expect to move, and how NULLs or duplicates get resolved.
- What breaks in app code, views, or reports.
- Risk: irreversible? Needs a backup or transaction?

## Approval gate
Ask once: apply step 1? Show the SQL again, short.
- Only my clear "yes" or "apply" counts. Silence, "ok cool", or answering a different point is not approval.
- After each step, show row counts and re-check the table. Then ask again for the next step.
- Destructive step (DROP table, DROP column, delete rows): name the data that disappears and ask separately.
- I say no or stop: stop. Leave the DB as it was, or offer the rollback for steps already applied.

## Safety
- Wrap the whole migration in a transaction when the engine allows; never leave a half-split schema.
- Take a backup or dump first if I have no rollback. Ask.
- Test on a copy or a scratch database first when the data matters.
- Never fabricate rows to satisfy a new PK. NULL/duplicate handling is my decision.

## When not to normalize
Ask about the workload first. Wrong context wastes a migration.
- **OLTP** (writes, storage, integrity): normalize. Split tables.
- **OLAP** (reads, reports, analytics): denormalize. Flatten, star schema, redundancy on purpose. Name the tradeoff: faster reads, update anomaly risk, bigger storage.

## Don't
- Run any DDL or DML before approval.
- Call something 3NF without showing the FDs.
- Rewrite the whole schema in one step.
- Say "redundant" without pointing at the column pair that repeats.
- Pad with praise.
