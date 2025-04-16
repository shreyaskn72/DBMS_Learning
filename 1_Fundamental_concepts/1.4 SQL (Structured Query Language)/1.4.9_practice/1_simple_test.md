Question grouped by difficulty:

---

### 🟢 **Basic Level – Answers**

1. **Difference between `WHERE` and `HAVING`:**  
   - `WHERE` filters rows before grouping.  
   - `HAVING` filters groups after aggregation.

2. **Retrieve all columns from `students`:**  
   ```sql
   SELECT * FROM students;
   ```

3. **Primary Key:**  
   - Uniquely identifies each record in a table.  
   - Cannot contain `NULL` and must be unique.

4. **Purpose of `LIMIT`:**  
   - Restricts the number of rows returned by a query.  
   ```sql
   SELECT * FROM students LIMIT 5;
   ```

5. **Insert a new row:**  
   ```sql
   INSERT INTO students (name, age, class) VALUES ('John', 16, '10A');
   ```

6. **`CHAR` vs `VARCHAR`:**  
   - `CHAR(n)`: Fixed-length string, padded with spaces.  
   - `VARCHAR(n)`: Variable-length string, saves space.

7. **`GROUP BY` use:**  
   - Groups rows sharing a property so aggregate functions can be applied.  
   ```sql
   SELECT class, COUNT(*) FROM students GROUP BY class;
   ```

---

### 🟡 **Intermediate Level – Answers**

1. **Second highest salary:**  
   ```sql
   SELECT MAX(salary) 
   FROM employees 
   WHERE salary < (SELECT MAX(salary) FROM employees);
   ```

2. **Indexes:**  
   - Speed up data retrieval.  
   - They store pointers to data in a way that makes searches faster.  
   - But they slow down inserts and updates slightly.

3. **Joins:**
   - `INNER JOIN`: Only matching rows.  
   - `LEFT JOIN`: All from left, plus matches from right.  
   - `RIGHT JOIN`: All from right, plus matches from left.  
   - `FULL OUTER JOIN`: All records from both (not natively supported in MySQL, emulate using `UNION`).

4. **Prevent SQL Injection:**  
   - Use **prepared statements** or **parameterized queries**.  
   - Never concatenate user input directly into queries.

5. **Count students per class:**  
   ```sql
   SELECT class, COUNT(*) FROM students GROUP BY class;
   ```

6. **Foreign key vs Primary key:**  
   - **Primary Key:** Unique, not null, identifies a record.  
   - **Foreign Key:** Refers to primary key in another table, used for relationships.

7. **`AUTO_INCREMENT`:**  
   - Automatically generates a unique number for new rows in a column.

---

### 🔴 **Advanced Level – Answers**

1. **Optimize a slow query:**
   - Use indexes.  
   - Avoid `SELECT *`.  
   - Use `EXPLAIN` to analyze.  
   - Reduce nested subqueries.  
   - Cache results if possible.

2. **`InnoDB` vs `MyISAM`:**
   - `InnoDB`: Supports transactions, foreign keys, row-level locking.  
   - `MyISAM`: Faster for read-heavy tasks, no transactions, table-level locking.

3. **Recursive CTE for employee hierarchy:**
   ```sql
   WITH RECURSIVE emp_hierarchy AS (
     SELECT id, name, manager_id FROM employees WHERE manager_id IS NULL
     UNION ALL
     SELECT e.id, e.name, e.manager_id
     FROM employees e
     INNER JOIN emp_hierarchy eh ON e.manager_id = eh.id
   )
   SELECT * FROM emp_hierarchy;
   ```

4. **Transactions & ACID:**
   - **Transaction:** A set of SQL operations executed as a unit.  
   - **ACID:**
     - **Atomicity** – All or nothing.
     - **Consistency** – From valid to valid state.
     - **Isolation** – Transactions don’t interfere.
     - **Durability** – Once committed, changes are permanent.

5. **Use of `EXPLAIN`:**
   ```sql
   EXPLAIN SELECT * FROM students WHERE class = '10A';
   ```
   - Shows how MySQL executes the query: indexes used, table scans, etc.

6. **Find duplicate emails:**
   ```sql
   SELECT email, COUNT(*) 
   FROM users 
   GROUP BY email 
   HAVING COUNT(*) > 1;
   ```

7. **Full-text search:**
   - Add full-text index:
     ```sql
     ALTER TABLE articles ADD FULLTEXT(title, body);
     ```
   - Search:
     ```sql
     SELECT * FROM articles 
     WHERE MATCH(title, body) AGAINST('mysql tutorial');
     ```

---
