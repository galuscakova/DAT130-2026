## Exercise A -- Anomalies

Insertion: cannot add a new course unless a student enrolls

Update: changing instructor office requires multiple rows

Deletion: deleting last student removes course info

Redundancy: instructor data repeated for each row

## Exercise B -- Keys

**Candidate key:** 

(student_id, course_id)

**Superkeys:**

(student_id, course_id, grade)
(student_id, course_id, instructor_id)

**Not a key**
student_id alone is not unique (student takes multiple courses)

## Exercise C -- Functional Dependencies

### C.1
student_id → student_name
course_id → course_title, instructor_id
instructor_id → instructor_name, instructor_office
(student_id, course_id) → grade

### C.2

**Trivial:**

(student_id, course_id) → student_id

**Non-trivial:**

student_id → student_name

**Semi-trivial:**

(student_id, course_id) → course_id, grade

## Exercise D -- 1NF

### D.1

Yes, atomic values → already 1NF.

### D.2

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50)
);
```

## Exercise E -- 2NF

### E.1

Partial dependencies:

student_id → student_name
course_id → course_title, instructor_id

Not fully dependent on whole key → violates 2NF.

### E.2

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100)
);

CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_title VARCHAR(100),
    instructor_id INT
);

CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    grade CHAR(2),
    PRIMARY KEY(student_id, course_id)
);
```

### Part F -- 3NF

### F.1

Transitive:

course_id → instructor_id
instructor_id → instructor_name, instructor_office

### F.2

3NF decomposition

```sql
CREATE TABLE instructors (
    instructor_id INT PRIMARY KEY,
    instructor_name VARCHAR(100),
    instructor_office VARCHAR(50)
);

CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_title VARCHAR(100),
    instructor_id INT,
    FOREIGN KEY (instructor_id) REFERENCES instructors(instructor_id)
);
```

## Exercise G -- Final Schema (BCNF)

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100)
);

CREATE TABLE instructors (
    instructor_id INT PRIMARY KEY,
    instructor_name VARCHAR(100),
    instructor_office VARCHAR(50)
);

CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_title VARCHAR(100),
    instructor_id INT,
    FOREIGN KEY (instructor_id) REFERENCES instructors(instructor_id)
);

CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    grade CHAR(2),
    PRIMARY KEY(student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```
