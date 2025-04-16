 Here’s a **new batch of SQL queries** (with increasing complexity) that you can use to **test an individual's practical SQL skills**. These cover a variety of topics including `JOIN`, `GROUP BY`, subqueries, window functions, `CASE`, `DATE`, and more.

---

## 🟢 **Basic SQL Query Challenges**

1. **Select names of all students older than 18 from a `students` table.**
   ```sql
   SELECT name FROM students WHERE age > 18;
   ```

2. **Find all products with a price between 100 and 500 from `products` table.**
   ```sql
   SELECT * FROM products WHERE price BETWEEN 100 AND 500;
   ```

3. **List all customers whose names start with 'A'.**
   ```sql
   SELECT * FROM customers WHERE name LIKE 'A%';
   ```

4. **Count the number of orders placed.**
   ```sql
   SELECT COUNT(*) FROM orders;
   ```

---

## 🟡 **Intermediate SQL Query Challenges**

1. **Get a list of customers who have placed more than 5 orders.**
   ```sql
   SELECT customer_id, COUNT(*) AS order_count
   FROM orders
   GROUP BY customer_id
   HAVING COUNT(*) > 5;
   ```

2. **Get the total revenue per product from `order_items` (assume: quantity * price).**
   ```sql
   SELECT product_id, SUM(quantity * price) AS total_revenue
   FROM order_items
   GROUP BY product_id;
   ```

3. **Find customers who haven’t placed any orders.**
   ```sql
   SELECT * FROM customers
   WHERE id NOT IN (SELECT customer_id FROM orders);
   ```

4. **Show all employees who report to 'John Doe' (assuming `manager_id` is a self-reference in `employees`).**
   ```sql
   SELECT e.*
   FROM employees e
   JOIN employees m ON e.manager_id = m.id
   WHERE m.name = 'John Doe';
   ```

5. **Get top 3 highest salaries from `employees`.**
   ```sql
   SELECT DISTINCT salary
   FROM employees
   ORDER BY salary DESC
   LIMIT 3;
   ```

---

## 🔴 **Advanced SQL Query Challenges**

1. **Find the highest salary in each department.**
   ```sql
   SELECT department_id, MAX(salary) AS max_salary
   FROM employees
   GROUP BY department_id;
   ```

2. **Use `RANK()` to find top 3 earners in each department.**
   ```sql
   SELECT *
   FROM (
     SELECT name, department_id, salary,
            RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rank
     FROM employees
   ) ranked
   WHERE rank <= 3;
   ```

3. **Calculate the running total of sales by date.**
   ```sql
   SELECT sale_date, amount,
          SUM(amount) OVER (ORDER BY sale_date) AS running_total
   FROM sales;
   ```

4. **Use a `CASE` statement to classify products by price range.**
   ```sql
   SELECT name, price,
          CASE
            WHEN price < 100 THEN 'Cheap'
            WHEN price BETWEEN 100 AND 500 THEN 'Moderate'
            ELSE 'Expensive'
          END AS price_category
   FROM products;
   ```

5. **Find the average number of items per order using a subquery.**
   ```sql
   SELECT AVG(items_per_order) AS avg_items
   FROM (
     SELECT order_id, COUNT(*) AS items_per_order
     FROM order_items
     GROUP BY order_id
   ) AS order_stats;
   ```

6. **Get all customers and their last order date (even those who never ordered).**
   ```sql
   SELECT c.id, c.name, MAX(o.order_date) AS last_order_date
   FROM customers c
   LEFT JOIN orders o ON c.id = o.customer_id
   GROUP BY c.id, c.name;
   ```

7. **Find products that have never been ordered.**
   ```sql
   SELECT * FROM products
   WHERE id NOT IN (
     SELECT DISTINCT product_id FROM order_items
   );
   ```

---