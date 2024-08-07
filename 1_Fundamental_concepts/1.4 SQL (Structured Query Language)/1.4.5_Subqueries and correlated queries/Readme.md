Subqueries and correlated subqueries are advanced SQL concepts used to perform complex queries that require additional filtering or calculation based on the results of other queries. Here’s a detailed explanation of each concept:

### 1. Subqueries

**Definition**: A subquery (or inner query) is a query nested inside another query (the outer query). It can be used to retrieve intermediate results that are then used by the outer query.

**Types**:
- **Single-Row Subquery**: Returns a single row and is typically used with operators like `=`, `<`, `>`, etc.
- **Multiple-Row Subquery**: Returns multiple rows and is used with operators like `IN`, `ANY`, `ALL`.
- **Scalar Subquery**: Returns a single value (one row, one column).

**Examples**:

1. **Single-Row Subquery**: Find employees who earn more than the average salary.

   ```sql
   SELECT name
   FROM Employees
   WHERE salary > (SELECT AVG(salary) FROM Employees);
   ```

   - Here, the subquery `(SELECT AVG(salary) FROM Employees)` calculates the average salary, and the outer query retrieves employees earning more than that average.

2. **Multiple-Row Subquery**: Find employees who work in departments that have more than 10 employees.

   ```sql
   SELECT name
   FROM Employees
   WHERE department_id IN (
       SELECT department_id
       FROM Employees
       GROUP BY department_id
       HAVING COUNT(*) > 10
   );
   ```

   - The subquery `(SELECT department_id FROM Employees GROUP BY department_id HAVING COUNT(*) > 10)` finds department IDs with more than 10 employees. The outer query retrieves employees who belong to those departments.

3. **Scalar Subquery**: Get the name of the employee with the highest salary.

   ```sql
   SELECT name
   FROM Employees
   WHERE salary = (SELECT MAX(salary) FROM Employees);
   ```

   - The subquery `(SELECT MAX(salary) FROM Employees)` finds the highest salary, and the outer query retrieves the employee(s) with that salary.

### 2. Correlated Subqueries

**Definition**: A correlated subquery is a subquery that references columns from the outer query. It is executed once for each row processed by the outer query.

**Characteristics**:
- **Dependent on the Outer Query**: Each row processed by the outer query can affect the result of the subquery.
- **Typically Used with Row-by-Row Comparisons**: Useful for row-by-row comparisons that involve relationships between tables.

**Examples**:

1. **Find Employees Earning More Than the Average Salary in Their Department**:

   ```sql
   SELECT e.name
   FROM Employees e
   WHERE e.salary > (
       SELECT AVG(salary)
       FROM Employees
       WHERE department_id = e.department_id
   );
   ```

   - In this case, the subquery `(SELECT AVG(salary) FROM Employees WHERE department_id = e.department_id)` calculates the average salary for the department of each employee `e` processed by the outer query. The outer query then checks if the employee’s salary is greater than this average.

2. **Find Employees Who Earn More Than Any Employee in a Specific Department**:

   ```sql
   SELECT name
   FROM Employees e1
   WHERE salary > (
       SELECT MAX(salary)
       FROM Employees e2
       WHERE e2.department_id = 1
   );
   ```

   - The subquery `(SELECT MAX(salary) FROM Employees e2 WHERE e2.department_id = 1)` finds the maximum salary in department 1. For each row in the outer query, it checks if the employee’s salary is greater than this maximum salary.

### Summary

- **Subqueries**: Queries nested inside another query, used for filtering or calculations based on the results of the inner query. They can be single-row, multiple-row, or scalar.
- **Correlated Subqueries**: Subqueries that reference columns from the outer query and are executed once for each row processed by the outer query. They are useful for comparisons that involve relationships between rows in different tables.

Both subqueries and correlated subqueries are powerful tools in SQL for performing complex queries and deriving insights from relational data.