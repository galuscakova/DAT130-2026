## Exercise #2: Try it

2.1 Insert a new employee into the database.

```SQL
INSERT INTO employee VALUES (1250, 'William', 55.000, 1);
```

2.2 Write an update to change the department of employee Tom to 2.

```SQL
UPDATE employee SET departmentID=2 where name='Tom';
```

Note: to get this working in MySQL Workbench, you need to disable safe updates: https://stackoverflow.com/questions/33971357/update-query-not-working-in-mysql-workbench

2.3 DELETE employee Tom.

```SQL
DELETE FROM employee where name='Tom';
```

2.4 UPDATE the salary of all employees in department 0 with 5%.

```SQL
UPDATE employee
SET salary = salary + salary * 0.05
WHERE departmentID = 0;
```

2.5 Include an additional column in the department table, that shows the employeeId of a deparment manager.

```SQL
ALTER TABLE department
ADD managerID INT;
```

If we want to add the foreign reference key to the manager, we first need to be sure that employee.ID is a primary key in the employee table:

```SQL
ALTER TABLE employee
ADD PRIMARY KEY (ID);
```

And then we can add the foreign key constraint:
```SQL
ALTER TABLE department
ADD CONSTRAINT fk_department_manager
FOREIGN KEY (managerID)
REFERENCES employee(ID);
```
