# 🔄 Concurrency Control in Databases

Concurrency control is a critical aspect of database management systems (DBMS) that ensures multiple transactions can execute simultaneously without compromising data integrity. This chapter explores the mechanisms and techniques used to manage concurrent access, including locks, isolation levels, and connection pooling.

---

## 📚 Table of Contents

1. [Introduction to Concurrency Control](#introduction-to-concurrency-control)
2. [Types of Locks](#types-of-locks)
3. [Isolation Levels](#isolation-levels)
4. [Connection Pooling](#connection-pooling)
5. [Two-Phase Locking](#two-phase-locking)
6. [Solving the Double Booking Problem](#solving-the-double-booking-problem)
7. [Key Takeaways](#key-takeaways)
8. [Quiz Time! 🧠](#quiz-time-)

---

## 🚦 Introduction to Concurrency Control

Concurrency control ensures that multiple users or processes can interact with a database simultaneously without causing conflicts or inconsistencies. Without proper concurrency control, issues like **dirty reads**, **non-repeatable reads**, and **phantom reads** can occur.

> **Analogy:** Think of concurrency control as traffic lights at an intersection. They ensure smooth flow while preventing collisions.

---

## 🔒 Types of Locks

Locks are mechanisms used to control access to shared resources. There are two primary types of locks:

### 1. 🤝 **Shared Locks**
   - Allow multiple transactions to read a resource but prevent any transaction from writing to it.
   - **Example:** A reporting job acquiring a shared lock on a table for reading.

### 2. 🛑 **Exclusive Locks**
   - Allow only one transaction to write to a resource, blocking all other transactions from reading or writing.
   - **Example:** Updating a user's balance requires an exclusive lock to prevent simultaneous reads or writes.

**Code Example:**
```sql
-- Acquiring a shared lock
BEGIN;
SELECT * FROM accounts WHERE id = 1 FOR SHARE;

-- Acquiring an exclusive lock
BEGIN;
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;
```

> **Critical Insight:** Locks help maintain consistency but can lead to contention if not managed properly.

---

## 🧪 Isolation Levels

Isolation levels define how transactions interact with each other, especially when accessing or modifying the same data concurrently. Each level balances **consistency**, **concurrency**, and **performance** differently. The higher the isolation, the fewer anomalies can occur—but often at the cost of speed or scalability.

### 1. 🟢 **Read Uncommitted**
- **Allows dirty reads:** A transaction can read data that another transaction has modified but not yet committed.
- **Fast but risky:** This level provides the highest concurrency but is vulnerable to anomalies like dirty reads, where a transaction reads data that might later be rolled back.
- **Use case:** Rarely used in practice due to high risk of inconsistencies; may be suitable for read-heavy workloads where perfect accuracy isn't critical.

### 2. 🟡 **Read Committed**
- **No dirty reads:** A transaction only sees data that has been committed.
- **Still allows non-repeatable reads:** If a row is read twice in the same transaction, the values might differ if another transaction modified it in between.
- **Use case:** Common default in many RDBMSs (e.g., SQL Server, Oracle). Good balance for many applications.

### 3. 🟠 **Repeatable Read**
- **Prevents dirty and non-repeatable reads:** Rows read once will return the same values if read again in the same transaction.
- **Phantom reads still possible:** New rows inserted by other transactions may appear in subsequent queries, even if previous rows are stable.
- **Use case:** Useful in financial applications where consistency across multiple reads of the same rows is critical.

### 4. 🔴 **Serializable**
- **Strictest isolation:** Transactions are completely isolated as if executed one after another.
- **Prevents dirty reads, non-repeatable reads, and phantom reads:** It simulates serial transaction execution.
- **Most robust, least concurrent:** Significantly reduces throughput due to locking or other concurrency control mechanisms.
- **Use case:** Best for critical systems where accuracy is more important than performance, such as banking or accounting.

**Code Example:**
```sql
-- Setting isolation level
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

BEGIN;
SELECT * FROM accounts WHERE id = 1;
COMMIT;
```

> **Tip:** Choose the isolation level based on your application's requirements and performance constraints.

---

## 🏊 Connection Pooling

Connection pooling improves performance by reusing database connections instead of creating and destroying them for every transaction. This reduces overhead and enhances scalability.

### Benefits:
- **Reduced Latency:** Reusing connections eliminates the need to establish new ones.
- **Resource Efficiency:** Limits the number of open connections, preventing resource exhaustion.

**Example:**
```python
# Using a connection pool in Python with psycopg2
import psycopg2
from psycopg2 import pool

# Create a connection pool
connection_pool = pool.SimpleConnectionPool(1, 10, user='user', password='password', host='localhost', database='mydb')

# Get a connection from the pool
connection = connection_pool.getconn()
cursor = connection.cursor()
cursor.execute("SELECT * FROM accounts WHERE id = 1")
print(cursor.fetchall())

# Return the connection to the pool
connection_pool.putconn(connection)
```

> **Note:** Connection pooling is especially useful in web applications with high request volumes.

---

## 🏁 Two-Phase Locking

Two-phase locking (2PL) is a protocol that ensures serializability by dividing lock acquisition into two phases:
1. **Growing Phase:** Transactions acquire locks but do not release any.
2. **Shrinking Phase:** Transactions release locks but do not acquire new ones.

**Code Example:**
```sql
-- Transaction using 2PL
BEGIN;

-- Growing phase
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;
UPDATE accounts SET balance = balance + 100 WHERE id = 1;

-- Shrinking phase
COMMIT;
```

> **Important:** While 2PL guarantees consistency, it can lead to deadlocks if not implemented carefully.

---

## 🎟️ Solving the Double Booking Problem

The **double booking problem** occurs when two transactions attempt to book the same resource simultaneously, leading to overbooking.

### Solution:
Use locks to ensure that only one transaction can book a resource at a time.

**Code Example:**
```sql
-- Prevent double booking
BEGIN;

-- Acquire an exclusive lock
SELECT * FROM bookings WHERE room_id = 101 AND date = '2023-12-01' FOR UPDATE;

-- Check availability
SELECT COUNT(*) FROM bookings WHERE room_id = 101 AND date = '2023-12-01';

-- If available, create a booking
INSERT INTO bookings (room_id, date, user_id) VALUES (101, '2023-12-01', 1);

COMMIT;
```

> **Analogy:** Think of this as reserving a seat in a movie theater. Once someone reserves it, no one else can book the same seat.

---

## 🧠 Key Takeaways

- **Concurrency control** ensures data integrity in multi-user environments.
- **Locks** (shared and exclusive) manage access to shared resources.
- **Isolation levels** define how transactions interact, balancing consistency and performance.
- **Connection pooling** improves efficiency by reusing database connections.
- **Two-phase locking** ensures serializability but requires careful implementation to avoid deadlocks.
- Solving issues like **double booking** requires proper use of locks and transaction management.

---

## Quiz Time! 🧠

1. **What is the difference between a shared lock and an exclusive lock?**
2. **How does the isolation level impact transaction safety and performance?**
3. **Describe a real-world scenario where two-phase locking prevents data inconsistency.**

---

[⬅️ Previous: Sharding](07-database-sharding.md) | [⬅️ Back to Course Index](README.md) | [➡️ Next: Replication](09-database-replication.md)