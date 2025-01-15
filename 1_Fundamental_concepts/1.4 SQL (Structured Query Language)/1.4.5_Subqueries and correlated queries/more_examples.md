Subqueries in SQL are queries nested inside another query, typically in the `WHERE`, `FROM`, or `SELECT` clause. Below are several examples of subqueries in different contexts:

### 1. **Subquery in the `WHERE` Clause**
   A common use of subqueries is in the `WHERE` clause to filter results based on the result of another query.

#### Example 1: Find employees who have a salary higher than the average salary.

```sql
SELECT employee_id, name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```
In this query, the subquery `(SELECT AVG(salary) FROM employees)` calculates the average salary, and the outer query returns employees with salaries above that average.

#### Example 2: Find customers who have placed orders.

```sql
SELECT customer_id, name
FROM customers
WHERE customer_id IN (SELECT customer_id FROM orders);
```
Here, the subquery retrieves the `customer_id`s from the `orders` table, and the outer query finds customers whose IDs appear in that result.

### 2. **Subquery in the `FROM` Clause**
   Subqueries can be used in the `FROM` clause to treat the result of the subquery as a temporary table or derived table.

#### Example 3: List the total sales for each product.

```sql
SELECT product_id, SUM(sales_amount) AS total_sales
FROM (SELECT product_id, sales_amount FROM sales WHERE sale_date BETWEEN '2024-01-01' AND '2024-12-31') AS yearly_sales
GROUP BY product_id;
```
The subquery selects sales data for 2024, and the outer query calculates the total sales for each product.

### 3. **Subquery in the `SELECT` Clause**
   A subquery can also be used in the `SELECT` clause to return a value for each row.

#### Example 4: Find employees and their department’s average salary.

```sql
SELECT employee_id, name, salary,
       (SELECT AVG(salary) FROM employees e2 WHERE e2.department_id = e1.department_id) AS dept_avg_salary
FROM employees e1;
```
In this query, the subquery calculates the average salary of each department and displays it alongside each employee's salary.

### 4. **Correlated Subquery**
   A correlated subquery references columns from the outer query and is executed once for each row of the outer query.

#### Example 5: Find employees whose salary is higher than the average salary in their department.

```sql
SELECT employee_id, name, salary, department_id
FROM employees e1
WHERE salary > (SELECT AVG(salary) FROM employees e2 WHERE e2.department_id = e1.department_id);
```
This correlated subquery compares each employee's salary to the average salary in the same department.

### 5. **Subquery with `EXISTS`**
   The `EXISTS` keyword is used to check if a subquery returns any rows.

#### Example 6: Find products that have been ordered at least once.

```sql
SELECT product_id, product_name
FROM products p
WHERE EXISTS (SELECT 1 FROM order_items oi WHERE oi.product_id = p.product_id);
```
Here, the `EXISTS` subquery checks if there is any entry in `order_items` that matches each product. If it does, that product is included in the results.

### 6. **Subquery with `NOT EXISTS`**
   The `NOT EXISTS` is used to find rows where no matching rows exist in the subquery.

#### Example 7: Find customers who have never placed an order.

```sql
SELECT customer_id, customer_name
FROM customers c
WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);
```
This subquery checks for customers who do not have any corresponding entries in the `orders` table.

### 7. **Subquery with `IN`**
   The `IN` keyword is used to filter results based on whether a column's value exists in a list of values returned by the subquery.

#### Example 8: Find employees working in departments located in 'New York'.

```sql
SELECT employee_id, name, department_id
FROM employees
WHERE department_id IN (SELECT department_id FROM departments WHERE location = 'New York');
```
The subquery selects the department IDs located in 'New York', and the outer query retrieves employees in those departments.

### 8. **Subquery with `ANY` or `ALL`**
   The `ANY` or `ALL` keyword allows comparison to a set of values returned by a subquery.

#### Example 9: Find employees with a salary higher than the highest salary of any employee in the 'HR' department.

```sql
SELECT employee_id, name, salary
FROM employees
WHERE salary > ANY (SELECT salary FROM employees WHERE department_id = (SELECT department_id FROM departments WHERE department_name = 'HR'));
```
This query compares each employee’s salary with the salaries of employees in the HR department, returning those with higher salaries.

#### Example 10: Find employees with a salary greater than or equal to all salaries in the 'Sales' department.

```sql
SELECT employee_id, name, salary
FROM employees
WHERE salary >= ALL (SELECT salary FROM employees WHERE department_id = (SELECT department_id FROM departments WHERE department_name = 'Sales'));
```
The `ALL` keyword ensures that an employee's salary is greater than or equal to the highest salary in the 'Sales' department.

---

These examples show how subqueries can be used in various parts of a SQL statement to achieve different kinds of filtering, aggregation, and data retrieval.