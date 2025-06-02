# 🏗️ Understanding Database Internals — Storage of Tables and Indexes on Disk

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [What is a Table?](#what-is-a-table)
3. [Row Identifiers (Row IDs)](#row-identifiers-row-ids)
4. [What is a Page?](#what-is-a-page)
5. [Heap Storage](#heap-storage)
6. [Indexes: Accelerating Data Access](#indexes-accelerating-data-access)
7. [How Indexes Work](#how-indexes-work)
8. [Why Indexes Improve Performance](#why-indexes-improve-performance)
9. [Important Considerations](#important-considerations)
10. [Summary Table](#summary-table)
11. [Simple Code Example: Using an Index to Find a Row](#simple-code-example-using-an-index-to-find-a-row)
12. [Final Notes](#final-notes)
13. [Quiz Time! 🧠](#quiz-time-)

---

## 🗄️ Introduction

Databases store data physically on disk in a way that affects how efficiently queries run. Understanding the storage model—how tables and indexes are organized on disk—is critical to mastering database internals and performance tuning.

> **Key Insight:** The way your data is stored and accessed on disk is often the root cause of both slow and fast queries!

---

## 📋 What is a Table?

- A **table** is a logical collection of rows and columns.
- Each **row** represents a record, and each **column** represents a field.
- Although some databases use different data models (e.g., document stores), at the lowest level, all data is stored as bits and bytes.

---

## 🆔 Row Identifiers (Row IDs)

- Most databases assign a **system-maintained unique identifier** to each row, called a **Row ID** or **Tuple ID**.
- This Row ID uniquely identifies the row within the table and is used internally for data retrieval.
- Example: In PostgreSQL, this is called a **Tuple ID**; in MySQL, the primary key often acts as a pseudo Row ID.

---

## 📦 What is a Page?

- A **page** is a fixed-size unit of storage on disk, typically 8KB or 16KB by default (configurable).
- Pages contain multiple rows depending on the row size.
- **Important:** Databases read and write data at the page level, not individual rows.
- When a query accesses data, it loads entire pages into memory, not just single rows or columns.

> **Critical Point:**  
> Disk I/O operations are expensive and are measured in pages read or written. **Minimizing page reads is key to database performance.**

**Example:**
Suppose each page holds 100 rows. If you need to fetch 1,000 rows, the database may need to read 10 pages from disk.

---

## 🗃️ Heap Storage

- The **heap** is the data structure where the actual table rows are stored.
- It consists of a collection of pages with rows stored in no particular order.
- Scanning a heap can be expensive because the database may need to read many pages to find relevant rows.

**Example:**
If you search for a specific employee in a heap without an index, the database may have to scan every page.

---

## 🏷️ Indexes: Accelerating Data Access

- An **index** is a separate data structure that helps pinpoint the exact location of rows in the heap.
- Indexes store keys (e.g., employee IDs) along with pointers (Row IDs) to the corresponding rows in the heap.
- Commonly implemented using **B-tree** data structures, indexes enable efficient searching.

---

## 🔍 How Indexes Work

1. Search the index (B-tree) to find the key.
2. Retrieve the Row ID and page number from the index.
3. Use the Row ID to fetch the exact row from the heap.

**Example:**
Suppose you want to find employee with ID 40.
- The index quickly locates the Row ID and page number for employee 40.
- The database fetches only the relevant page from the heap and retrieves the row.

---

## ⚡ Why Indexes Improve Performance

- Without an index, the database must scan all heap pages sequentially to find a row (a **full table scan**).
- With an index, the database performs a quick B-tree search and fetches only the necessary page(s).
- This drastically reduces the number of pages read and improves query speed.

> **Performance Tip 🚀:** Indexes are your best friend for fast lookups, but over-indexing can slow down writes and consume extra storage.

---

## 🧠 Important Considerations

- Indexes themselves are stored as pages on disk and require I/O to read.
- If an index is very large and does not fit into memory, searching the index can also be costly.
- Not all columns should be indexed; indexing only frequently searched columns is best practice.
- Some databases support **clustered indexes** or **index-organized tables**, where the heap is physically ordered around the index, improving locality and performance.

---

## 📝 Summary Table

| Concept            | Description                                                                                   |
|--------------------|-----------------------------------------------------------------------------------------------|
| Table              | Logical collection of rows and columns                                                       |
| Row ID / Tuple ID   | Unique identifier for each row                                                               |
| Page               | Fixed-size storage unit containing multiple rows                                            |
| Heap               | Unordered collection of pages storing actual table data                                     |
| Index              | Data structure (usually B-tree) storing keys and pointers to rows in the heap               |
| Clustered Index    | Heap physically organized around a single index for faster access                            |

---

## 💻 Simple Code Example: Using an Index to Find a Row

```sql
-- Without index: full table scan
SELECT * FROM employees WHERE employee_id = 10000;

-- With index on employee_id: quick B-tree search + heap fetch
CREATE INDEX idx_employee_id ON employees(employee_id);
SELECT * FROM employees WHERE employee_id = 10000;
```

---

## 🧩 Final Notes

- Understanding how data is stored and accessed on disk helps explain why some queries are slow and others are fast.
- Minimizing page reads and leveraging indexes effectively are key to database performance.
- Future chapters will cover B-tree structures, clustered indexes, and query optimization in more detail.

---

## Quiz Time! 🧠

1. **What is a page in the context of database storage, and why is it important for performance?**
2. **How does an index help the database find a specific row more efficiently than a full table scan?**
3. **What are the trade-offs of adding more indexes to a table?**

---

[⬅️ Previous: ACID Properties](02-acid-properties.md) | [⬅️ Back to Course Index](README.md) | [➡️ Next: Indexing](04-database-indexing.md)