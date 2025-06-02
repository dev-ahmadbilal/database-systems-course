# 💎 ACID Properties in Database Systems

This chapter explains the fundamental ACID properties—Atomicity, Consistency, Isolation, and Durability—that are critical to relational and many other types of database systems. Understanding these principles is essential for engineers working with databases such as PostgreSQL, MySQL, SQL Server, Oracle, and even some NoSQL or graph databases.

---

## 📚 Table of Contents

1. [Introduction to ACID](#introduction-to-acid)
2. [Transaction Lifecycle](#transaction-lifecycle)
3. [ACID Properties Explained](#acid-properties-explained)
4. [Practical Example: Bank Transfer](#practical-example-bank-transfer)
5. [Additional Notes on Transactions](#additional-notes-on-transactions)
6. [Summary](#summary)
7. [Quiz Time! 🧠](#quiz-time-)

---

## 🧪 Introduction to ACID

**ACID** stands for:

- **Atomicity**
- **Consistency**
- **Isolation**
- **Durability**

These four properties ensure reliable processing of database transactions, which are groups of SQL queries treated as a single unit of work.

> **Key Insight:** ACID is the foundation of trust in database systems. Without it, data integrity and reliability cannot be guaranteed.

### What is a Transaction?

A **transaction** is a collection of one or more SQL queries executed as a single logical unit. Transactions allow multiple operations to be grouped so that they either all succeed or all fail together, maintaining data integrity.

Example: Transferring money between two bank accounts involves multiple steps:
1. Check balance of Account A.
2. Deduct amount from Account A.
3. Add amount to Account B.

All these steps must succeed or fail as one unit.

---

## 🕰️ Transaction Lifecycle

- A transaction begins with the `BEGIN` keyword.
- Multiple queries are executed within the transaction.
- The transaction ends with either:
  - `COMMIT` to save all changes permanently.
  - `ROLLBACK` to discard all changes if any query fails or an error occurs.

### Important Points:

- Until `COMMIT`, changes are not durable (not persisted to disk).
- Transactions can be **read-write** or **read-only**.
- Read-only transactions provide a consistent snapshot of the data at the start of the transaction, useful for generating reports or consistent reads.
- Long transactions are generally discouraged because they can cause performance issues and complex rollback scenarios.
- Databases handle commits and rollbacks differently, optimizing for performance or crash recovery depending on their design.

---

## 💡 ACID Properties Explained

### 1. ⚡ Atomicity

- **Definition:** All queries within a transaction must succeed completely or the entire transaction fails.
- If any query fails (e.g., constraint violation, duplicate key), the entire transaction is rolled back.
- Atomicity ensures the database never ends up in a partial or inconsistent state.

**Example:**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;
```

If the second update fails, the first update must be undone to keep data consistent.

### 2. 🧩 Consistency

- Ensures that a transaction brings the database from one valid state to another.
- All data integrity rules, constraints, and triggers must be preserved.
- Consistency depends on atomicity and isolation working correctly.

**Example:**

- An account balance should never go negative.
- Constraints can be enforced at the database level or in application logic.

> **Critical Point:** Consistency is only as strong as your constraints and business rules!

### 3. 🛡️ Isolation

- Controls how and when the changes made by one transaction become visible to other concurrent transactions.
- Prevents "dirty reads," "non-repeatable reads," and "phantom reads."
- Different isolation levels (Read Uncommitted, Read Committed, Repeatable Read, Serializable) offer trade-offs between performance and strictness.

**Example:**

Two transactions updating the same account simultaneously should not interfere with each other, avoiding inconsistent or unexpected data.

> **Tip:** Use higher isolation levels for critical operations, but be aware of performance trade-offs.

### 4. 🏅 Durability

- Once a transaction is committed, its changes must be permanent, surviving system crashes or power failures.
- Data is persisted to non-volatile storage.
- Some NoSQL databases may sacrifice durability for performance, but understanding durability is crucial.

**Example:**

If the database server crashes right after a `COMMIT`, the changes will still be there after restart.

---

## 🏦 Practical Example: Bank Transfer

```sql
BEGIN;

-- Check balance of Account 1
SELECT balance FROM accounts WHERE account_id = 1;

-- Ensure balance is sufficient (application logic or constraint)

-- Deduct $100 from Account 1
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;

-- Add $100 to Account 2
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

COMMIT;
```

If any step fails, the transaction should be rolled back to prevent inconsistent balances.

> **Performance Tip 🚀:** Keep transactions short to avoid locking resources and impacting performance.

---

## 📝 Additional Notes on Transactions

- Databases implicitly start transactions for single queries if no explicit transaction is started.
- Rollbacks can be triggered manually or automatically if errors occur.
- Crash recovery mechanisms ensure that committed transactions are not lost and incomplete transactions are rolled back on restart.
- The implementation details of transactions vary across database systems like PostgreSQL, MySQL, SQL Server, and Oracle.

---

## 🧠 Summary

- **Transaction:** A logical unit of work consisting of multiple queries.
- **Atomicity:** All-or-nothing execution of transactions.
- **Consistency:** Database rules and constraints are always preserved.
- **Isolation:** Transactions do not interfere with each other.
- **Durability:** Committed changes survive failures.

---

## Quiz Time! 🧠

1. **What does each letter in ACID stand for, and why is each property important?**
2. **Give a real-world example where atomicity prevents data corruption.**
3. **How does isolation level affect transaction performance and safety?**

---

[⬅️ Previous: Introduction & Welcome](01-introduction-and-welcome.md) | [⬅️ Back to Course Index](README.md) | [➡️ Next: Database Internals](03-database-internals.md)

This chapter provides a foundational understanding of ACID properties critical for building reliable and consistent database applications.