Let's explore another detailed example that showcases Boyce-Codd Normal Form (BCNF) in a relational database, complete with primary keys for each table.

### Scenario: Employee Project Assignment

#### Initial Table: EmployeeProject

Suppose we have a table that captures information about employees, their projects, and the departments they belong to:

**Table: EmployeeProject**
```
| EmployeeID | ProjectID | Department   |
|------------|-----------|--------------|
| 1          | 101       | HR           |
| 2          | 102       | IT           |
| 1          | 103       | HR           |
| 3          | 101       | HR           |
| 2          | 104       | IT           |
```

#### Functional Dependencies
1. \( \text{EmployeeID, ProjectID} \rightarrow \text{Department} \) (Each employee on a specific project belongs to one department.)
2. \( \text{EmployeeID} \rightarrow \text{Department} \) (Each employee belongs to one department, independent of the project.)

#### Analysis of BCNF
- **Candidate Keys**: The composite key \( (\text{EmployeeID, ProjectID}) \) is a candidate key.
- The functional dependency \( \text{EmployeeID} \rightarrow \text{Department} \) violates BCNF because \( \text{EmployeeID} \) is not a superkey; it does not uniquely identify a tuple in the table.

### Decomposing to Achieve BCNF

To convert this table into BCNF, we need to decompose it into two separate tables based on the functional dependencies:

**1. Create the Employee Table**
This table will capture each employee's department.

**Table: Employee**
```
| EmployeeID | Department   |
|------------|--------------|
| 1          | HR           |
| 2          | IT           |
| 3          | HR           |
```
- **Primary Key**: `EmployeeID`

**2. Create the ProjectAssignment Table**
This table will capture the projects assigned to each employee.

**Table: ProjectAssignment**
```
| EmployeeID | ProjectID |
|------------|-----------|
| 1          | 101       |
| 1          | 103       |
| 2          | 102       |
| 2          | 104       |
| 3          | 101       |
```
- **Primary Key**: Composite Key `(EmployeeID, ProjectID)`

### Benefits of Decomposition

1. **Elimination of Redundancy**: Employee departments are stored in one place, avoiding duplication of department information across multiple rows.
2. **Improved Data Integrity**: Changes to department information (e.g., if an employee changes departments) can be made in the `Employee` table without affecting the `ProjectAssignment` table.
3. **Simplified Maintenance**: Each table serves a distinct purpose, making updates and queries more straightforward.

### Summary

- **Before BCNF**: The `EmployeeProject` table had a functional dependency that violated BCNF because not all determinants were superkeys.
- **After BCNF**: The database was decomposed into two tables (`Employee` and `ProjectAssignment`), eliminating redundancy and ensuring that all functional dependencies comply with BCNF rules.

This example illustrates how to effectively structure a database to meet BCNF requirements, leading to improved data integrity and maintainability.