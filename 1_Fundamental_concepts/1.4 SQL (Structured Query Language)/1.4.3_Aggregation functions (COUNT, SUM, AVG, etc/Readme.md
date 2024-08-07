Aggregation functions in SQL are used to perform calculations on a set of values and return a single result. These functions are commonly used with the `GROUP BY` clause to aggregate data into summary results. Here’s an overview of some common aggregation functions:

### 1. `COUNT`

- **Purpose**: Returns the number of rows that match a specified condition.
- **Syntax**:
  ```sql
  SELECT COUNT(column_name)
  FROM table_name
  WHERE condition;
  ```
- **Example**:
  ```sql
  SELECT COUNT(*) AS num_employees
  FROM Employees;
  ```
  This query returns the total number of employees in the `Employees` table.

- **With `GROUP BY`**:
  ```sql
  SELECT Department, COUNT(*)
  FROM Employees
  GROUP BY Department;
  ```
  This query returns the number of employees in each department.

### 2. `SUM`

- **Purpose**: Returns the total sum of a numeric column.
- **Syntax**:
  ```sql
  SELECT SUM(column_name)
  FROM table_name
  WHERE condition;
  ```
- **Example**:
  ```sql
  SELECT SUM(Salary) AS total_salary
  FROM Employees;
  ```
  This query returns the total sum of salaries for all employees.

- **With `GROUP BY`**:
  ```sql
  SELECT Department, SUM(Salary)
  FROM Employees
  GROUP BY Department;
  ```
  This query returns the total salary expense for each department.

### 3. `AVG`

- **Purpose**: Returns the average value of a numeric column.
- **Syntax**:
  ```sql
  SELECT AVG(column_name)
  FROM table_name
  WHERE condition;
  ```
- **Example**:
  ```sql
  SELECT AVG(Salary) AS average_salary
  FROM Employees;
  ```
  This query returns the average salary of all employees.

- **With `GROUP BY`**:
  ```sql
  SELECT Department, AVG(Salary)
  FROM Employees
  GROUP BY Department;
  ```
  This query returns the average salary for each department.

### 4. `MAX`

- **Purpose**: Returns the maximum value in a set of values.
- **Syntax**:
  ```sql
  SELECT MAX(column_name)
  FROM table_name
  WHERE condition;
  ```
- **Example**:
  ```sql
  SELECT MAX(Salary) AS highest_salary
  FROM Employees;
  ```
  This query returns the highest salary among all employees.

- **With `GROUP BY`**:
  ```sql
  SELECT Department, MAX(Salary)
  FROM Employees
  GROUP BY Department;
  ```
  This query returns the highest salary in each department.

### 5. `MIN`

- **Purpose**: Returns the minimum value in a set of values.
- **Syntax**:
  ```sql
  SELECT MIN(column_name)
  FROM table_name
  WHERE condition;
  ```
- **Example**:
  ```sql
  SELECT MIN(Salary) AS lowest_salary
  FROM Employees;
  ```
  This query returns the lowest salary among all employees.

- **With `GROUP BY`**:
  ```sql
  SELECT Department, MIN(Salary)
  FROM Employees
  GROUP BY Department;
  ```
  This query returns the lowest salary in each department.

### 6. `GROUP_CONCAT` (MySQL) / `STRING_AGG` (PostgreSQL) / `LISTAGG` (Oracle)

- **Purpose**: Concatenates values from multiple rows into a single string.
- **Syntax**:
  - **MySQL**:
    ```sql
    SELECT Department, GROUP_CONCAT(FirstName)
    FROM Employees
    GROUP BY Department;
    ```
  - **PostgreSQL**:
    ```sql
    SELECT Department, STRING_AGG(FirstName, ', ')
    FROM Employees
    GROUP BY Department;
    ```
  - **Oracle**:
    ```sql
    SELECT Department, LISTAGG(FirstName, ', ') WITHIN GROUP (ORDER BY FirstName)
    FROM Employees
    GROUP BY Department;
    ```
  These queries return a concatenated list of employee names for each department.

### Summary

- **`COUNT`**: Counts the number of rows.
- **`SUM`**: Calculates the total sum of a numeric column.
- **`AVG`**: Calculates the average of a numeric column.
- **`MAX`**: Finds the maximum value in a column.
- **`MIN`**: Finds the minimum value in a column.
- **`GROUP_CONCAT` / `STRING_AGG` / `LISTAGG`**: Concatenates values into a single string.

These aggregation functions help summarize and analyze data efficiently by providing insights into overall metrics and patterns within your datasets.