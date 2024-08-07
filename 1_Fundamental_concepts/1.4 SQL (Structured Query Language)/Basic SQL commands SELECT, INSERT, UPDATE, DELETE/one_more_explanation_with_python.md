Flask-SQLAlchemy is an extension for Flask that adds support for SQLAlchemy, a powerful Object Relational Mapper (ORM) for Python. SQLAlchemy allows you to interact with your database using Python classes and objects, rather than writing raw SQL queries. Here’s how you can use Flask-SQLAlchemy to perform basic database operations similar to the SQL commands you mentioned: `SELECT`, `INSERT`, `UPDATE`, and `DELETE`.

### 1. Setup

First, you need to set up Flask and Flask-SQLAlchemy:

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'  # Use your database URI here
db = SQLAlchemy(app)
```

### 2. Define a Model

Models in Flask-SQLAlchemy represent tables in your database. Here’s an example model for `Employee`:

```python
class Employee(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    first_name = db.Column(db.String(50), nullable=False)
    last_name = db.Column(db.String(50), nullable=False)
    department = db.Column(db.String(50))
    hire_date = db.Column(db.Date)

    def __repr__(self):
        return f'<Employee {self.first_name} {self.last_name}>'
```

### 3. Basic Operations

#### `SELECT` (Retrieve Data)

To retrieve data, you can use query methods provided by SQLAlchemy:

```python
# Get all employees
all_employees = Employee.query.all()

# Get a specific employee by ID
employee = Employee.query.get(1)

# Query employees by department
sales_employees = Employee.query.filter_by(department='Sales').all()
```

#### `INSERT` (Add New Records)

To insert a new record, create an instance of the model and add it to the session:

```python
new_employee = Employee(
    first_name='Emily',
    last_name='Clark',
    department='HR',
    hire_date='2024-08-07'
)

db.session.add(new_employee)
db.session.commit()
```

#### `UPDATE` (Modify Existing Records)

To update records, first query for the record, then modify its attributes and commit the changes:

```python
employee = Employee.query.get(2)  # Get employee with ID 2
if employee:
    employee.department = 'Marketing'
    db.session.commit()
```

#### `DELETE` (Remove Records)

To delete a record, query for it, then remove it from the session and commit:

```python
employee = Employee.query.get(4)  # Get employee with ID 4
if employee:
    db.session.delete(employee)
    db.session.commit()
```

### Full Example

Here’s how it all fits together in a simple Flask app:

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from datetime import datetime

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'
db = SQLAlchemy(app)

class Employee(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    first_name = db.Column(db.String(50), nullable=False)
    last_name = db.Column(db.String(50), nullable=False)
    department = db.Column(db.String(50))
    hire_date = db.Column(db.Date)

    def __repr__(self):
        return f'<Employee {self.first_name} {self.last_name}>'

@app.route('/')
def index():
    # Retrieve all employees
    employees = Employee.query.all()
    return '<br>'.join([f'{e.first_name} {e.last_name}' for e in employees])

@app.route('/add')
def add_employee():
    # Insert a new employee
    new_employee = Employee(
        first_name='Emily',
        last_name='Clark',
        department='HR',
        hire_date=datetime(2024, 8, 7)
    )
    db.session.add(new_employee)
    db.session.commit()
    return 'Employee added!'

@app.route('/update/<int:employee_id>')
def update_employee(employee_id):
    # Update an employee
    employee = Employee.query.get(employee_id)
    if employee:
        employee.department = 'Marketing'
        db.session.commit()
        return f'Employee {employee_id} updated!'
    return 'Employee not found!'

@app.route('/delete/<int:employee_id>')
def delete_employee(employee_id):
    # Delete an employee
    employee = Employee.query.get(employee_id)
    if employee:
        db.session.delete(employee)
        db.session.commit()
        return f'Employee {employee_id} deleted!'
    return 'Employee not found!'

if __name__ == '__main__':
    app.run(debug=True)
```

This Flask app demonstrates how to use Flask-SQLAlchemy to perform basic CRUD operations (Create, Read, Update, Delete) on a database.