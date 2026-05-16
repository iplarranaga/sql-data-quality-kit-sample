# SQL Data Quality Kit — Sample Queries

25 Oracle SQL queries to validate data quality before it becomes a problem in production.  
This repository contains **3 sample queries** from the full kit. Each one is ready to run against your own schema.

→ **[Get the full kit (14€)](https://larranaga17.gumroad.com/l/sql-data-quality-kit)** — 25 queries · 5 validation blocks · PDF + .sql file · Validated on Oracle 21c and PostgreSQL

---

## What's in the full kit

The kit covers the five most common data quality failure points in production environments:

| Block | What it catches |
|---|---|
| **1 · Duplicates** | Exact and partial duplicates across key fields |
| **2 · Critical nulls** | Nulls in columns that should never be empty |
| **3 · Referential integrity** | Orphaned records, broken foreign key relationships |
| **4 · Load validation & temporal gaps** | Missing load dates, unexpected gaps in time series |
| **5 · Anomalies & general quality** | Statistical outliers, format violations, suspicious patterns |

---

## Sample queries

### Query 01 — Duplicate detection by key fields

Detects records that share the same combination of key columns. Adapt `col1`, `col2` and `your_table` to your schema.

```sql
-- Block 1: Duplicates
-- Finds rows with identical values across the specified key columns.
-- Useful for identifying ingestion issues or missing deduplication logic.

SELECT
    col1,
    col2,
    COUNT(*) AS num_duplicates
FROM your_table
GROUP BY
    col1,
    col2
HAVING COUNT(*) > 1
ORDER BY num_duplicates DESC;
```

---

### Query 08 — Null check on critical columns

Flags records where a column that should always have a value is empty. Essential before any aggregation or reporting step.

```sql
-- Block 2: Critical nulls
-- Returns rows where a business-critical column contains NULL.
-- Run this after every load to catch upstream issues early.

SELECT
    id,
    col_critical,
    load_date
FROM your_table
WHERE col_critical IS NULL
ORDER BY load_date DESC;
```

---

### Query 13 — Orphaned records (referential integrity)

Identifies records in a child table with no matching parent. Common after partial loads or when cascade deletes are not enforced at the database level.

```sql
-- Block 3: Referential integrity
-- Finds child records with no corresponding parent record.
-- Adapt child_table, parent_table, and the join key to your schema.

SELECT
    c.id,
    c.foreign_key_col,
    c.load_date
FROM child_table c
WHERE NOT EXISTS (
    SELECT 1
    FROM parent_table p
    WHERE p.id = c.foreign_key_col
)
ORDER BY c.load_date DESC;
```

---

## How to use these queries

1. Clone or download this repository
2. Open the `.sql` file in SQL Developer, DBeaver, or any Oracle-compatible client
3. Replace `your_table`, `col1`, `col2`, and key column names with your actual schema
4. Run against your Oracle or PostgreSQL instance (validated on Oracle 21c XE and PostgreSQL)

The queries are intentionally schema-agnostic — they require minimal adaptation to work in any Oracle environment.

---

## Full kit

The complete kit includes 25 queries across all 5 blocks, delivered as:

- **PDF** — formatted and documented, ready to share with your team
- **.sql file** — all queries in a single file, ready to run

**→ [SQL Data Quality Kit — 14€ on Gumroad](https://larranaga17.gumroad.com/l/sql-data-quality-kit)**

Validated on Oracle 21c XE and PostgreSQL.

---

## About

Built by [Ignacio Larrañaga](https://www.linkedin.com/in/iplarranaga), Data Engineer with 10+ years working on data pipelines in complex enterprise environments.

More resources at [ignaciolarranaga.dev](https://www.ignaciolarranaga.dev)  
Contact: info@ignaciolarranaga.dev
