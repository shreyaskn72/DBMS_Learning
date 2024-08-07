Flask-SQLAlchemy integrates SQLAlchemy with Flask, allowing you to perform data aggregation operations using Python code. Below, I’ll explain how to use aggregation functions (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`, etc.) with Flask-SQLAlchemy.

### Setup

First, ensure you have Flask and Flask-SQLAlchemy installed and set up. Here’s an example of how to set up a Flask application with Flask-SQLAlchemy:

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'  # Adjust as needed
db = SQLAlchemy(app)
```

Define a model for demonstration:

```python
from datetime import date

class Employee(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    first_name = db.Column(db.String(50), nullable=False)
    last_name = db.Column(db.String(50), nullable=False)
    department = db.Column(db.String(50))
    salary = db.Column(db.Float)
    hire_date = db.Column(db.Date)

    def __repr__(self):
        return f'<Employee {self.first_name} {self.last_name}>'
```

### Aggregation Functions with Flask-SQLAlchemy

#### 1. `COUNT`

**Purpose**: Count the number of rows.

**Example**: Count the total number of employees.

```python
@app.route('/total_employees')
def total_employees():
    count = db.session.query(db.func.count(Employee.id)).scalar()
    return f'Total number of employees: {count}'
```

**Example**: Count the number of employees in each department.

```python
@app.route('/employee_count_by_department')
def employee_count_by_department():
    counts = db.session.query(
        Employee.department,
        db.func.count(Employee.id)
    ).group_by(Employee.department).all()

    result = '<br>'.join([f'{dept}: {count}' for dept, count in counts])
    return result
```

#### 2. `SUM`

**Purpose**: Calculate the total sum of a numeric column.

**Example**: Calculate the total salary of all employees.

```python
@app.route('/total_salary')
def total_salary():
    total = db.session.query(db.func.sum(Employee.salary)).scalar()
    return f'Total salary of all employees: {total}'
```

**Example**: Calculate the total salary by department.

```python
@app.route('/total_salary_by_department')
def total_salary_by_department():
    totals = db.session.query(
        Employee.department,
        db.func.sum(Employee.salary)
    ).group_by(Employee.department).all()

    result = '<br>'.join([f'{dept}: {total}' for dept, total in totals])
    return result
```

#### 3. `AVG`

**Purpose**: Calculate the average value of a numeric column.

**Example**: Calculate the average salary of all employees.

```python
@app.route('/average_salary')
def average_salary():
    average = db.session.query(db.func.avg(Employee.salary)).scalar()
    return f'Average salary of all employees: {average}'
```

**Example**: Calculate the average salary by department.

```python
@app.route('/average_salary_by_department')
def average_salary_by_department():
    averages = db.session.query(
        Employee.department,
        db.func.avg(Employee.salary)
    ).group_by(Employee.department).all()

    result = '<br>'.join([f'{dept}: {avg}' for dept, avg in averages])
    return result
```

#### 4. `MAX`

**Purpose**: Find the maximum value in a column.

**Example**: Find the highest salary among all employees.

```python
@app.route('/highest_salary')
def highest_salary():
    max_salary = db.session.query(db.func.max(Employee.salary)).scalar()
    return f'Highest salary of all employees: {max_salary}'
```

**Example**: Find the highest salary by department.

```python
@app.route('/highest_salary_by_department')
def highest_salary_by_department():
    max_salaries = db.session.query(
        Employee.department,
        db.func.max(Employee.salary)
    ).group_by(Employee.department).all()

    result = '<br>'.join([f'{dept}: {max_salary}' for dept, max_salary in max_salaries])
    return result
```

#### 5. `MIN`

**Purpose**: Find the minimum value in a column.

**Example**: Find the lowest salary among all employees.

```python
@app.route('/lowest_salary')
def lowest_salary():
    min_salary = db.session.query(db.func.min(Employee.salary)).scalar()
    return f'Lowest salary of all employees: {min_salary}'
```

**Example**: Find the lowest salary by department.

```python
@app.route('/lowest_salary_by_department')
def lowest_salary_by_department():
    min_salaries = db.session.query(
        Employee.department,
        db.func.min(Employee.salary)
    ).group_by(Employee.department).all()

    result = '<br>'.join([f'{dept}: {min_salary}' for dept, min_salary in min_salaries])
    return result
```

### Summary

- **`COUNT`**: Use `db.func.count()` to count rows or group by specific columns.
- **`SUM`**: Use `db.func.sum()` to calculate the total sum of a column.
- **`AVG`**: Use `db.func.avg()` to find the average of a column.
- **`MAX`**: Use `db.func.max()` to find the maximum value in a column.
- **`MIN`**: Use `db.func.min()` to find the minimum value in a column.

These aggregation functions allow you to summarize and analyze your data efficiently using Flask-SQLAlchemy, making it easier to get insights from your database.