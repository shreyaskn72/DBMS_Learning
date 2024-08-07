In Flask-SQLAlchemy, subqueries and correlated subqueries can be implemented using SQLAlchemy’s query constructs. Below are examples of how you can achieve these operations using Flask-SQLAlchemy.

### Setup

Here’s a basic setup for Flask and Flask-SQLAlchemy:

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
    salary = db.Column(db.Float, nullable=False)
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

### 1. Subqueries

**Definition**: A subquery is a query within another query that retrieves intermediate results.

#### Example 1: Single-Row Subquery

**Goal**: Find employees who earn more than the average salary.

```python
@app.route('/high_earners')
def high_earners():
    subquery = db.session.query(db.func.avg(Employee.salary)).scalar_subquery()
    employees = db.session.query(Employee).filter(Employee.salary > subquery).all()
    return '<br>'.join([e.name for e in employees])
```

In this example:
- `db.func.avg(Employee.salary).scalar_subquery()` calculates the average salary and creates a subquery.
- `Employee.salary > subquery` filters employees whose salaries exceed the average.

#### Example 2: Multiple-Row Subquery

**Goal**: Find employees working in departments with more than 10 employees.

```python
@app.route('/large_department_employees')
def large_department_employees():
    subquery = (
        db.session.query(Employee.department_id)
        .group_by(Employee.department_id)
        .having(db.func.count() > 10)
        .subquery()
    )
    employees = db.session.query(Employee).filter(Employee.department_id.in_(subquery)).all()
    return '<br>'.join([e.name for e in employees])
```

In this example:
- The subquery `db.session.query(Employee.department_id).group_by(Employee.department_id).having(db.func.count() > 10).subquery()` finds departments with more than 10 employees.
- The outer query filters employees based on these departments.

#### Example 3: Scalar Subquery

**Goal**: Get the name of the employee with the highest salary.

```python
@app.route('/highest_salary_employee')
def highest_salary_employee():
    subquery = db.session.query(db.func.max(Employee.salary)).scalar_subquery()
    employee = db.session.query(Employee).filter(Employee.salary == subquery).first()
    return f'Highest salary employee: {employee.name}' if employee else 'No employees found'
```

In this example:
- `db.func.max(Employee.salary).scalar_subquery()` finds the maximum salary and creates a subquery.
- The outer query retrieves the employee(s) with this maximum salary.

### 2. Correlated Subqueries

**Definition**: A correlated subquery references columns from the outer query and is executed once for each row processed by the outer query.

#### Example 1: Employees Earning More Than the Average in Their Department

**Goal**: Find employees who earn more than the average salary in their own department.

```python
@app.route('/employees_above_avg_in_department')
def employees_above_avg_in_department():
    subquery = (
        db.session.query(db.func.avg(Employee.salary))
        .filter(Employee.department_id == Employee.department_id)
        .correlate(Employee)
        .scalar_subquery()
    )
    employees = db.session.query(Employee).filter(Employee.salary > subquery).all()
    return '<br>'.join([e.name for e in employees])
```

In this example:
- The subquery `db.session.query(db.func.avg(Employee.salary)).filter(Employee.department_id == Employee.department_id).correlate(Employee).scalar_subquery()` calculates the average salary for each department, correlated with the outer query’s `Employee.department_id`.
- The outer query filters employees earning more than the average salary in their department.

#### Example 2: Employees Who Earn More Than Any Employee in a Specific Department

**Goal**: Find employees who earn more than any employee in department 1.

```python
@app.route('/employees_more_than_any_in_department_1')
def employees_more_than_any_in_department_1():
    subquery = (
        db.session.query(db.func.max(Employee.salary))
        .filter(Employee.department_id == 1)
        .scalar_subquery()
    )
    employees = db.session.query(Employee).filter(Employee.salary > subquery).all()
    return '<br>'.join([e.name for e in employees])
```

In this example:
- The subquery `db.session.query(db.func.max(Employee.salary)).filter(Employee.department_id == 1).scalar_subquery()` finds the highest salary in department 1.
- The outer query retrieves employees earning more than this highest salary.

### Summary

- **Subqueries**: Use SQLAlchemy’s query constructs such as `scalar_subquery()` and `subquery()` to create and use subqueries within your main queries.
- **Correlated Subqueries**: These involve nested queries that depend on the outer query’s columns and can be constructed using `correlate()` to indicate which columns from the outer query are referenced.

Flask-SQLAlchemy provides powerful abstractions for querying and manipulating data, making it easier to handle complex querying scenarios such as subqueries and correlated subqueries.