Here’s a **comprehensive list of MySQL interview questions with sample answers**, categorized by difficulty. This can be super helpful for preparing interviews, tests, or even building a curriculum.

---

## 🟢 **Basic SQL Querying Questions with Answers**

1. **What is the difference between `SELECT *` and selecting specific columns?**  
   `SELECT *` retrieves all columns, which may include unnecessary data. Selecting specific columns (e.g., `SELECT id, name`) is more efficient and improves performance.

2. **How do you filter rows using the `WHERE` clause?**  
   Use `WHERE` to specify conditions:  
   ```sql
   SELECT * FROM students WHERE age > 18;
   ```

3. **What does the `LIKE` operator do? Give examples.**  
   `LIKE` is used for pattern matching:  
   ```sql
   SELECT * FROM users WHERE name LIKE 'J%'; -- names starting with 'J'
   ```

4. **How do you sort query results using `ORDER BY`?**  
   ```sql
   SELECT * FROM products ORDER BY price DESC;
   ```

5. **Explain the use of `DISTINCT` in queries.**  
   Removes duplicates:  
   ```sql
   SELECT DISTINCT department FROM employees;
   ```

6. **How do you limit the number of results returned?**  
   Use `LIMIT`:  
   ```sql
   SELECT * FROM products LIMIT 5;
   ```

7. **What is the use of the `BETWEEN` keyword?**  
   Filters values in a range:  
   ```sql
   SELECT * FROM orders WHERE price BETWEEN 100 AND 500;
   ```

8. **How is `NULL` handled in SQL comparisons?**  
   Use `IS NULL` or `IS NOT NULL`:  
   ```sql
   SELECT * FROM employees WHERE manager_id IS NULL;
   ```

9. **What does the `IN` operator do?**  
   Tests for values in a list:  
   ```sql
   SELECT * FROM customers WHERE country IN ('USA', 'Canada');
   ```

10. **Write a query to find the total number of records in a table.**  
   ```sql
   SELECT COUNT(*) FROM orders;
   ```

---

## 🟡 **Intermediate SQL Querying Questions with Answers**

1. **What is the difference between `INNER JOIN`, `LEFT JOIN`, and `RIGHT JOIN`?**  
   - `INNER JOIN`: only matching rows  
   - `LEFT JOIN`: all left + matching right  
   - `RIGHT JOIN`: all right + matching left

2. **How do you group results using `GROUP BY`?**  
   ```sql
   SELECT department, COUNT(*) FROM employees GROUP BY department;
   ```

3. **What is the purpose of the `HAVING` clause?**  
   Filters groups after aggregation:  
   ```sql
   SELECT department, COUNT(*) FROM employees GROUP BY department HAVING COUNT(*) > 5;
   ```

4. **How do you find duplicate values in a table?**  
   ```sql
   SELECT email, COUNT(*) FROM users GROUP BY email HAVING COUNT(*) > 1;
   ```

5. **Write a query to get the second highest salary.**  
   ```sql
   SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary) FROM employees);
   ```

6. **How do you update rows based on a condition in another table?**  
   ```sql
   UPDATE employees
   SET department_id = 2
   WHERE id IN (SELECT id FROM employees WHERE department_id = 1);
   ```

7. **Explain the difference between `UNION` and `UNION ALL`.**  
   - `UNION`: combines results and removes duplicates  
   - `UNION ALL`: includes duplicates

8. **How do you find customers who have never placed an order?**  
   ```sql
   SELECT * FROM customers
   WHERE id NOT IN (SELECT customer_id FROM orders);
   ```

9. **What is a subquery? Write a query using one.**  
   A query within another query:  
   ```sql
   SELECT name FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);
   ```

10. **How do you calculate the total revenue from multiple columns?**  
   ```sql
   SELECT SUM(quantity * price) AS total_revenue FROM order_items;
   ```

---

## 🔴 **Advanced SQL Querying Questions with Answers**

1. **What are window functions? Example with `RANK()`:**  
   ```sql
   SELECT name, salary,
          RANK() OVER (ORDER BY salary DESC) AS salary_rank
   FROM employees;
   ```

2. **How do you calculate running totals?**  
   ```sql
   SELECT sale_date, amount,
          SUM(amount) OVER (ORDER BY sale_date) AS running_total
   FROM sales;
   ```

3. **What is a CTE and how is it different from a subquery?**  
   CTEs use `WITH`, are reusable and readable.  
   ```sql
   WITH avg_salaries AS (
     SELECT department_id, AVG(salary) AS avg_sal
     FROM employees GROUP BY department_id
   )
   SELECT * FROM avg_salaries;
   ```

4. **Explain recursive CTEs and give a use case.**  
   Used for hierarchical data (e.g., org charts):  
   ```sql
   WITH RECURSIVE emp_cte AS (
     SELECT id, name, manager_id FROM employees WHERE manager_id IS NULL
     UNION ALL
     SELECT e.id, e.name, e.manager_id
     FROM employees e
     JOIN emp_cte ON e.manager_id = emp_cte.id
   )
   SELECT * FROM emp_cte;
   ```

5. **How do you pivot data in MySQL (simulate PIVOT)?**  
   Use `CASE WHEN`:  
   ```sql
   SELECT
     department_id,
     SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS males,
     SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS females
   FROM employees
   GROUP BY department_id;
   ```

6. **Correlated vs. Non-Correlated Subqueries?**  
   - **Correlated:** references outer query, runs once per row.  
   - **Non-correlated:** runs independently.

7. **How would you optimize a slow query?**  
   - Add indexes  
   - Use `EXPLAIN`  
   - Avoid `SELECT *`  
   - Optimize joins & subqueries

8. **How does indexing affect query performance?**  
   - Speeds up search and joins  
   - Can slow down inserts/updates

9. **How can `EXPLAIN` be used to analyze a query’s performance?**  
   ```sql
   EXPLAIN SELECT * FROM orders WHERE customer_id = 10;
   ```
   It shows table access methods, index usage, estimated rows, etc.

10. **Write a query to return top N records per group.**  
   ```sql
   SELECT * FROM (
     SELECT name, department_id, salary,
            RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rank
     FROM employees
   ) ranked
   WHERE rank <= 3;
   ```

---

