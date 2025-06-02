# 🔁 Database Replication

Database replication is a process where data from one database (the **primary** or **master**) is copied to one or more databases (the **replicas** or **standbys**). This ensures high availability, fault tolerance, and improved read scalability. This chapter explores the fundamentals of replication, its types, implementation, and best practices.

---

## 📚 Table of Contents

1. [Introduction to Replication](#introduction-to-replication)
2. [Types of Replication](#types-of-replication)
3. [How Replication Works](#how-replication-works)
4. [Setting Up Replication in PostgreSQL](#setting-up-replication-in-postgresql)
5. [Replication Use Cases](#replication-use-cases)
6. [Key Takeaways](#key-takeaways)
7. [Quiz Time! 🧠](#quiz-time-)

---

## 🌍 Introduction to Replication

Replication ensures that data is consistently synchronized across multiple database instances. It is critical for disaster recovery, load balancing, and geographically distributed systems.

> **Analogy:** Think of replication as creating multiple copies of a document. If one copy is lost or damaged, you can rely on the others.

---

## 🗂️ Types of Replication

### 1. 🔗 **Synchronous Replication**
   - Ensures that data is written to both the primary and replica(s) before acknowledging the transaction.
   - Guarantees strong consistency but may introduce latency.

### 2. ⚡ **Asynchronous Replication**
   - Data is written to the primary first, and changes are propagated to replicas later.
   - Provides higher performance but risks data loss in case of a failure.

### 3. 🟠 **Semi-Synchronous Replication**
   - A hybrid approach where at least one replica must acknowledge receipt of the data before the transaction is considered complete.

### 4. 🧩 **Logical Replication**
   - Replicates specific tables or rows instead of the entire database.
   - Useful for selective data synchronization.

**Example Table:**
| Replication Type   | Consistency | Performance | Use Case                        |
|--------------------|-------------|-------------|----------------------------------|
| Synchronous        | Strong      | Lower       | Banking, critical data           |
| Asynchronous       | Eventual    | Higher      | Analytics, reporting             |
| Semi-Synchronous   | Balanced    | Balanced    | E-commerce, mixed workloads      |
| Logical            | Flexible    | Varies      | Selective sync, migrations       |

---

## 🔬 How Replication Works

In a typical replication setup:
1. The **primary database** processes write operations and logs them in a **Write-Ahead Log (WAL)**.
2. The **replica(s)** continuously fetch the WAL and apply the changes to stay in sync.
3. Applications can query replicas for read operations, reducing the load on the primary.

> **Critical Insight:** Replication introduces trade-offs between consistency, availability, and performance. Choose the type of replication based on your application's requirements.

---

## 🛠️ Setting Up Replication in PostgreSQL

Let's walk through setting up replication in PostgreSQL step by step:

### Step 1: Configure the Primary Database
1. Update the `postgresql.conf` file to enable replication:
   ```conf
   wal_level = replica
   max_wal_senders = 3
   ```
2. Add a replication user in `pg_hba.conf`:
   ```conf
   host replication replicator 0.0.0.0/0 md5
   ```

### Step 2: Create a Standby Database
1. Stop the standby instance and copy the data directory from the primary:
   ```bash
   rsync -av --progress /path/to/primary/data/ /path/to/standby/data/
   ```
2. Update the `recovery.conf` file on the standby:
   ```conf
   standby_mode = 'on'
   primary_conninfo = 'host=primary_host port=5432 user=replicator password=your_password'
   ```

### Step 3: Start Both Instances
1. Restart the primary and standby databases.
2. Verify replication by performing a write operation on the primary and checking if it appears on the standby.

**Code Example:**
```sql
-- On the primary
CREATE TABLE test_replication(id SERIAL PRIMARY KEY, data TEXT);
INSERT INTO test_replication(data) VALUES ('Hello, Replication!');

-- On the standby
SELECT * FROM test_replication;
```

> **Tip:** Use monitoring tools like `pg_stat_replication` to track replication status.

---

## 🚦 Replication Use Cases

### 1. **High Availability**
   - Replicas act as failover instances in case the primary goes down.

### 2. **Read Scalability**
   - Offload read queries to replicas, reducing the load on the primary.

### 3. **Disaster Recovery**
   - Maintain backups in geographically distributed locations.

### 4. **Data Analysis**
   - Use replicas for reporting and analytics without impacting the primary.

> **Performance Tip:** Use asynchronous replicas for analytics to avoid impacting primary write performance.

---

## 🧠 Key Takeaways

- **Replication** ensures data consistency and availability across multiple database instances.
- **Synchronous**, **asynchronous**, and **semi-synchronous** replication offer different trade-offs between consistency and performance.
- Logical replication allows selective synchronization of tables or rows.
- Proper configuration and monitoring are essential for maintaining a healthy replication setup.

---

## Quiz Time! 🧠

1. **What is the main difference between synchronous and asynchronous replication?**
2. **Describe a scenario where logical replication would be preferable to physical replication.**
3. **Why is monitoring replication status important in production systems?**

---

[⬅️ Previous: Concurrency Control](08-concurrency-control.md) | [⬅️ Back to Course Index](README.md) | [➡️ Next: System Design](10-database-system-design.md)