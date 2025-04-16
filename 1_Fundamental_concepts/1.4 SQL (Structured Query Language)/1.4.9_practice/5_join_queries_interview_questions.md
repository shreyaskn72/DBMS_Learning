Here's a **complete list of JOIN-related SQL questions with detailed answers** that could be asked in interviews — covering **basic to advanced** concepts. These will test both knowledge **and practical querying skills**.

---

## 🟢 **Basic JOIN Questions (with Answers)**

1. ### **What is a JOIN in SQL?**
   **Answer:**  
   A JOIN is used to combine rows from two or more tables based on a related column between them (typically a foreign key).

2. ### **What is an `INNER JOIN`? Give an example.**
   **Answer:**  
   Returns only the rows with matching keys in both tables.
   ```sql
   SELECT employees.name, departments.name AS department
   FROM employees
   INNER JOIN departments ON employees.department_id = departments.id;
   ```

3. ### **What is a `LEFT JOIN`?**
   **Answer:**  
   Returns all rows from the left table and matched rows from the right table. Unmatched right-side values are `NULL`.
   ```sql
   SELECT customers.name, orders.id
   FROM customers
   LEFT JOIN orders ON customers.id = orders.customer_id;
   ```

4. ### **What is a `RIGHT JOIN`?**
   **Answer:**  
   Returns all rows from the right table and matched rows from the left table. Unmatched left-side values are `NULL`.
   ```sql
   SELECT customers.name, orders.id
   FROM customers
   RIGHT JOIN orders ON customers.id = orders.customer_id;
   ```

5. ### **What is a `FULL OUTER JOIN` and is it supported in MySQL?**
   **Answer:**  
   - Returns all rows from both tables, with `NULL` for missing matches on either side.
   - MySQL doesn't natively support `FULL OUTER JOIN`, but you can simulate it with `UNION`:
   ```sql
   SELECT a.id, b.name
   FROM tableA a
   LEFT JOIN tableB b ON a.id = b.a_id
   UNION
   SELECT a.id, b.name
   FROM tableA a
   RIGHT JOIN tableB b ON a.id = b.a_id;
   ```

6. ### **What is a `CROSS JOIN`?**
   **Answer:**  
   Returns the **Cartesian product** – every combination of rows from both tables.
   ```sql
   SELECT * FROM colors CROSS JOIN sizes;
   ```

---

## 🟡 **Intermediate JOIN Questions (with Answers)**

1. ### **How do you join more than two tables?**
   **Answer:**  
   Just chain `JOIN`s:
   ```sql
   SELECT e.name, d.name AS dept, l.city
   FROM employees e
   JOIN departments d ON e.department_id = d.id
   JOIN locations l ON d.location_id = l.id;
   ```

2. ### **Write a query to get all customers and their latest order (if any).**
   **Answer:**
   ```sql
   SELECT c.name, o.order_date
   FROM customers c
   LEFT JOIN orders o ON c.id = o.customer_id
   WHERE o.order_date = (
     SELECT MAX(order_date)
     FROM orders
     WHERE customer_id = c.id
   );
   ```

3. ### **What’s the difference between `USING` and `ON` in JOINs?**
   **Answer:**
   - `USING` is shorthand when the column names are the same in both tables.
   ```sql
   SELECT * FROM orders JOIN customers USING(customer_id);
   ```
   - `ON` gives more control, especially with different column names.

4. ### **Write a query to find employees who don’t belong to any department.**
   **Answer:**
   ```sql
   SELECT e.*
   FROM employees e
   LEFT JOIN departments d ON e.department_id = d.id
   WHERE d.id IS NULL;
   ```

5. ### **How do you perform a self-join? Give an example.**
   **Answer:**
   A self-join joins a table with itself:
   ```sql
   SELECT a.name AS employee, b.name AS manager
   FROM employees a
   JOIN employees b ON a.manager_id = b.id;
   ```

6. ### **What’s the impact of using `JOIN` vs. `SUBQUERY`?**
   **Answer:**
   - JOINs are often **faster**, especially with indexing.
   - Subqueries can be more **readable** for single-column lookups.
   - Correlated subqueries can be **slower**.

---

## 🔴 **Advanced JOIN Questions (with Answers)**

1. ### **Write a query to get each department and the employee with the highest salary in that department.**
   **Answer:**
   ```sql
   SELECT d.name AS department, e.name AS employee, e.salary
   FROM employees e
   JOIN departments d ON e.department_id = d.id
   WHERE e.salary = (
     SELECT MAX(salary)
     FROM employees
     WHERE department_id = d.id
   );
   ```

2. ### **Write a query to get the number of orders per customer (including customers with zero orders).**
   **Answer:**
   ```sql
   SELECT c.name, COUNT(o.id) AS order_count
   FROM customers c
   LEFT JOIN orders o ON c.id = o.customer_id
   GROUP BY c.id, c.name;
   ```

3. ### **How do you use aggregate functions with JOINs?**
   **Answer:**
   Example – Total order value per customer:
   ```sql
   SELECT c.name, SUM(oi.price * oi.quantity) AS total_spent
   FROM customers c
   JOIN orders o ON c.id = o.customer_id
   JOIN order_items oi ON o.id = oi.order_id
   GROUP BY c.id, c.name;
   ```

4. ### **How do you join tables based on date ranges or non-equal conditions?**
   **Answer:**
   ```sql
   SELECT *
   FROM bookings b
   JOIN rooms r ON b.booking_date BETWEEN r.available_from AND r.available_to;
   ```

5. ### **Write a query to list products that have never been ordered.**
   **Answer:**
   ```sql
   SELECT p.*
   FROM products p
   LEFT JOIN order_items oi ON p.id = oi.product_id
   WHERE oi.product_id IS NULL;
   ```

---

## ✅ BONUS: Real-World Join Challenges

1. **List each employee with their department and location (3-table join).**  
2. **Find the most recent order for every customer.**  
3. **List all products with the number of times they’ve been ordered.**  
4. **Find students who are enrolled in multiple courses.**  
5. **Match each customer to their most purchased product.**

---