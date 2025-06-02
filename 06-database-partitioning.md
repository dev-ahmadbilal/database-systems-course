# 🧩 Database Partitioning

Partitioning is a powerful technique used to improve the performance and manageability of large database tables. By dividing a table into smaller, more manageable pieces, partitioning allows databases to handle billions of rows efficiently. This chapter explores the concept of partitioning, its benefits, implementation strategies, and best practices.

---

## 📚 Table of Contents

1. [Introduction to Partitioning](#introduction-to-partitioning)
2. [Types of Partitioning](#types-of-partitioning)
3. [How Partitioning Works](#how-partitioning-works)
4. [Benefits of Partitioning](#benefits-of-partitioning)
5. [Partitioning in Practice](#partitioning-in-practice)
6. [Archiving Old Data](#archiving-old-data)
7. [Key Takeaways](#key-takeaways)

---

## 🏗️ Introduction to Partitioning

Partitioning involves splitting a large table into smaller, independent parts called **partitions**. Each partition contains a subset of the data, typically based on a specific criterion like date ranges, IDs, or regions. The database manages these partitions transparently, allowing queries to access only the relevant partitions.

> **Analogy:** Think of partitioning as organizing a library by genre. Instead of searching through all books, you only look in the section that matches your query.

---

## 🗂️ Types of Partitioning

### 1. 📅 **Range Partitioning**
   - Divides data based on a range of values (e.g., dates, IDs).
   - **Example:** Partitioning a `sales` table by year (`2020`, `2021`, `2022`).

### 2. 📝 **List Partitioning**
   - Groups data based on discrete values (e.g., regions or categories).
   - **Example:** Partitioning a `customers` table by region (`North`, `South`, `East`, `West`).

### 3. 🧮 **Hash Partitioning**
   - Distributes data evenly across partitions using a hash function.
   - **Example:** Partitioning a `users` table by hashing user IDs.

### 4. 🧬 **Composite Partitioning**
   - Combines multiple partitioning methods (e.g., range + hash).
   - **Example:** Partitioning a `logs` table by date range and hashing user IDs within each range.

---

## 🔬 How Partitioning Works

When a query is executed, the database determines which partitions are relevant based on the query's filters. Only those partitions are scanned, reducing the amount of data processed.

**Code Example:**
```sql
-- Create a partitioned table
CREATE TABLE sales (
    sale_id SERIAL,
    sale_date DATE,
    amount NUMERIC
) PARTITION BY RANGE (sale_date);

-- Create partitions
CREATE TABLE sales_2023 PARTITION OF sales
FOR VALUES FROM ('2023-01-01') TO ('2023-12-31');

CREATE TABLE sales_2024 PARTITION OF sales
FOR VALUES FROM ('2024-01-01') TO ('2024-12-31');
```

> **Critical Insight:** Partitioning eliminates the need for full table scans by narrowing down the search space.

---

## 🚀 Benefits of Partitioning

### 1. **Improved Query Performance**
   - Queries targeting specific partitions scan less data, resulting in faster execution.

### 2. **Efficient Maintenance**
   - Maintenance tasks like backups, indexing, and vacuuming can be performed on individual partitions.

### 3. **Cost Optimization**
   - Frequently accessed data can reside on high-performance storage, while archival data can be stored on slower, cheaper disks.

### 4. **Scalability**
   - Partitioning enables databases to scale horizontally by distributing data across multiple disks or servers.

> **Performance Tip:** Align your partitioning strategy with your most common query patterns for maximum benefit!

---

## 🛠️ Partitioning in Practice

### Example: Partitioning a `Customers` Table
Let's partition a `customers` table by ID ranges:

**Code Example:**
```sql
-- Create the main table
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
) PARTITION BY RANGE (id);

-- Create partitions
CREATE TABLE customers_1_to_1M PARTITION OF customers
FOR VALUES FROM (1) TO (1000000);

CREATE TABLE customers_1M_to_2M PARTITION OF customers
FOR VALUES FROM (1000001) TO (2000000);
```

> **Tip:** Use tools like `EXPLAIN` to verify that queries target the correct partitions.

---

## 📦 Archiving Old Data

Partitioning makes it easy to archive old or infrequently accessed data. For example, historical data from previous years can be moved to slower storage or even dropped entirely.

**Code Example:**
```sql
-- Move old data to a separate tablespace
ALTER TABLE sales_2020 SET TABLESPACE slow_disk;

-- Drop outdated partitions
DROP TABLE sales_2019;
```

> **Analogy:** Archiving old data is like moving old books to a basement storage room, freeing up space in the main library.

---

## 🧠 Key Takeaways

- **Partitioning** divides large tables into smaller, manageable pieces, improving performance and scalability.
- **Range**, **list**, **hash**, and **composite** partitioning are common strategies, each suited to specific use cases.
- Partitioning reduces query times by limiting the amount of data scanned.
- **Archiving** old data helps optimize storage costs and performance.
- Proper partitioning requires careful planning to align with query patterns and maintenance needs.

---

## Quiz Time! 🧠

1. **What is the main advantage of partitioning a large table?**
2. **Describe a scenario where hash partitioning is preferable to range partitioning.**
3. **How does partitioning make archiving and maintenance easier?**

---

[⬅️ Previous: B-Trees & B+Trees](05-b-trees-and-bplus-trees.md) | [⬅️ Back to Course Index](README.md) | [➡️ Next: Sharding](07-database-sharding.md)