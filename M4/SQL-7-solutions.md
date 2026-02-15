## Part A – Access Control

### Task A.1 – Create User

```sql
-- Create intern user with password, limited to localhost
CREATE USER 'intern'@'localhost' IDENTIFIED BY 'intern123';
```

You might need to change the validate_password.policy.\
Check the current value:
```sql
SHOW VARIABLES LIKE 'validate_password%';
```
and possibly change the value
```sql
SET GLOBAL validate_password.policy=LOW;
```

### Task A.2 – Grant Minimal Access

```sql
-- Grant SELECT privilege on students table only
GRANT SELECT ON unibook.students TO 'intern'@'localhost';
```

### Task A.3 – Verify Privileges

```sql
-- Show privileges for intern
SHOW GRANTS FOR 'intern'@'localhost';
```

Expected output:

```sql
GRANT USAGE ON *.* TO 'intern'@'localhost'
GRANT SELECT ON `unibook`.`students` TO 'intern'@'localhost'
```

### Task A.4 – Roles

```sql
-- Create role for teacher
CREATE ROLE teacher_role;

-- Grant privileges to role
GRANT SELECT ON unibook.students TO teacher_role;
GRANT SELECT ON unibook.enrollments TO teacher_role;

-- Create teacher1 user
CREATE USER 'teacher1'@'localhost' IDENTIFIED BY 'teacher123';

-- Assign role to user
GRANT teacher_role TO 'teacher1'@'localhost';

-- Set role as default
SET DEFAULT ROLE teacher_role TO 'teacher1'@'localhost';
```

### Task A.5 – Revoke

```sql
-- Remove SELECT privilege from intern
REVOKE SELECT ON unibook.students FROM 'intern'@'localhost';
```

## Part B – Views

### Task B.1 – Hide SSN

```sql
-- Create a public student view without SSN
CREATE VIEW student_public AS
SELECT id, name, program
FROM students;
```

### Task B.2 – Restrict Intern

```sql
-- Revoke access to base table
REVOKE ALL PRIVILEGES ON unibook.students FROM 'intern'@'localhost';

-- Grant access to view only
GRANT SELECT ON unibook.student_public TO 'intern'@'localhost';
```

### Task B.3 – Teacher-Specific View

```sql
-- View showing student name, course, grade for Dr. Hansen only
CREATE VIEW dr_hansen_grades AS
SELECT s.name AS student_name, c.name AS course_name, e.grade
FROM students s
JOIN enrollments e ON s.id = e.student_id
JOIN courses c ON e.course_id = c.id
WHERE c.teacher = 'Dr. Hansen';
```

### Part C – Conceptual Explanations

Indexes improve availability\
Indexes allow the database to quickly locate rows without scanning the entire table, reducing query time and improving system responsiveness, which indirectly supports availability under load.

Access control improves confidentiality\
By granting specific privileges (SELECT, UPDATE, etc.) only to authorized users and using roles or views, sensitive data is protected from unauthorized access, ensuring confidentiality.

Transactions improve integrity\
Transactions ensure that a series of operations either fully succeed or fully fail (ACID properties). This prevents partial updates that could leave data in an inconsistent state, maintaining data integrity.
