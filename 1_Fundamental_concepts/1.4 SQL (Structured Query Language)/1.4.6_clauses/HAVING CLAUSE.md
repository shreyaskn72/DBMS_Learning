The `HAVING` clause is used in SQL to filter groups created by the `GROUP BY` clause based on aggregate functions. It is particularly useful when you want to apply conditions on the results of aggregate functions like `COUNT()`, `SUM()`, `AVG()`, `MAX()`, or `MIN()`. Here are several more examples of queries using the `HAVING` clause:

---

### 1. **Find departments where the total salary exceeds $1 million.**

```sql
SELECT department_id, SUM(salary) AS total_salary
FROM employees
GROUP BY department_id
HAVING SUM(salary) > 1000000;
```
This query groups employees by `department_id` and filters those departments where the total salary exceeds 1 million.

---

### 2. **Find products with more than 100 units sold.**

```sql
SELECT product_id, SUM(quantity) AS total_sold
FROM order_items
GROUP BY product_id
HAVING SUM(quantity) > 100;
```
Here, the query groups the order items by `product_id`, sums up the quantities sold, and returns only those products where the total quantity sold is greater than 100.

---

### 3. **Find employees who have been with the company for more than 10 years, based on their hire date.**

```sql
SELECT department_id, COUNT(employee_id) AS num_employees
FROM employees
WHERE hire_date < CURRENT_DATE - INTERVAL '10 years'
GROUP BY department_id
HAVING COUNT(employee_id) > 5;
```
This query groups employees by department and counts the number of employees who have been with the company for more than 10 years, then filters those departments where there are more than 5 such employees.

---

### 4. **Find customers who have spent more than $5000 on orders.**

```sql
SELECT customer_id, SUM(order_amount) AS total_spent
FROM orders
GROUP BY customer_id
HAVING SUM(order_amount) > 5000;
```
This query groups orders by `customer_id`, sums up the total amount spent by each customer, and filters those customers who have spent more than $5000.

---

### 5. **Find products with an average price greater than $50.**

```sql
SELECT product_id, AVG(price) AS average_price
FROM products
GROUP BY product_id
HAVING AVG(price) > 50;
```
Here, the query calculates the average price for each product and returns only those products where the average price exceeds $50.

---

### 6. **Find the departments where the number of employees is less than 3.**

```sql
SELECT department_id, COUNT(employee_id) AS num_employees
FROM employees
GROUP BY department_id
HAVING COUNT(employee_id) < 3;
```
This query groups employees by `department_id` and counts how many employees are in each department, filtering the departments where fewer than 3 employees exist.

---

### 7. **Find employees whose salary is higher than the average salary in their department.**

```sql
SELECT department_id, employee_id, name, salary
FROM employees e1
GROUP BY department_id, employee_id, name, salary
HAVING salary > (SELECT AVG(salary) FROM employees e2 WHERE e2.department_id = e1.department_id);
```
This query groups employees by department, calculates their salary, and filters those whose salary is greater than the average salary in their respective departments using a correlated subquery.

---

### 8. **Find the top 3 products with the highest sales, where sales exceed $1000.**

```sql
SELECT product_id, SUM(sales_amount) AS total_sales
FROM sales
GROUP BY product_id
HAVING SUM(sales_amount) > 1000
ORDER BY total_sales DESC
LIMIT 3;
```
This query sums the sales amount for each product and filters those with sales greater than $1000. It then orders the products by total sales in descending order and limits the result to the top 3.

---

### 9. **Find the number of employees with salaries above $60,000 in each department.**

```sql
SELECT department_id, COUNT(employee_id) AS num_employees
FROM employees
WHERE salary > 60000
GROUP BY department_id
HAVING COUNT(employee_id) > 2;
```
This query first filters employees with salaries greater than $60,000 and then groups them by department, returning only departments that have more than 2 such employees.

---

### 10. **Find customers who have placed at least 5 orders, and the total number of orders placed by them.**

```sql
SELECT customer_id, COUNT(order_id) AS num_orders
FROM orders
GROUP BY customer_id
HAVING COUNT(order_id) >= 5;
```
This query counts the number of orders placed by each customer and filters those customers who have placed 5 or more orders.

---

### 11. **Find employees whose salary is in the top 10% of their department.**

```sql
SELECT department_id, employee_id, name, salary
FROM employees e1
WHERE salary > (
    SELECT PERCENTILE_CONT(0.9) WITHIN GROUP (ORDER BY salary) 
    FROM employees e2
    WHERE e2.department_id = e1.department_id
)
GROUP BY department_id, employee_id, name, salary;
```
This query uses the `PERCENTILE_CONT()` window function to calculate the 90th percentile (top 10%) of salaries for each department, and filters employees whose salary is above that percentile.

---

### 12. **Find orders where the total order value is greater than $10,000.**

```sql
SELECT order_id, SUM(order_amount) AS total_order_value
FROM order_items
GROUP BY order_id
HAVING SUM(order_amount) > 10000;
```
This query sums the `order_amount` for each order and filters those orders where the total value exceeds $10,000.

---

### 13. **Find employees who have received more than 3 promotions.**

```sql
SELECT employee_id, COUNT(promotion_id) AS num_promotions
FROM promotions
GROUP BY employee_id
HAVING COUNT(promotion_id) > 3;
```
This query counts the number of promotions each employee has received and filters those who have received more than 3 promotions.

---

### 14. **Find the departments with an average salary less than $40,000, but where at least 10 employees have a salary greater than $60,000.**

```sql
SELECT department_id, AVG(salary) AS avg_salary, COUNT(employee_id) AS num_employees
FROM employees
GROUP BY department_id
HAVING AVG(salary) < 40000 AND COUNT(CASE WHEN salary > 60000 THEN 1 END) >= 10;
```
This query calculates the average salary and counts the number of employees with salaries greater than $60,000 in each department, filtering for departments where the average salary is below $40,000 and at least 10 employees have salaries above $60,000.

---

### 15. **Find the years with total sales greater than $50,000.**

```sql
SELECT EXTRACT(YEAR FROM sale_date) AS sale_year, SUM(sales_amount) AS total_sales
FROM sales
GROUP BY sale_year
HAVING SUM(sales_amount) > 50000;
```
This query groups sales by year, sums the sales amount for each year, and filters the years where total sales exceed $50,000.

---

### 16. **Find customers who have returned more than 10% of their orders.**

```sql
SELECT customer_id, COUNT(order_id) AS total_orders, 
       SUM(CASE WHEN returned = TRUE THEN 1 ELSE 0 END) AS returned_orders
FROM orders
GROUP BY customer_id
HAVING SUM(CASE WHEN returned = TRUE THEN 1 ELSE 0 END) / COUNT(order_id) > 0.1;
```
This query calculates the percentage of orders that were returned by each customer and filters those who have returned more than 10% of their orders.

---

These examples show how the `HAVING` clause can be used in various scenarios to filter aggregated data. It’s particularly useful when dealing with complex conditions on grouped data, especially when `WHERE` cannot be used (e.g., with aggregates).