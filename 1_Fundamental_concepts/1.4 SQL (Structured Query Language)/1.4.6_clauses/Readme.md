The `GROUP BY` clause in SQL is used to group rows that have the same values in specified columns into aggregated data, such as sums, averages, counts, etc. The `HAVING` clause is often used in conjunction with `GROUP BY` to filter results after aggregation.

### 1. **Basic `GROUP BY` Example**

#### Example 1: Count the number of employees in each department.

```sql
SELECT department_id, COUNT(employee_id) AS num_employees
FROM employees
GROUP BY department_id;
```
This query groups the employees by `department_id` and counts the number of employees in each department.

### 2. **Using `GROUP BY` with Aggregates**

#### Example 2: Calculate the total salary per department.

```sql
SELECT department_id, SUM(salary) AS total_salary
FROM employees
GROUP BY department_id;
```
This query calculates the total salary for each department by grouping employees by their `department_id`.

#### Example 3: Find the average salary in each department.

```sql
SELECT department_id, AVG(salary) AS average_salary
FROM employees
GROUP BY department_id;
```
This query calculates the average salary in each department.

#### Example 4: Find the highest and lowest salary in each department.

```sql
SELECT department_id, MAX(salary) AS highest_salary, MIN(salary) AS lowest_salary
FROM employees
GROUP BY department_id;
```
This query groups employees by `department_id` and calculates the maximum and minimum salaries within each department.

### 3. **Using `GROUP BY` with Multiple Columns**

#### Example 5: Count the number of orders per product and order status.

```sql
SELECT product_id, order_status, COUNT(order_id) AS num_orders
FROM orders
GROUP BY product_id, order_status;
```
This query groups the results first by `product_id` and then by `order_status`, counting the number of orders for each combination.

### 4. **Using `HAVING` to Filter Groups**

The `HAVING` clause is used to filter groups based on aggregate functions, whereas `WHERE` is used to filter rows before aggregation.

#### Example 6: Find departments with more than 10 employees.

```sql
SELECT department_id, COUNT(employee_id) AS num_employees
FROM employees
GROUP BY department_id
HAVING COUNT(employee_id) > 10;
```
This query filters the groups to only include departments that have more than 10 employees after the aggregation.

#### Example 7: Find products with total sales greater than 5000.

```sql
SELECT product_id, SUM(sales_amount) AS total_sales
FROM sales
GROUP BY product_id
HAVING SUM(sales_amount) > 5000;
```
This query calculates the total sales for each product and filters out products with total sales less than 5000.

#### Example 8: Find departments with an average salary greater than 60,000.

```sql
SELECT department_id, AVG(salary) AS average_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 60000;
```
This query groups employees by `department_id`, calculates the average salary, and filters to only include departments where the average salary is greater than 60,000.

#### Example 9: Find employees whose salary is higher than the average salary in their department.

```sql
SELECT department_id, employee_id, name, salary
FROM employees
GROUP BY department_id, employee_id, name, salary
HAVING salary > (SELECT AVG(salary) FROM employees WHERE department_id = employees.department_id);
```
This query groups employees by their `department_id` and filters out employees whose salary is not higher than the average salary within their respective departments.

### 5. **Using `HAVING` with Multiple Aggregates**

#### Example 10: Find departments with both an average salary greater than 50,000 and more than 5 employees.

```sql
SELECT department_id, AVG(salary) AS avg_salary, COUNT(employee_id) AS num_employees
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 50000 AND COUNT(employee_id) > 5;
```
This query finds departments where the average salary is above 50,000 and the number of employees is greater than 5.

### 6. **Using `GROUP BY` with `ORDER BY`**

You can use `ORDER BY` in combination with `GROUP BY` to sort the results after aggregation.

#### Example 11: Find the total sales per product, ordered by total sales in descending order.

```sql
SELECT product_id, SUM(sales_amount) AS total_sales
FROM sales
GROUP BY product_id
ORDER BY total_sales DESC;
```
This query groups sales by `product_id`, sums up the `sales_amount`, and orders the results by total sales in descending order.

#### Example 12: Find the number of orders per customer, ordered by the number of orders.

```sql
SELECT customer_id, COUNT(order_id) AS num_orders
FROM orders
GROUP BY customer_id
ORDER BY num_orders DESC;
```
This query counts the number of orders per customer and sorts the results by the number of orders in descending order.

### 7. **Combining `GROUP BY`, `HAVING`, and `ORDER BY`**

You can combine all three clauses to create powerful queries.

#### Example 13: Find departments with more than 3 employees, whose average salary is greater than 50,000, and order the results by the average salary in descending order.

```sql
SELECT department_id, AVG(salary) AS avg_salary, COUNT(employee_id) AS num_employees
FROM employees
GROUP BY department_id
HAVING COUNT(employee_id) > 3 AND AVG(salary) > 50000
ORDER BY avg_salary DESC;
```
This query groups employees by department, calculates the average salary, filters departments with more than 3 employees and an average salary above 50,000, and sorts the results by the average salary in descending order.

### 8. **`GROUP BY` with `DISTINCT`**

You can use `DISTINCT` within a `GROUP BY` query to return only unique values.

#### Example 14: Find the unique products ordered by each customer.

```sql
SELECT customer_id, COUNT(DISTINCT product_id) AS num_unique_products
FROM orders
GROUP BY customer_id;
```
This query counts the number of unique products ordered by each customer.

### 9. **`GROUP BY` with `ROLLUP` and `CUBE` for Subtotal/Grand Total**

#### Example 15: Use `ROLLUP` to get subtotals and grand total.

```sql
SELECT department_id, SUM(salary) AS total_salary
FROM employees
GROUP BY department_id WITH ROLLUP;
```
This query groups employees by `department_id`, calculates the total salary, and adds a grand total row at the end using `WITH ROLLUP`.

#### Example 16: Use `CUBE` to get subtotals for all combinations of department and salary range.

```sql
SELECT department_id, salary_range, COUNT(employee_id) AS num_employees
FROM (SELECT department_id,
             CASE 
                 WHEN salary < 50000 THEN 'Low'
                 WHEN salary BETWEEN 50000 AND 100000 THEN 'Medium'
                 ELSE 'High'
             END AS salary_range, 
             employee_id
      FROM employees) AS employee_salaries
GROUP BY department_id, salary_range WITH CUBE;
```
This query groups by both `department_id` and salary ranges, providing subtotals and grand totals for every combination using `WITH CUBE`.

---

These examples illustrate how the `GROUP BY` and `HAVING` clauses can be used for aggregating data and filtering it based on the results of those aggregations.