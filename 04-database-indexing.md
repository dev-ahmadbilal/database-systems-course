# 🏷️ Database Indexing

Indexing is a critical concept in database optimization, enabling faster data retrieval and improving query performance. This chapter dives into the fundamentals of indexing, its types, and best practices for creating and managing indexes effectively. By the end of this section, you'll understand how to optimize your database queries using indexes and avoid common pitfalls.

---

## 📚 Table of Contents

1. [Introduction to Indexing](#introduction-to-indexing)
2. [Types of Indexes](#types-of-indexes)
3. [How Indexes Work](#how-indexes-work)
4. [Best Practices for Creating Indexes](#best-practices-for-creating-indexes)
5. [Composite Indexes](#composite-indexes)
6. [Index-Only Scans](#index-only-scans)
7. [Creating Indexes Concurrently](#creating-indexes-concurrently)
8. [Bloom Filters and Bitmap Indexes](#bloom-filters-and-bitmap-indexes)
9. [Working with Large Tables](#working-with-large-tables)
10. [Key Takeaways](#key-takeaways)

---

## 🔍 Introduction to Indexing

Database indexing is akin to an index in a book—it allows the database to quickly locate specific rows without scanning the entire table. Without indexes, queries would perform a **full table scan**, which becomes inefficient as the dataset grows.

> **Why Use Indexes?**
> - 🚀 **Faster Queries:** Indexes reduce the time required to retrieve data.
> - 🔗 **Efficient Joins:** Indexes help optimize joins between tables.
> - 📊 **Improved Sorting:** Indexed columns can speed up sorting operations.

> **Important:** While indexes improve read performance, they come at the cost of additional storage and slower write operations (inserts, updates, deletes).

**Analogy:**
Think of an index as a library catalog. Instead of searching every bookshelf for a specific title, you consult the catalog to find its exact location.

---

## 🗂️ Types of Indexes

### 1. 🌳 **B-Tree Index**
The most common type of index, ideal for equality and range queries.
- **Example:**
  ```sql
  CREATE INDEX idx_name ON employees(name);
  ```

### 2. 🪓 **LSM Trees**
Used in NoSQL databases like Cassandra, LSM trees are optimized for write-heavy workloads.

### 3. 🟦 **Bitmap Index**
Useful for low-cardinality columns (e.g., gender or status fields).
- **Example:**
  A bitmap index on a "status" column with values like "active" and "inactive."

**Example Table:**
| Index Type   | Best For                | Example Use Case                |
|--------------|-------------------------|---------------------------------|
| B-Tree       | Range/equality queries  | User ID, Name, Date             |
| LSM Tree     | Write-heavy workloads   | Time-series, logs               |
| Bitmap       | Low-cardinality fields  | Gender, Status, Boolean columns |

---

## 🔬 How Indexes Work

Indexes store sorted references to rows in a table, allowing the database to locate data efficiently. When a query is executed, the database checks if an index can be used to speed up the operation.

### Index Scan vs. Full Table Scan
- **Index Scan:** The database uses the index to locate rows.
- **Full Table Scan:** The database scans every row in the table.

**Code Example:**
```sql
-- Without an index
EXPLAIN ANALYZE SELECT * FROM employees WHERE salary > 50000;

-- With an index
CREATE INDEX idx_salary ON employees(salary);
EXPLAIN ANALYZE SELECT * FROM employees WHERE salary > 50000;
```

> **Critical Insight:** Always use `EXPLAIN` or `EXPLAIN ANALYZE` to understand how your queries interact with indexes.

---

## 🏆 Best Practices for Creating Indexes

1. **Index High-Selectivity Columns:**
   - Columns with many unique values (e.g., IDs) benefit more from indexing than low-cardinality columns (e.g., gender).
2. **Avoid Over-Indexing:**
   - Too many indexes can slow down write operations and consume excessive storage.
3. **Use Composite Indexes Wisely:**
   - Combine multiple columns into a single index for queries that filter on those columns together.
4. **Monitor Index Usage:**
   - Periodically check if indexes are being used effectively and remove unused ones.

> **Performance Tip 🚦:** Index only what you query often. Unused indexes are wasted space and slow down writes!

---

## 🧩 Composite Indexes

A composite index spans multiple columns, making it ideal for queries that filter on multiple fields.

**Example:**
```sql
CREATE INDEX idx_composite ON employees(department, salary);

-- Query that benefits from the composite index
SELECT * FROM employees WHERE department = 'Engineering' AND salary > 70000;
```

> **Tip:** The order of columns in a composite index matters. Place the most selective column first.

---

## 🪄 Index-Only Scans

An **index-only scan** occurs when the database retrieves all required data directly from the index without accessing the table.

**Example:**
```sql
CREATE INDEX idx_id_name ON employees(id, name);

-- Query that triggers an index-only scan
EXPLAIN ANALYZE SELECT id, name FROM employees WHERE id = 100;
```

> **Important:** Index-only scans are faster because they avoid accessing the heap (table data). However, they only work if all requested columns are present in the index.

---

## 🏗️ Creating Indexes Concurrently

Creating indexes on large production tables can block writes. To avoid this, use the `CONCURRENTLY` option.

**Code Example:**
```sql
CREATE INDEX CONCURRENTLY idx_concurrent ON employees(department);
```

> **Caution ⚠️:** Concurrent index creation takes longer and consumes more resources but ensures minimal disruption.

---

## 🧪 Bloom Filters and Bitmap Indexes

### Bloom Filters
A probabilistic data structure used to test whether an element exists in a set. Useful for reducing disk I/O in certain scenarios.

### Bitmap Indexes
Bitmap indexes are efficient for low-cardinality columns and can be combined using bitwise operations.

**Example:**
```sql
-- Bitmap index example (syntax may vary by DBMS)
CREATE INDEX idx_bitmap_status ON employees USING BITMAP(status);
```

---

## 🏢 Working with Large Tables

When dealing with millions or billions of rows, consider the following strategies:

1. **Partitioning:**
   - Split large tables into smaller, manageable chunks based on a key (e.g., date or region).
2. **Parallel Processing:**
   - Distribute queries across multiple nodes or cores.
3. **Brute Force:**
   - For very large datasets, sometimes a full table scan is faster than using an index.

**Code Example:**
```sql
-- Partitioning example
CREATE TABLE sales_partitioned (
    sale_id SERIAL,
    sale_date DATE,
    amount NUMERIC
) PARTITION BY RANGE (sale_date);

CREATE TABLE sales_2023 PARTITION OF sales_partitioned
FOR VALUES FROM ('2023-01-01') TO ('2023-12-31');
```

> **Analogy:** Partitioning is like organizing books by genre—finding a specific book becomes much easier.

---

## 🧠 Key Takeaways

- **Indexes** are essential for optimizing query performance but come with trade-offs.
- **B-Tree** indexes are the most common and versatile.
- **Composite Indexes** improve performance for multi-column queries.
- **Index-Only Scans** eliminate the need to access table data.
- **Concurrent Index Creation** minimizes disruption in production environments.
- **Partitioning** and **Parallel Processing** are effective for handling large datasets.

Understanding indexing is crucial for building high-performance database systems. Experiment with different indexing strategies and monitor their impact to find the optimal configuration for your workload.

---

## Quiz Time! 🧠

1. **What is the main trade-off when adding more indexes to a table?**
2. **Describe a scenario where a composite index would significantly improve query performance.**
3. **What is an index-only scan, and why is it faster than a regular index scan?**

---

[⬅️ Previous: Database Internals](03-database-internals.md) | [⬅️ Back to Course Index](README.md) | [➡️ Next: B-Trees & B+Trees](05-b-trees-and-bplus-trees.md)