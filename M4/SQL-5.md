In this lab you will explore how indexes affect query performance in MySQL.

Run the following scripts first.

Create the tables:

```sql
DROP DATABASE IF EXISTS dat130_indexes;
CREATE DATABASE dat130_indexes;
USE dat130_indexes;

-- Users table (Instagram style)
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50),
    email VARCHAR(100),
    country VARCHAR(50),
    created_at DATE
);

-- Posts table
CREATE TABLE posts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    title VARCHAR(200),
    body TEXT,
    likes INT,
    created_at DATE,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Orders table (shop example)
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    amount DECIMAL(8,2),
    status VARCHAR(20),
    order_date DATE,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

Insert test data:

```sql
INSERT INTO users(username,email,country,created_at)
SELECT 
    CONCAT('user', x),
    CONCAT('user', x, '@mail.com'),
    ELT(1 + FLOOR(RAND()*5), 'Norway','Sweden','Denmark','Germany','France'),
    DATE('2020-01-01') + INTERVAL FLOOR(RAND()*1500) DAY
FROM (
    SELECT @row := @row + 1 AS x
    FROM information_schema.tables, (SELECT @row := 0) r
    LIMIT 20000
) t;


INSERT INTO posts(user_id,title,body,likes,created_at)
SELECT
    FLOOR(1 + RAND()*20000),
    CONCAT('Post about SQL ', x),
    'MySQL indexing is super important for performance',
    FLOOR(RAND()*500),
    DATE('2021-01-01') + INTERVAL FLOOR(RAND()*1200) DAY
FROM (
    SELECT @row2 := @row2 + 1 AS x
    FROM information_schema.tables, (SELECT @row2 := 0) r
    LIMIT 50000
) t;


INSERT INTO orders(user_id,amount,status,order_date)
SELECT
    FLOOR(1 + RAND()*20000),
    ROUND(RAND()*5000,2),
    ELT(1 + FLOOR(RAND()*3),'pending','paid','cancelled'),
    DATE('2022-01-01') + INTERVAL FLOOR(RAND()*900) DAY
FROM (
    SELECT @row3 := @row3 + 1 AS x
    FROM information_schema.tables, (SELECT @row3 := 0) r
    LIMIT 80000
) t;
```

## Exercise A -- No indexes

### A.1

Find a user by username:

```sql
SELECT * FROM users WHERE username = 'user15000';
```

Run EXPLAIN

How many rows are scanned?

### A.2

Find all users from Norway.

Run with EXPLAIN

Is an index used?

## Exercise B -- Single column indexes

### B.1

Create an index on username.

Run EXPLAIN again for A.1

What changed?

### B.2

Create an index on country.

Run A.2 again

Compare rows scanned before/after

## Exercise 3 -- Composite indexes

We often search:

```sql
SELECT * FROM orders 
WHERE user_id = 100 AND status = 'paid';
```
### 3.1
Run EXPLAIN without index

### 3.2
Create a composite index

Run EXPLAIN again

Which index order works best?

## Exercise 4 -- Range queries

Find expensive orders:

```sql
SELECT * FROM orders
WHERE amount BETWEEN 4000 AND 4500;
```

Does an index help?

Create one and test

## Exercise 5 -- FULLTEXT search

Search posts containing the word “performance”.

### 5.1
Try with LIKE

### 5.2

Create FULLTEXT index

Use MATCH ... AGAINST

Compare speed

## Exercise 6 -- Index inspection

Use:

```sql
SHOW INDEX FROM users;
```

Explain:

- Cardinality
- Key_len
- Non_unique

## Exercise 7 -- Query optimizer hints

Force MySQL to use an index even if it doesn’t want to.

Use:

- USE INDEX
- FORCE INDEX

Test on the orders table.

What differences do you observe?

# Exercise 8 -- Partitioning

Create a partitioned copy of orders by year of order_date using RANGE partitioning.

Then run

```sql
EXPLAIN SELECT * FROM orders_partitioned
WHERE order_date >= '2024-01-01';
```

Does MySQL scan all partitions or only one?
