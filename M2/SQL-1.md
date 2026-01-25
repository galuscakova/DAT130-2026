# SQL exercises, Part I. (create and select)

### Install and use MySQL Workbench
[Instructions for Windows](https://github.com/galuscakova/DAT130-2026/blob/bc32b2962ebb9055f136cdafb9d7b11929b0765a/M2/Installation-guides/Install%20MySQL%20for%20Windows.docx)

[Instructions for MacOS](https://github.com/galuscakova/DAT130-2026/blob/bc32b2962ebb9055f136cdafb9d7b11929b0765a/M2/Installation-guides/Install%20MySQL%20for%20macOS.docx)

[Instructions for Linux](https://github.com/galuscakova/DAT130-2026/blob/bc32b2962ebb9055f136cdafb9d7b11929b0765a/M2/Installation-guides/Install%20MySQL%20for%20Linux.docx)

For instructrions on how to use the MySQL Workbench, see https://www.youtube.com/watch?v=2mbHyB2VLYY

## Exercise #1: Create a DB and insert data

Create a new schema called myDB. Use this schema in the following commands.

``` sql
use myDB;
```

Create employee table

``` sql
CREATE TABLE employee (
   ID INT,
   name VARCHAR(50),
   salary INT,
   departmentID SMALLINT
);
```

Create department table

``` sql
CREATE TABLE department (
   ID INT,
   name VARCHAR(50)
);
```

Insert departments

``` sql
INSERT INTO department (ID, name) VALUES 
    (0, 'Engineering'),
    (1, 'HR'), 
    (2, 'Engineering');
```

Insert empoyees

``` sql
INSERT INTO employee (ID, name, salary, departmentID) 
VALUES 
    (1234, 'Tom', 50.000, 0),
    (1235, 'Bjørn', NULL, 1),
    (1345, 'Ida', 60.000,  0);
```

Check data empoyees

``` sql
SELECT * FROM department;
SELECT * FROM employee;
```

## Exercise #2: Try it 

    2.1 Insert a new employee into the database.

    2.2 Write an update to change the department of employee Tom to 2.

    2.3 DELETE employee Tom.

    2.4 UPDATE the salary of all employees in department 0 with 5%.

    2.5 Include an additional column in the department table, that shows the employeeId of a deparment manager.


Based on https://github.com/dat310-2025/info/blob/main/exercises/sql/basics
