Joins in SQL are used to combine rows from two or more tables based on a related column between them. They help in querying data that is spread across multiple tables in a relational database. Here’s an overview of different types of joins:

### 1. INNER JOIN

**Purpose**: Retrieves records that have matching values in both tables.

**Syntax**:
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.common_column = table2.common_column;
```

**Example**: Consider two tables, `Employees` and `Departments`:

- **Employees**:
  - `EmployeeID`
  - `Name`
  - `DepartmentID`

- **Departments**:
  - `DepartmentID`
  - `DepartmentName`

To get the names of employees along with their department names:

```sql
SELECT Employees.Name, Departments.DepartmentName
FROM Employees
INNER JOIN Departments
ON Employees.DepartmentID = Departments.DepartmentID;
```

This query will return only those employees who have a matching department in the `Departments` table.

### 2. LEFT JOIN (or LEFT OUTER JOIN)

**Purpose**: Retrieves all records from the left table and the matched records from the right table. If no match is found, `NULL` values are returned for columns from the right table.

**Syntax**:
```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.common_column = table2.common_column;
```

**Example**: To get all employees and their departments, including employees who are not assigned to any department:

```sql
SELECT Employees.Name, Departments.DepartmentName
FROM Employees
LEFT JOIN Departments
ON Employees.DepartmentID = Departments.DepartmentID;
```

This query will return all employees, and for those who do not have a matching department, `NULL` will be displayed in the `DepartmentName` column.

### 3. RIGHT JOIN (or RIGHT OUTER JOIN)

**Purpose**: Retrieves all records from the right table and the matched records from the left table. If no match is found, `NULL` values are returned for columns from the left table.

**Syntax**:
```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.common_column = table2.common_column;
```

**Example**: To get all departments and the employees assigned to them, including departments with no employees:

```sql
SELECT Employees.Name, Departments.DepartmentName
FROM Employees
RIGHT JOIN Departments
ON Employees.DepartmentID = Departments.DepartmentID;
```

This query will return all departments, and for those departments with no employees, `NULL` will be displayed in the `Name` column.

### 4. FULL JOIN (or FULL OUTER JOIN)

**Purpose**: Retrieves all records when there is a match in either left or right table. If there is no match, `NULL` values are returned for columns from the table without a match.

**Syntax**:
```sql
SELECT columns
FROM table1
FULL JOIN table2
ON table1.common_column = table2.common_column;
```

**Example**: To get a complete list of employees and departments, including those that don’t have a corresponding entry in the other table:

```sql
SELECT Employees.Name, Departments.DepartmentName
FROM Employees
FULL JOIN Departments
ON Employees.DepartmentID = Departments.DepartmentID;
```

This query will return all employees and all departments. For employees without departments and departments without employees, `NULL` values will be displayed where there is no match.

### Summary

- **INNER JOIN**: Retrieves records with matching values in both tables.
- **LEFT JOIN**: Retrieves all records from the left table and matching records from the right table, with `NULL` where no match is found.
- **RIGHT JOIN**: Retrieves all records from the right table and matching records from the left table, with `NULL` where no match is found.
- **FULL JOIN**: Retrieves all records when there is a match in either table, with `NULL` for missing matches.

Joins are powerful tools in SQL for combining data across multiple tables based on relationships and keys. They help in creating complex queries and generating comprehensive reports from relational databases.