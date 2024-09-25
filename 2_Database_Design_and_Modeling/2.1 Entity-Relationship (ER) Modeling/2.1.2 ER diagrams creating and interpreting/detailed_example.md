
### 1. One-to-One (1:1) Relationship
**Example: Employee and Company Car**

**Entities:**
- **Employee**
  - Attributes: EmployeeID (PK), Name, Position
- **CompanyCar**
  - Attributes: CarID (PK), LicensePlate, Model

**Diagram:**
```
 +-----------------+       +-----------------+
 |    Employee     |       |   CompanyCar    |
 |-----------------|       |-----------------|
 | EmployeeID (PK) |<----->| CarID (PK)      |
 | Name            |       | LicensePlate     |
 | Position        |       | Model            |
 +-----------------+       +-----------------+
```
**Constraints:**
- Each employee is assigned exactly one company car, and each company car is assigned to only one employee.

---

### 2. One-to-Many (1:N) Relationship
**Example: Teacher and Class**

**Entities:**
- **Teacher**
  - Attributes: TeacherID (PK), Name, Subject
- **Class**
  - Attributes: ClassID (PK), ClassName, TeacherID (FK)

**Diagram:**
```
 +-----------------+       +-----------------+
 |     Teacher     |       |      Class      |
 |-----------------|       |-----------------|
 | TeacherID (PK)  |<----- | ClassID (PK)    |
 | Name            |       | ClassName       |
 | Subject         |       | TeacherID (FK)  |
 +-----------------+       +-----------------+
```
**Constraints:**
- A teacher can teach multiple classes, but each class is taught by only one teacher.

---

### 3. Many-to-Many (M:N) Relationship
**Example: Student and Course**

**Entities:**
- **Student**
  - Attributes: StudentID (PK), Name, Major
- **Course**
  - Attributes: CourseID (PK), CourseName, Credits
- **Enrollment** (junction table)
  - Attributes: StudentID (FK), CourseID (FK), Grade

**Diagram:**
```
 +-----------------+       +------------------+       +-----------------+
 |     Student     |       |    Enrollment     |       |      Course     |
 |-----------------|       |------------------|       |-----------------|
 | StudentID (PK)  |<----- | StudentID (FK)   |       | CourseID (PK)   |
 | Name            |       | CourseID (FK)    |<----- | CourseName      |
 | Major           |       | Grade            |       | Credits         |
 +-----------------+       +------------------+       +-----------------+
```
**Constraints:**
- A student can enroll in multiple courses, and each course can have multiple students.
- The `Grade` attribute in the `Enrollment` table tracks the grade a student receives in each course.

---

### 4. One-to-Many with Additional Constraints
**Example: Library and Book Copies**

**Entities:**
- **Library**
  - Attributes: LibraryID (PK), Name, Location
- **BookCopy**
  - Attributes: CopyID (PK), ISBN, LibraryID (FK), Status

**Diagram:**
```
 +-----------------+       +-----------------+
 |     Library     |       |   BookCopy      |
 |-----------------|       |-----------------|
 | LibraryID (PK)  |<----- | CopyID (PK)     |
 | Name            |       | ISBN            |
 | Location        |       | LibraryID (FK)  |
 +-----------------+       | Status          |
                           +-----------------+
```
**Constraints:**
- A library can have multiple copies of books, but each book copy belongs to only one library.
- The `Status` attribute indicates whether a book is "available," "checked out," or "reserved."

---

### 5. Many-to-Many with Additional Attributes
**Example: Project and Employee**

**Entities:**
- **Project**
  - Attributes: ProjectID (PK), ProjectName, Budget
- **Employee**
  - Attributes: EmployeeID (PK), Name, Role
- **ProjectAssignment** (junction table)
  - Attributes: ProjectID (FK), EmployeeID (FK), HoursWorked

**Diagram:**
```
 +-----------------+       +---------------------+       +-----------------+
 |     Project     |       |   ProjectAssignment  |       |    Employee     |
 |-----------------|       |---------------------|       |-----------------|
 | ProjectID (PK)  |<----- | ProjectID (FK)      |       | EmployeeID (PK) |
 | ProjectName     |       | EmployeeID (FK)     |<----- | Name            |
 | Budget          |       | HoursWorked         |       | Role            |
 +-----------------+       +---------------------+       +-----------------+
```
**Constraints:**
- A project can have multiple employees working on it, and an employee can work on multiple projects.
- The `HoursWorked` attribute in the `ProjectAssignment` table tracks how many hours each employee has dedicated to each project.

---
