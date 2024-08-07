Flask-SQLAlchemy, an extension for Flask that integrates SQLAlchemy with Flask applications, allows you to perform various types of joins using Python code. Here's how you can use Flask-SQLAlchemy to perform different types of joins:

### Setup

Ensure you have Flask and Flask-SQLAlchemy installed and set up. Here’s a basic setup example:

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'  # Adjust as needed
db = SQLAlchemy(app)
```

Define models for demonstration:

```python
class Employee(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), nullable=False)
    department_id = db.Column(db.Integer, db.ForeignKey('department.id'))
    department = db.relationship('Department', back_populates='employees')

    def __repr__(self):
        return f'<Employee {self.name}>'

class Department(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), nullable=False)
    employees = db.relationship('Employee', back_populates='department')

    def __repr__(self):
        return f'<Department {self.name}>'
```

### 1. INNER JOIN

**Purpose**: Retrieves records with matching values in both tables.

**Example**: Get employee names along with their department names using an inner join.

```python
@app.route('/inner_join')
def inner_join():
    results = db.session.query(Employee.name, Department.name).join(Department).all()
    return '<br>'.join([f'{emp_name} works in {dept_name}' for emp_name, dept_name in results])
```

In this example, `db.session.query(Employee.name, Department.name).join(Department)` performs an inner join between the `Employee` and `Department` tables based on the relationship defined in the `Employee` model.

### 2. LEFT JOIN (or LEFT OUTER JOIN)

**Purpose**: Retrieves all records from the left table and the matched records from the right table. If no match is found, `NULL` values are returned for columns from the right table.

**Example**: Get all employees and their departments, including employees without a department.

```python
@app.route('/left_join')
def left_join():
    results = db.session.query(Employee.name, Department.name).outerjoin(Department).all()
    return '<br>'.join([f'{emp_name} works in {dept_name if dept_name else "No Department"}' for emp_name, dept_name in results])
```

In this example, `db.session.query(Employee.name, Department.name).outerjoin(Department)` performs a left join between `Employee` and `Department`, including all employees even if they don’t have a corresponding department.

### 3. RIGHT JOIN (or RIGHT OUTER JOIN)

**Purpose**: Retrieves all records from the right table and the matched records from the left table. If no match is found, `NULL` values are returned for columns from the left table.

**Example**: Get all departments and their employees, including departments with no employees.

```python
@app.route('/right_join')
def right_join():
    # SQLAlchemy does not have a direct right join method,
    # so you need to manually simulate it using outer joins and filtering
    results = db.session.query(Department.name, Employee.name).outerjoin(Employee).all()
    return '<br>'.join([f'{dept_name} has employee {emp_name if emp_name else "No Employees"}' for dept_name, emp_name in results])
```

SQLAlchemy does not have a direct `RIGHT JOIN` method. To simulate it, you can use an outer join and filter as needed. The above example shows how to achieve a similar result.

### 4. FULL JOIN (or FULL OUTER JOIN)

**Purpose**: Retrieves all records when there is a match in either table. If there is no match, `NULL` values are returned for columns from the table without a match.

**Example**: Get a complete list of employees and departments, including those that don’t have a corresponding entry in the other table.

```python
@app.route('/full_join')
def full_join():
    # SQLAlchemy does not have a direct full outer join method,
    # so you need to simulate it using union of left and right joins
    employees_with_departments = db.session.query(Employee.name, Department.name).outerjoin(Department).all()
    departments_with_employees = db.session.query(Department.name, Employee.name).outerjoin(Employee).all()
    
    # Combine results from both queries
    combined_results = set(employees_with_departments + departments_with_employees)
    
    return '<br>'.join([f'{emp_name if emp_name else "No Employee"} works in {dept_name if dept_name else "No Department"}' for emp_name, dept_name in combined_results])
```

Similar to `RIGHT JOIN`, SQLAlchemy does not have a direct `FULL JOIN` method. To simulate a full outer join, you can combine results from both left and right outer joins using a union.

### Summary

- **INNER JOIN**: Use `.join()` to get matching records from both tables.
- **LEFT JOIN**: Use `.outerjoin()` to include all records from the left table and matched records from the right table.
- **RIGHT JOIN**: Simulate using `.outerjoin()` and filtering, as SQLAlchemy lacks a direct method.
- **FULL JOIN**: Simulate using a union of left and right outer joins, as SQLAlchemy lacks a direct method.

These techniques help you retrieve and combine data from multiple tables using Flask-SQLAlchemy, enabling complex queries and data analysis in your Flask applications.