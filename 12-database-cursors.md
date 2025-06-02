# 🧭 Database Cursors

Database cursors are a powerful tool for managing and processing large datasets in a database. They allow you to retrieve and manipulate rows incrementally, reducing memory usage and improving performance. This chapter explores the concept of cursors, their use cases, and best practices for implementation.

---

## 📚 Table of Contents

1. [Introduction to Cursors](#introduction-to-cursors)
2. [How Cursors Work](#how-cursors-work)
3. [Advantages of Using Cursors](#advantages-of-using-cursors)
4. [Disadvantages of Using Cursors](#disadvantages-of-using-cursors)
5. [Practical Example: Using Cursors](#practical-example-using-cursors)
6. [Best Practices for Using Cursors](#best-practices-for-using-cursors)
7. [Key Takeaways](#key-takeaways)
8. [Quiz Time! 🧠](#quiz-time-)

---

## 🏁 Introduction to Cursors

A **cursor** is a database object that allows you to traverse through the rows of a result set one at a time. Unlike standard queries that return all rows at once, cursors enable incremental processing, which is especially useful for large datasets.

> **Analogy:** Think of a cursor as a pointer that moves through a list of items, allowing you to process one item at a time instead of loading the entire list into memory.

---

## 🔬 How Cursors Work

Cursors operate in the following steps:
1. **Declare:** Define the cursor and associate it with a query.
2. **Open:** Execute the query and initialize the cursor.
3. **Fetch:** Retrieve rows from the result set incrementally.
4. **Close:** Release the resources associated with the cursor.

**Code Example:**
```sql
-- Declare a cursor
DECLARE my_cursor CURSOR FOR
SELECT id, name FROM employees;

-- Open the cursor
OPEN my_cursor;

-- Fetch rows incrementally
FETCH NEXT FROM my_cursor INTO @id, @name;

-- Close the cursor
CLOSE my_cursor;

-- Deallocate the cursor
DEALLOCATE my_cursor;
```

> **Critical Insight:** Cursors are particularly useful when working with procedural logic in SQL, such as looping through rows to perform updates or calculations.

---

## ✅ Advantages of Using Cursors

### 1. **Memory Efficiency**
   - Cursors process rows incrementally, avoiding the need to load the entire result set into memory.

### 2. **Row-Level Processing**
   - Enable row-by-row operations, which are necessary for certain types of logic (e.g., conditional updates).

### 3. **Flexibility**
   - Allow complex procedural logic that cannot be expressed in a single SQL query.

---

## ⚠️ Disadvantages of Using Cursors

### 1. **Performance Overhead**
   - Cursors can be slower than set-based operations because they process rows individually.

### 2. **Resource Consumption**
   - Maintaining a cursor consumes server resources, especially for large datasets.

### 3. **Complexity**
   - Cursors introduce additional complexity compared to simple queries.

> **Tip:** Use cursors sparingly and only when necessary. For most operations, set-based queries are faster and more efficient.

---

## 🧑‍💻 Practical Example: Using Cursors

### Scenario:
You need to calculate a bonus for each employee based on their salary and update their record accordingly.

**Code Example:**
```sql
-- Declare variables
DECLARE @emp_id INT;
DECLARE @salary DECIMAL(10, 2);
DECLARE @bonus DECIMAL(10, 2);

-- Declare the cursor
DECLARE emp_cursor CURSOR FOR
SELECT id, salary FROM employees;

-- Open the cursor
OPEN emp_cursor;

-- Fetch the first row
FETCH NEXT FROM emp_cursor INTO @emp_id, @salary;

-- Loop through all rows
WHILE @@FETCH_STATUS = 0
BEGIN
    -- Calculate bonus
    SET @bonus = @salary * 0.1;

    -- Update the employee's record
    UPDATE employees
    SET bonus = @bonus
    WHERE id = @emp_id;

    -- Fetch the next row
    FETCH NEXT FROM emp_cursor INTO @emp_id, @salary;
END;

-- Close and deallocate the cursor
CLOSE emp_cursor;
DEALLOCATE emp_cursor;
```

> **Example:** This script calculates a 10% bonus for each employee and updates their record using a cursor.

---

## 🏆 Best Practices for Using Cursors

1. **Use Set-Based Operations When Possible:**
   - Always prefer set-based queries over cursors for better performance.
2. **Limit Cursor Scope:**
   - Declare cursors locally and close them as soon as they are no longer needed.
3. **Avoid Unnecessary Fetching:**
   - Fetch only the columns you need to reduce overhead.
4. **Monitor Resource Usage:**
   - Keep an eye on server resources when using cursors for large datasets.

---

## 🧠 Key Takeaways

- **Cursors** allow row-by-row processing of query results, enabling procedural logic in SQL.
- They are useful for memory-efficient processing of large datasets but come with performance and resource trade-offs.
- Use cursors sparingly and prefer set-based operations for most tasks.
- Properly manage cursor lifecycle (declare, open, fetch, close, deallocate) to avoid resource leaks.

---

## Quiz Time! 🧠

1. **What is the main advantage of using a cursor over a standard SQL query?**
2. **Describe a scenario where a cursor is necessary and a set-based query would not suffice.**
3. **What are the risks of leaving cursors open for too long?**

---

[⬅️ Previous: Database Engines](11-database-engines.md) | [⬅️ Back to Course Index](README.md) | [➡️ Next: Security](13-database-security.md)