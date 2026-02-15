## Setup 

Run First

```sql
USE unibook;

CREATE TABLE students (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100),
    ssn VARCHAR(20),
    program VARCHAR(100)
);

CREATE TABLE courses (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    teacher VARCHAR(100)
);

CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    grade CHAR(2),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(id),
    FOREIGN KEY (course_id) REFERENCES courses(id)
);

INSERT INTO students (name, email, ssn, program) VALUES
('Alice', 'alice@uni.no', '123-45-6789', 'Computer Science'),
('Bob', 'bob@uni.no', '987-65-4321', 'Data Science'),
('Charlie', 'charlie@uni.no', '111-22-3333', 'Cyber Security');

INSERT INTO courses (name, teacher) VALUES
('Databases', 'Dr. Hansen'),
('Web Programming', 'Dr. Olsen');

INSERT INTO enrollments VALUES
(1,1,'A'),
(2,1,'B'),
(3,2,'A');
```

## Part A – Access Control

⚠ These tasks must be run as a user with permission to create users (e.g. root).\
[Instructions](https://dev.mysql.com/doc/mysql-getting-started/en/)\
I will thus run the commands inside of the command line instead of MySQL Workbench.

### Task A.1 – Create User

Create user:

username: intern\
password: intern123

Limit access to localhost.

### Task A.2 – Grant Minimal Access

Grant intern permission to:

```sql
SELECT from students
```

Nothing else

### Task A.3 – Verify Privileges

Show the privileges of intern.

### Task A.4 – Roles

Create role teacher_role

Allow it to SELECT from:\
students\
enrollments

Create user teacher1\
Assign the role\
Set it as default

### Task A.5 – Revoke

Remove SELECT privilege on students from intern.

## Part B – Views 

### Task B.1 – Hide SSN

Create view for student_public

Columns: id, name, program

### Task B.2 – Restrict Intern

Revoke intern’s access to students table.\
Grant access only to the view.

### Task B.3 – Teacher-Specific View

Create a view that shows:
- student name
- course name
- grade

ONLY for courses taught by 'Dr. Hansen'.

## Part C

Explain how:
- Indexes improve availability
- Access control improves confidentiality
- Transactions improve integrity
