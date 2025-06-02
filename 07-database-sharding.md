# 🪓 Database Sharding

Sharding is a database architecture technique that involves splitting a large database into smaller, more manageable pieces called **shards**. Each shard contains a subset of the data and operates independently, enabling horizontal scaling and improving performance for high-volume workloads. This chapter explores the concept of sharding, its benefits, implementation strategies, and challenges.

---

## 📚 Table of Contents

1. [Introduction to Sharding](#introduction-to-sharding)
2. [How Sharding Works](#how-sharding-works)
3. [Types of Sharding](#types-of-sharding)
4. [Benefits of Sharding](#benefits-of-sharding)
5. [Challenges of Sharding](#challenges-of-sharding)
6. [Sharding in Practice](#sharding-in-practice)
7. [Key Takeaways](#key-takeaways)

---

## 🏗️ Introduction to Sharding

Sharding is a method of partitioning a database horizontally across multiple servers or nodes. Each shard is responsible for storing and managing a portion of the data, reducing the load on any single server and enabling scalability.

> **Analogy:** Think of sharding as dividing a library into multiple branches, where each branch holds a subset of the books. Readers can visit the branch that contains the books they need, reducing congestion at any one location.

---

## 🔬 How Sharding Works

In a sharded database:
- Data is divided based on a **shard key**, such as user ID, region, or another unique identifier.
- Each shard resides on a separate server or node.
- Queries are routed to the appropriate shard based on the shard key.

**Example:**
Imagine a `users` table with millions of rows. You can shard this table by user ID:
- Shard 1: User IDs 1–100,000
- Shard 2: User IDs 100,001–200,000
- Shard 3: User IDs 200,001–300,000

> **Critical Insight:** The choice of shard key is crucial. A poorly chosen key can lead to uneven data distribution, known as **shard skew**.

---

## 🗂️ Types of Sharding

### 1. 📏 **Range-Based Sharding**
   - Data is divided based on ranges of the shard key.
   - **Example:** Sharding users by ID ranges (e.g., 1–100,000).

### 2. 🧮 **Hash-Based Sharding**
   - A hash function is applied to the shard key to determine the shard.
   - **Example:** Hashing user IDs to distribute them evenly across shards.

### 3. 📖 **Directory-Based Sharding**
   - A lookup table maps shard keys to specific shards.
   - **Example:** A central registry that directs queries to the correct shard.

### 4. 🌍 **Geographical Sharding**
   - Data is distributed based on geographic regions.
   - **Example:** Storing European users in one shard and Asian users in another.

---

## 🚀 Benefits of Sharding

### 1. **Improved Performance**
   - Queries are executed on smaller datasets, reducing latency.

### 2. **Scalability**
   - Adding new shards allows the system to handle increased loads.

### 3. **Fault Isolation**
   - Failures in one shard do not affect others, improving reliability.

### 4. **Cost Efficiency**
   - Distributing data across cheaper, commodity hardware reduces infrastructure costs.

> **Performance Tip:** Monitor shard sizes and access patterns to avoid hotspots and ensure even distribution.

---

## ⚠️ Challenges of Sharding

### 1. **Complexity**
   - Sharding introduces architectural complexity, requiring careful planning and management.

### 2. **Uneven Data Distribution**
   - Poorly chosen shard keys can lead to imbalanced shards.

### 3. **Cross-Shard Queries**
   - Queries spanning multiple shards are slower and more complex to execute.

### 4. **Rebalancing**
   - Adding or removing shards requires redistributing data, which can be resource-intensive.

> **Example:** If one shard grows significantly larger than others, it may become a bottleneck, necessitating rebalancing.

---

## 🛠️ Sharding in Practice

### Example: Sharding a `Users` Table
Let's shard a `users` table by user ID using hash-based sharding.

**Code Example:**
```sql
-- Create a table for Shard 1
CREATE TABLE users_shard1 (
    id SERIAL PRIMARY KEY,
    username VARCHAR(100),
    email VARCHAR(100)
);

-- Create a table for Shard 2
CREATE TABLE users_shard2 (
    id SERIAL PRIMARY KEY,
    username VARCHAR(100),
    email VARCHAR(100)
);

-- Insert data into the appropriate shard based on hash
INSERT INTO users_shard1 (username, email) VALUES ('alice', 'alice@example.com');
INSERT INTO users_shard2 (username, email) VALUES ('bob', 'bob@example.com');
```

> **Tip:** Use a middleware layer or application logic to route queries to the correct shard.

---

## 🧠 Key Takeaways

- **Sharding** divides a large database into smaller, independent pieces called shards.
- **Shard keys** determine how data is distributed across shards.
- Sharding improves performance, scalability, and fault isolation but introduces complexity.
- Careful planning is essential to avoid issues like uneven data distribution and cross-shard queries.

---

## Quiz Time! 🧠

1. **What is a shard key, and why is its choice so important?**
2. **Describe a scenario where sharding would be necessary for a database.**
3. **What are the main challenges of rebalancing shards?**

---

[⬅️ Previous: Partitioning](06-database-partitioning.md) | [⬅️ Back to Course Index](README.md) | [➡️ Next: Concurrency Control](08-concurrency-control.md)

---
