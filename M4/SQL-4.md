In the exercisese, we will work with the following table:

```sql
DROP DATABASE IF EXISTS normalization_practice;
CREATE DATABASE normalization_practice;
USE normalization_practice;

CREATE TABLE enrollment_raw (
    student_id INT,
    student_name VARCHAR(100),
    course_id INT,
    course_title VARCHAR(100),
    instructor_id INT,
    instructor_name VARCHAR(100),
    instructor_office VARCHAR(50),
    grade CHAR(2)
);

INSERT INTO enrollment_raw VALUES
(1,'Alice Berg',101,'Databases',10,'Dr. Smith','B312','A'),
(2,'Bjorn Larsen',101,'Databases',10,'Dr. Smith','B312','B'),
(3,'Cecilie Nilsen',102,'Web Dev',11,'Dr. Jones','C210','A'),
(1,'Alice Berg',102,'Web Dev',11,'Dr. Jones','C210','B');
```

## Exercise A -- Anomalies

### A1
Learn about different anomalies which might occur in the databases if the schema is not normalized:
[anomalies link](https://www.geeksforgeeks.org/dbms/introduction-of-database-normalization/).

Then, find example for:
- one insertion anomaly
- one update anomaly
- one deletion anomaly
- one redundancy example

## Exercise B -- Keys

For the given table, answer following questions:
- What is a candidate key?
- Give two superkeys.
- Why is student_id alone not a key?

## Exercise C -- Functional Dependencies

### C.1

Provide 3 functional dependencies (example: student_id → student_name)

### C.2

Identify:
- trivial FD
- non-trivial FD
- semi-trivial FD

## Exercise D -- 1NF

### D.1

Does the current table violate 1NF? Why/why not?

### D.2

Let's add one more row to the table:
(1,'Alice,Berg',102,'Web Dev',11,'Dr. Jones','C210','B');

How would you redesign it to satisfy 1NF?
Write CREATE TABLE.

## Exercise E -- 2NF

### E.1

Is the table in 2NF? Explain which attributes have partial dependency.

### E.2

Decompose the table into relations that satisfy 2NF.

Write CREATE TABLE statements.

## Exercise F -- 3NF

### F.1

Identify transitive dependencies.

### F.2

Further decompose to reach 3NF.

Write CREATE TABLE statements.

## Part G -- BCNF

Check whether your 3NF design is already BCNF.
If not, fix it.

## Part H -- Implementation

Create the final normalized schema and insert the original data.

Write:

CREATE TABLEs

INSERT statements

Then verify with:

```sql
SELECT * FROM students;
SELECT * FROM courses;
SELECT * FROM instructors;
SELECT * FROM enrollments;
```
