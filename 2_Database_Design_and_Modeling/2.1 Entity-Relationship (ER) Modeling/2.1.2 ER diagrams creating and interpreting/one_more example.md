
### 1. One-to-One (1:1) Relationship
**Example: Person and Passport**

**Entities:**
- **Person**
  - Attributes: PersonID (PK), Name, DateOfBirth
- **Passport**
  - Attributes: PassportID (PK), IssueDate, ExpirationDate

**Diagram:**
```
 +-----------------+       +-----------------+
 |     Person      |       |     Passport     |
 |-----------------|       |-----------------|
 | PersonID (PK)   |<----->| PassportID (PK)  |
 | Name            |       | IssueDate       |
 | DateOfBirth     |       | ExpirationDate   |
 +-----------------+       +-----------------+
```
**Constraint:** Each person can have only one passport and each passport belongs to only one person.

---

### 2. One-to-Many (1:N) Relationship
**Example: Author and Book**

**Entities:**
- **Author**
  - Attributes: AuthorID (PK), Name, Email
- **Book**
  - Attributes: BookID (PK), Title, Genre, AuthorID (FK)

**Diagram:**
```
 +-----------------+       +-----------------+
 |      Author     |       |      Book       |
 |-----------------|       |-----------------|
 | AuthorID (PK)   |<----- | BookID (PK)     |
 | Name            |       | Title           |
 | Email           |       | Genre           |
 +-----------------+       | AuthorID (FK)   |
                           +-----------------+
```
**Constraint:** An author can write many books, but each book is written by only one author.

---

### 3. Many-to-Many (M:N) Relationship
**Example: Student and Course**

**Entities:**
- **Student**
  - Attributes: StudentID (PK), Name, Major
- **Course**
  - Attributes: CourseID (PK), CourseName, Credits
- **Enrollment** (junction table to resolve M:N)
  - Attributes: StudentID (FK), CourseID (FK), Grade

**Diagram:**
```
 +-----------------+       +-----------------+       +-----------------+
 |     Student     |       |    Enrollment    |       |      Course     |
 |-----------------|       |-----------------|       |-----------------|
 | StudentID (PK)  |<----- | StudentID (FK)  |       | CourseID (PK)   |
 | Name            |       | CourseID (FK)   |<----- | CourseName      |
 | Major           |       | Grade           |       | Credits         |
 +-----------------+       +-----------------+       +-----------------+
```
**Constraints:**
- A student can enroll in many courses, and a course can have many students. 
- Each enrollment record links a specific student to a specific course.

---

### Summary of Relationships:
- **1:1**: Each entity instance in one set is related to exactly one entity instance in another set.
- **1:N**: An entity in one set can relate to many entities in another set, but those entities relate back to only one in the first set.
- **M:N**: Entities in both sets can relate to multiple instances in the other set, necessitating a junction table to manage the relationships.

